# I Was Teaching My Son to Make Pancakes. I Didn't Realize I Was Teaching Him AI Architecture.

*A complete guide to the agentic loop — the mental model behind every AI agent that actually works in production.*

---

Last Sunday morning, my 10-year-old asked if he could make pancakes for the family.

I said yes, and stood next to him to teach.

What I didn't realize until later: every step I walked him through maps *exactly* to how a modern AI agent is architected. The tools, the decision points, the failure modes — it's all there, in a kitchen, with a bag of pancake mix and two eggs.

If you're trying to understand why AI agents break in production — or how to build ones that don't — this is the mental model you need.

---

## The Pancake World → Agentic System Map

Before we go any further, here's the full translation table. Keep this. It makes everything else click.

| Pancake World | Agentic System |
|---|---|
| My 10-year-old son | The AI Agent (Claude / GPT / your LLM) |
| Me (dad teaching) | The system prompt + instructions |
| "Make pancakes for a family of 4" | The task — the user's message |
| Mixer, pan, spatula | Tools the agent can call |
| Pancake mix, eggs, water | Input data and context |
| Skills: cracking, mixing, flipping | Tool functions — what each tool actually does |
| The recipe / SOP | The agent workflow |
| One pancake cycle | One loop iteration (one "turn") |
| "Flip when one side is solid" | The `stop_reason` check |
| "Enough pancakes for 4 people" | The termination condition — `stop_reason: "end_turn"` |
| Strict recipe vs. try-and-error | Deterministic pipeline vs. agentic loop |

Every element maps. This isn't a loose analogy — it's a precise one. Let me show you.

---

## The Agentic Loop — A Sunday Morning in Code

Here's what my son's pancake process actually looks like, step by step:

```
1. Read the recipe (system prompt)
2. Get the task: "Make enough for 4 people" (user message)
3. Mix batter          → tool call: mixer
4. Pour onto pan       → tool call: pan
5. Wait, check bottom  → is it solid? NO → wait more. YES → flip
6. Flip                → tool call: spatula
7. Wait, check top     → is it done? NO → wait more. YES → plate it
8. Count pancakes      → enough for 4? NO → back to step 3. YES → done ✓
```

That "enough for 4? → back to step 3" is the loop.

In software, it looks like this:

```python
import anthropic

client = anthropic.Anthropic()

def run_agent(user_message: str, tools: list, max_turns: int = 10) -> str:
    messages = [{"role": "user", "content": user_message}]
    # ^ "Make pancakes for a family of 4"

    for turn in range(max_turns):          # "max pancakes we'll ever make"

        response = client.messages.create(
            model="claude-sonnet-4-6",
            max_tokens=1024,
            tools=tools,
            messages=messages
        )

        if response.stop_reason == "end_turn":
            # ✓ "Enough pancakes. We're done."
            return next(b.text for b in response.content if hasattr(b, "text"))

        if response.stop_reason == "tool_use":
            # "The bottom is solid. Time to flip."
            # → execute the tool, feed result back, keep looping

            messages.append({"role": "assistant", "content": response.content})

            tool_results = []
            for block in response.content:
                if block.type == "tool_use":
                    result = execute_tool(block.name, block.input)
                    tool_results.append({
                        "type": "tool_result",
                        "tool_use_id": block.id,   # must match the request ID
                        "content": str(result)
                    })

            messages.append({"role": "user", "content": tool_results})
            # loop continues ↑

    return "max_turns reached — graceful exit"
```

Two signals. That's the whole engine:

- `stop_reason: "tool_use"` → *"The bottom is solid — time to flip. I need a tool result."* Keep looping.
- `stop_reason: "end_turn"` → *"Enough pancakes. I'm done."* Stop, return the answer.

Everything else — the complexity of real agents — is built on top of those two signals.

---

## The 3 Anti-Patterns That Kill Production Agents

Here's the part nobody talks about in tutorials. The loop is easy. What breaks agents isn't the loop — it's three specific failure modes. And every single one has a perfect pancake equivalent.

---

### Anti-Pattern #1: Nobody Told Him When to Stop

*(No `max_turns` — no exit condition)*

My son is making pancakes. He checks: does everyone have enough? He's not sure. He makes another one. And another. He runs out of eggs. He stares at an empty carton, confused, then looks at the empty bag of mix. He just... keeps trying to make more.

No one told him the number. So he loops forever.

**In production:** your tool throws an error. The agent calls it again. It errors again. The agent calls it again. Without a turn limit, this runs indefinitely — burning API tokens, stalling your system, never returning an answer.

**The fix:** always set `max_turns`. If the job isn't done in 10 turns, something went wrong upstream. Exit gracefully, log the state, alert.

