# Thinking Is All You Need: How Adding a "Thinking Workflow" to My Agent Suddenly Boosted Accuracy

I was building an Agent with LangGraph to automatically generate executable robot task instructions from user requests.

At first, I assumed it was a classic Prompt Engineering problem. So I kept tuning prompts: adding rules, constraints, examples, counterexamples, format requirements.

The result was brutal: **no meaningful improvement.**

Stranger still, I even had another Agent analyze the errors — to explain why the output was wrong and how it should be fixed. That didn't fundamentally improve accuracy either. It could explain the problem, but next time it would make a similar mistake.

What actually worked, in the end, was the most primitive approach: talking to a human, debating back and forth, pointing out flaws, revising the logic.

That's when I realized:

> Maybe the Agent doesn't lack rules. Maybe it lacks a correct thinking process.

---

## The Problem Isn't "Weak Prompts" — It's "Wrong Thinking Order"

Generating robot task instructions isn't like generating text. It's not about writing a paragraph that reads smoothly. It's more like drawing a flowchart — with nodes, branches, condition checks, jumps, and returns.

One small mistake makes the entire task unexecutable. These problems can't be solved by saying "please follow the rules strictly." Because when the Agent generates instructions, it's essentially **thinking while writing**, following its own reasoning logic.

It doesn't plan completely first, then output rigorously. Most of the time, it decides the next node on the fly. Each step looks reasonable in isolation, but chained together, things break.

It's like asking someone to draw a map while navigating a maze. Moving through, they might forget a branch, or merge two paths into one.

---

## The Insight Came From a "Superpower-Style" Brainstorm

The real turning point happened on an ordinary afternoon. I was eating lunch, still chewing on the question: why does the Agent keep making mistakes on complex tasks despite all the rules I've added?

Then it hit me. When I brainstorm with Superpower, the results are good not because it hands me a final answer immediately — but because it first deconstructs the problem with me, finds contradictions, explores possibilities, and gradually converges on a better solution.

That gave me the idea:

> Why not make the Agent go through a round of structured thinking first, just like brainstorming, before generating the final result?

Not just telling it "think carefully." "Think carefully" is too abstract — the model doesn't know what careful thinking actually looks like. What actually helps is giving it an **explicit thinking path**: understand the task first, then decompose the structure, then trace through branches, then verify the result.

This is a lot like how humans make complex decisions. Before we act, it looks like we move directly — but our brain has already completed countless judgments subconsciously: what's the goal, are the conditions met, what risks might appear, what's the next step, what to do if it fails.

A lot of what we call "smart" isn't about having better answers — it's about having a better thinking path.

So I decided to stop stacking prompts and instead give the Agent a **pre-output thinking workflow**. I wanted to test one thing:

> If the model can't reliably produce a correct thinking process on its own, can I **explicitly design one**?

---

## I Didn't Add More Rules — I Designed a Thinking Workflow

I realized that piling on constraints is pointless. Constraints only tell the model "don't make mistakes" — they don't tell it **how to avoid making them**.

So I changed my approach: instead of asking the Agent to directly produce the final result, I made it go through a fixed thinking workflow before output.

The workflow has four steps:

### Step 1: Reconstruct the Task Structure

The Agent can't jump straight to writing results. It has to understand the task first: what does the user actually want to accomplish? What are the key objects? What are their sequences, relationships, and dependencies? Which information is explicit, which needs inference, and where are the risks?

The goal of this step isn't to generate answers — it's to **see the problem clearly**.

### Step 2: Decompose Into an Executable Intermediate Plan

The most dangerous thing for complex tasks is the model jumping directly to the final answer. If it skips a step in the middle, the final output might still look complete — but the logic is already broken.

So I require the Agent to first produce an intermediate plan, breaking the task into checkable steps. Each step must explain: why it's needed, what it depends on, and what it leads to.

This is like asking the Agent to draw a **sketch** before delivering the finished product.

### Step 3: Trace Through Possible Paths

Many errors don't appear in individual steps — they only surface when multiple steps are chained together.

So I ask the Agent not just to list the plan, but to **simulate executing it**: start from the beginning, walk down different possibilities, check whether each path makes sense, whether it breaks, whether it leads to wrong results.

This step is essentially a **rehearsal**. Like how humans mentally run through scenarios before acting: what if this happens? What if that doesn't work? What comes next?

