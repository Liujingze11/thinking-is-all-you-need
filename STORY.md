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

## 中文版

# Thinking Is All You Need：我给 Agent 加上"思考流程"后，准确率突然提升了

最近我在用 LangGraph 搭建一个 Agent，目标是让它根据用户任务，自动生成机器人可执行的任务指令。

一开始我以为，这会是一个典型的 Prompt Engineering 问题。于是我不断调提示词：加规则、加限制、加示例、加反例、加格式要求。

结果很残酷：**效果并没有明显提升。**

更奇怪的是，我甚至让另一个 Agent 去辅助分析错误，让它解释为什么生成错了、应该怎么改，结果也没有从根本上提高准确率。它能解释问题，但下一次还是可能犯类似的错。最后真正有效的方式，反而是最原始的方式：和人类对话，反复辩论，指出问题，修改逻辑。

这让我开始意识到一个问题：

> 也许不是 Agent 不知道规则，而是它缺少一个正确的思考过程。

## 问题不在"提示词不够强"，而在"思考顺序不对"

机器人任务指令生成不是普通文本生成。它不像写一段文案，只要语义通顺就可以。它更像是在生成一张流程图，里面有节点、有分支、有条件判断、有跳转、有返回。

任何一个小错误都会导致整个任务不可执行。这些问题靠"请你严格遵守规则"是很难解决的。因为 Agent 在生成指令时，本质上是在**边想边写**，按照自己的思考逻辑进行推理。

它不是先完整规划一遍，再严谨输出。很多时候，它是在生成过程中临时决定下一个节点是什么。于是看起来每一步都合理，但连起来就可能出错。这就像让一个人一边走迷宫一边画地图。走着走着，他可能忘了某个分支，也可能把两条路画到了一起。

## 灵感来自一次"Superpower 式"的头脑风暴

真正的转折点，发生在一个很普通的中午。我一边吃饭，一边还在想这个问题：为什么我已经给 Agent 加了这么多规则，它还是会在复杂任务里出错？

后来我突然想到，我平时和 Superpower 做头脑风暴时，效果之所以好，并不是因为它一上来就给我一个最终答案，而是它会先和我一起拆问题、找矛盾、推演可能性，再慢慢逼近一个更好的方案。

这给了我一个启发：

> 为什么不让 Agent 也像头脑风暴一样，先完成一轮结构化思考，再生成最终结果？

不是简单地告诉它"请仔细思考"。因为"仔细思考"本身太抽象了，模型并不知道到底该怎么仔细。真正有用的是给它一套**明确的思考路径**：先理解任务，再拆解结构，再推演分支，最后校验结果。

于是我决定不再继续堆提示词，而是给 Agent 增加一套前置思考流程。我想验证一件事：

> 如果模型自己不会稳定地产生正确的思考过程，那我能不能把这个过程**显式设计出来**？

## 我做的不是增加规则，而是给 Agent 设计一套思考流程

后来我意识到，单纯给模型加限制是没有意义的。因为限制只是在告诉它"不要犯错"，但没有告诉它"应该如何避免犯错"。于是我换了一种方式：不再要求 Agent 直接生成最终结果，而是让它在输出之前，先经过一套固定的思考流程。

**第一步：还原任务结构。** Agent 不能一上来就写结果，而是要先理解任务本身：用户真正想完成什么？任务中有哪些关键对象？它们之间是什么顺序、关系和依赖？哪些信息是明确的，哪些信息需要推断，哪些地方存在风险？这一步的目标不是生成答案，而是先把问题"看清楚"。

**第二步：把任务拆成可执行的中间计划。** 复杂任务最怕模型直接跳到最终答案。因为一旦它在中间漏掉某个环节，最终输出看起来可能还很完整，但实际逻辑已经错了。所以我要求 Agent 先生成一个中间规划，把任务拆成一个个可检查的步骤。这相当于让 Agent 先画出一张"草图"，而不是直接交付成品。