```python
for turn in range(max_turns):      # ← this one line saves you from infinite loops
    ...

return "max_turns reached"         # ← graceful exit, never a hang
```

---

### Anti-Pattern #2: Flipping Before the Bottom Is Solid

*(Stopping on `tool_use` instead of waiting for `end_turn`)*

My son sees the pancake on the pan. He's impatient. He flips it before the bottom has set. The batter is still liquid inside. The pancake tears, sticks, falls apart. He stopped at the wrong signal.

`stop_reason: "tool_use"` is not the finish line. It's the pancake saying: *"I need to be flipped — I'm mid-process."*

**In production:** the agent calls a tool (searches the web, queries a database, runs a calculation). You read `stop_reason: "tool_use"` and break the loop — thinking it's done. But the agent was mid-task. It never got the tool result. It never finished the job.

**The fix:** only stop the loop on `end_turn`. `tool_use` means *keep going, execute this, feed it back.*

```python
if response.stop_reason == "end_turn":
    return response            # ← this is the only valid exit

if response.stop_reason == "tool_use":
    # execute tools, append results, continue
    # NEVER break here
```

---

### Anti-Pattern #3: The Eggshell Nobody Mentioned

*(Silent tool failure)*

My son cracks an egg. A piece of shell falls into the bowl. He sees it — but he doesn't say anything. He keeps mixing. I don't know. The batter looks fine.

Every pancake that comes out has a crunchy surprise. The family eats them. Nobody can figure out why the pancakes taste wrong.

**In production:** your tool throws an error. You catch the exception — but instead of passing the error back to the agent, you pass nothing. Or you pass `""`. Or you swallow the exception silently.

The agent has no idea anything went wrong. It keeps making decisions based on a tool result that doesn't exist — producing confident, fluent, completely wrong answers.

This is the most dangerous failure mode. It doesn't crash. It doesn't error. It just outputs nonsense with full confidence.

**The fix:** always return the error as a `tool_result`. Let the agent know the eggshell fell in so it can adapt.

```python
try:
    result = execute_tool(block.name, block.input)
except Exception as e:
    result = f"ERROR: {str(e)}"    # ← tell the agent what happened

tool_results.append({
    "type": "tool_result",
    "tool_use_id": block.id,
    "content": result              # ← agent gets the error and can adjust
})
```

An agent that knows something failed can ask for a different approach.
An agent that doesn't know has no chance.

---

## The `tool_choice` Gotcha — The One-Tool Kitchen

There's one more trap worth naming. It's subtle and it will burn you.

You hand my son the kitchen and give him instructions:

- **`auto`** — *"Use whatever tool you need."* He decides: fork for a small batch, mixer for a large one. He uses his judgment. ✓
- **`required`** — *"You must use a tool. I don't care which."* He picks one and uses it. ✓
- **`tool: "mixer"`** — *"Use the mixer. Specifically."* No choice, but clear. ✓

Now the trap:

**`required` + only one tool available.**

You tell him he must use a tool. The only tool in the kitchen is the mixer. He has to use the mixer. For everything. Cracking the egg: mixer. Checking if the pancake is done: mixer. Plating it: mixer.

He can't stop. He can't move on. Every single turn, he grabs the mixer. The loop never exits.

**In production:**

```python
# This creates an infinite loop:
response = client.messages.create(
    tools=[only_one_tool],                           # only one tool defined
    tool_choice={"type": "required"},                # MUST use a tool every turn
    ...
)
# Claude will call that tool every turn, forever
```

The fix: use `required` only when you have multiple tools and genuinely need Claude to call one. If you're forcing a specific tool, use `{"type": "tool", "name": "your_tool"}` — and make sure your workflow has a path to `end_turn`.

---

## Why This Mental Model Matters

The pancake loop isn't a dumbed-down version of the agentic loop. It *is* the agentic loop.

Every AI agent that works in production — whether it's a research assistant, a coding agent, a robotics controller, or a customer service bot — runs this same pattern:

1. Get the task
2. Work, calling tools as needed
3. Handle every tool result (including errors)
4. Know when to stop

The code is 20 lines.

The failure modes are what separate demos from production systems.

My son made 8 pancakes that morning. The family was fed. He checked that everyone had enough before he stopped. He fed back every result — including the one time he burnt a edge and had to adjust the heat.

That's a production agent.

---

## What's Next

I'm running **OmegaFounders** — a 12-day Silicon Valley cohort where founders build, certify, and ship real agentic AI systems.

Day 1 is exactly this: the agentic loop, the three anti-patterns, the `tool_choice` gotcha — hands-on, with working code you run yourself.

If you're a founder who wants to stop watching AI from the sidelines and start building — follow along. More coming daily.

*Drop a comment: which anti-pattern have you hit in the wild?*