### Step 4: Verify Against Rules in Reverse

Finally, the Agent can't just trust its own plan. It has to go back and check item by item: any omissions? Any duplicates? Any conflicts? Anything that looks reasonable but doesn't actually hold up?

If problems are found, go back to the plan and fix them — don't carry errors into the final output.

This step is critical. Because one of the model's biggest flaws is that it will **confidently output a wrong answer**. The purpose of the verification workflow is to force a structured review before delivery.

---

## Result: Accuracy Jumped

Once this methodology was in place, the results changed immediately. Not because I wrote more constraints — but because I changed **how the Agent works**.

**Before:**

```
See task → Generate directly → Explain errors afterward
```

**After:**

```
See task → Extract paths → Plan nodes → Trace branches → Check rules → Revise draft → Output
```

The biggest change: I stopped betting accuracy entirely on the model's "real-time generation ability." Instead, I decomposed the task into a **verifiable thinking workflow**. The Prompt is no longer about "commanding the model what to do" — it's about **prescribing how the model should think**.

That's the real improvement.

---

## I'm Starting to Suspect: Many Agent Failures Aren't About Capability — They're About Missing Thinking Workflows

This experience made me think bigger.

We often say large models have reasoning ability. But in real-world tasks, models don't always automatically choose the correct reasoning approach.

Especially with smaller models, or tasks with complex constraints — the model may simply never form a stable internal thinking process on its own. It knows the rules, it knows the goal, but it doesn't know **in what order to decompose the problem**. Like a new employee: telling them "don't make mistakes" doesn't help. What helps is giving them a working method: check the goal first, then list steps, then verify the path, then deliver.

Humans are the same: before any action, our subconscious has already completed massive rehearsal. These thoughts happen so fast we think we're "acting directly." Many Agents don't have this subconscious — so we have to write it out explicitly.

---

## From LangGraph to Claude Code: Spreading the Idea

This experience made me wonder: isn't code generation the same? When we use Claude Code, Cursor, or other AI coding tools, we often run into similar problems:

- It can write code, but easily breaks other things
- It can fix bugs, but might introduce new ones
- It can understand requirements, but the implementation path is unstable

Maybe the problem isn't that it can't code — it's that it lacks a fixed engineering thinking workflow.

Before touching any code, shouldn't it first:

```
Understand goal → Locate relevant files → Infer impact scope → Design change plan → List test paths → Then actually modify code
```

If robot task instruction generation can improve accuracy through a "thinking scaffold," AI coding should be able to as well.

So Claude Code and I built a project together: **[thinking-is-all-you-need](https://github.com/Liujingze11/thinking-is-all-you-need)**

Its core idea is simple:

> Don't optimize model output. Optimize model thinking.

---

## The Next Step of Prompt Engineering Might Not Be Longer Prompts

We used to tune prompts by thinking about how to make the model "obedient."

But I increasingly believe the real lever isn't making the model memorize more rules — it's giving it a better workflow.

- Rules are static. Thinking workflows are dynamic.
- Rules tell the model what not to do. Workflows tell the model how to do it.

These are fundamentally different.

For complex tasks, stacking rules easily leads to longer and longer prompts while the model gets more and more confused — because rules can conflict, priorities are unclear, and verification is ambiguous.

But if you give the model a clear thinking workflow, it can first decompose, then plan, then verify, then output. That's much closer to how humans solve problems.

---

## Conclusion: Thinking Is All You Need

Building robot task instruction generation with LangGraph made me rethink what an Agent is.

An Agent isn't "a model that writes better answers." An Agent should be **a model with a working method.**

Without the right thinking workflow, even the strongest model can make basic mistakes on complex tasks. With the right thinking workflow, results can dramatically improve — even if the model itself hasn't changed.

So I increasingly believe: the key to optimizing Agents in the future isn't just model capability, tool calling, or Prompt tricks.

What really matters is:

> **Designing an executable, verifiable, and iterable thinking process for Agents.**

That's why I built thinking-is-all-you-need.

Because most of the time, the answer isn't the most important thing. **How you think — that's what matters.**

---

⭐ [GitHub Repo](https://github.com/Liujingze11/thinking-is-all-you-need) — If you found this useful, a Star would mean the world 🥺

---

> 📖 [阅读中文版 (Read in Chinese) →](STORY.zh-CN.md)