**第三步：沿着可能路径进行推演。** 很多错误不是在单个步骤里出现的，而是在多个步骤串起来之后才暴露出来。所以我让 Agent 不只是列计划，还要模拟执行这套计划：从起点开始，沿着不同可能性往下走，检查每一条路径是否合理，是否会断掉，是否会走向错误的结果。这一步本质上是在让模型做"预演"。

**第四步：用规则反向校验结果。** 最后，Agent 不能直接相信自己的规划，而是要回过头来逐条检查：有没有遗漏？有没有重复？有没有冲突？有没有看似合理但实际上不成立的地方？如果发现问题，就回到前面的规划里修正，而不是带着错误进入最终输出。因为模型最大的问题之一，就是它经常会**自信地输出一个错误答案**。而校验流程的意义，就是让它在交付之前，先对自己的结果做一次结构化审查。

## 结果：准确率明显提升

这套方法加进去之后，效果立刻不一样了。不是因为我写了更多限制条件，而是因为我改变了 Agent 的工作方式。

**以前它是：** `看到任务 → 直接生成结果 → 出错后解释原因`

**现在它变成了：** `看到任务 → 提取路径 → 规划节点 → 追踪分支 → 对照规则 → 修正草稿 → 输出结果`

这中间最大的变化是：我不再把准确率完全交给模型的"即时生成能力"，而是把任务拆成了一套**可验证的思考流程**。Prompt 不再只是"命令模型做什么"，而是变成了**"规定模型如何思考"**。

## 很多 Agent 失败，不是能力不够，而是没有思考流程

我们经常说大模型有推理能力，但在真实任务里，模型并不总是会自动选择正确的推理方式。尤其是小模型，或者复杂约束任务，它可能根本不会自发形成稳定的内部思考流程。它知道规则，也知道目标，但它不知道应该按什么顺序去拆解问题。就像一个新人执行任务，你只告诉他"不要出错"，其实没什么用。真正有用的是给他一套工作方法：先检查目标，再列步骤，再验证路径，最后交付结果。

## 从 LangGraph 到 Claude Code：把这种思想推广出去

这次经验让我想到，代码生成是不是也一样？我们用 Claude Code、Cursor 或其他 AI 编程工具时，经常会遇到类似问题：它能写代码，但容易改坏别的地方；它能修 bug，但可能引入新的 bug；它能理解需求，但实现路径不稳定。也许问题不是它不会写代码，而是它缺少一套固定的工程思考流程。

于是我和 Claude Code 一起写了一个项目：**[thinking-is-all-you-need](https://github.com/Liujingze11/thinking-is-all-you-need)**，它的核心思想很简单：

> 不要只优化模型输出，而要优化模型思考。

## Prompt Engineering 的下一步，可能不是更长的 Prompt

过去我们调 Prompt，总是在想怎么让模型"听话"。但我现在越来越觉得，真正重要的不是让模型记住更多规则，而是让它拥有更好的工作流。

- 规则是静态的。思考流程是动态的。
- 规则告诉模型什么不能做。流程告诉模型应该怎么做。

对于复杂任务来说，只靠规则堆叠，很容易让 Prompt 变得越来越长，模型却越来越混乱。因为规则之间可能冲突，优先级不清晰，验证方式也不明确。但如果给模型一套清晰的思考流程，它就能先拆解，再规划，再验证，最后输出。这才更接近人类解决问题的方式。

## 结语：Thinking Is All You Need

Agent 不是一个"更会写答案的模型"。Agent 应该是一个**"有工作方法的模型"**。

如果没有正确的思考流程，再强的模型也可能在复杂任务里犯低级错误。如果有了正确的思考流程，即使模型本身没有变，结果也可能大幅提升。

真正重要的是：**给 Agent 设计一套可执行、可验证、可迭代的思考过程。**

因为很多时候，答案不是最重要的。**如何思考，才是。**

---

⭐ [GitHub 仓库](https://github.com/Liujingze11/thinking-is-all-you-need) — 觉得有用的话，给孩子点点 Star 吧，求求了 🥺
