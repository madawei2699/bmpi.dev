---
title: "Designing Agent-Friendly Workflows"
date: 2026-06-14
draft: false
tags: ["AI Native", "Agent", "Workflow", "DSL", "DAG", "Validation", "MinePilot", "MyInvestPilot"]
keywords: "Agent-friendly, AI Native, DSL, DAG, Validation, Compiler, Repair Loop"
description: "A chat box alone does not solve complex problems. Through MyInvestPilot and MinePilot, this article explores a workflow design pattern for AI agents: Domain DSLs, IR/DAG pipelines, validation contracts, and repair loops."
isMermaidEnabled: true
categories: [
    "Building & Practice"
]
---

Some time ago, I was thinking about how to enable agents to execute tasks stably in complex virtual environments. During this time, I stumbled upon an open-source project called [mindcraft](https://github.com/mindcraft-bots/mindcraft). The project uses a harness around Minecraft LAN mode to let an LLM-powered bot join the game world. You can issue commands to it directly in the game using natural language—for instance, "build a house"—and the bot will start placing blocks in the world.

This demonstration was fascinating, but when I tried it myself, I found the bot's stability to be less than ideal. It often got stuck in place; even when building a very small house, it frequently stalled halfway through. The causes might stem from environmental perception, action execution, pathfinding, or context maintenance. But the core issue is this: relying on an LLM to perform real-time, closed-loop spatial control in a game makes stability extremely difficult to guarantee.

I started researching the underlying reasons. To have an LLM place blocks piece by piece through "imperative" steps requires the model to precisely understand 3D spatial coordinates within the game while simultaneously processing a large amount of environmental context. This demands that the model maintain spatial state over long periods, understand environmental feedback, and continuously perform precise coordinate reasoning. For current LLMs, such closed-loop control tasks are highly susceptible to accumulating errors.

So I started looking for a different approach. If real-time building in-game is too prone to freezing, could I have the LLM generate a building schematic outside the game and then automatically load it into the game via mods? Following this train of thought, I discovered WorldEdit and the Minecraft building toolchain surrounding the schematic ecosystem. Consequently, my core approach underwent a shift: **How can we use LLMs to generate `.schem` schematic files that can be loaded by WorldEdit or related tools?**

This also became the direct starting point for my side project, [MinePilot](https://www.myminepilot.com/).

However, this still didn't solve the fundamental problem. Blueprint formats like `.schem` are essentially collections of low-level block states: every position must ultimately correspond to a specific block state. Asking an LLM to directly generate such low-level voxel data still easily results in walls with holes, misaligned roofs, and broken stairs.

At this point, I recalled a similar issue I encountered while developing the quantitative investment system [MyInvestPilot](https://www.myinvestpilot.com/). As I mentioned in the article [*How I Built an AI-Native Quantitative Investment System*](/en/dev/ai-native-investment-system/), to prevent the LLM from writing Python trading code with look-ahead bias, I introduced a "Strategy Primitives Engine" paradigm. Instead of having the LLM directly generate low-level logic code, I provided a Directed Acyclic Graph (DAG) based on a Domain-Specific Language (DSL). This creates a constrained space where the LLM can express its architectural intent.

The pattern turned out to be more general than investing. It can be applied to other domains where agents need to operate on complex, verifiable workflows. Essentially, it is a reusable workflow pattern for Agent-friendly systems: a workflow surface that allows an agent to read, write, validate, execute, observe, and repair tasks.

Currently, I am practicing this workflow primarily on MyInvestPilot and MinePilot—two projects that seemingly have nothing in common. After continuously refactoring both, I discovered that this architecture is actually running on the exact same underlying logic.

## The Leap from Low-Level APIs to Domain DSL

Many AI product developers, upon realizing that "letting AI write free-form code is unreliable," often swing to the opposite extreme: providing extremely granular, structured APIs.

For example, in quantitative investing, a common approach is to require the AI to return a deeply nested JSON tree to represent a strategy; in Minecraft, another common approach is to restrict the AI to only calling a `placeBlock(x, y, z, type)` API.

These approaches do not scale well. Once the strategy becomes slightly complex (e.g., "Buy when QQQ relative strength is >101, sell when <99 and confirmed for two consecutive days"), the parameter schema quickly becomes unwieldy. When generating these deeply nested structures, LLMs are very prone to omitting fields or using the wrong types. Asking an LLM to manually write the placement coordinates for tens of thousands of blocks is similar; it directly consumes its reasoning window on "spatial planning," wasting it on low-level arithmetic calculations.

### Consolidating Low-Level Control

A better solution is: **Consolidate the Agent's low-level control by designing a Domain-Specific Language (Domain DSL) specifically for it.**

In the MyInvestPilot quantitative system, I didn't let the LLM write low-level calculation logic. Instead, I abstracted a set of "Strategy Primitives." These primitives include moving averages (EMA), logical predicates (GreaterThan/LessThan), state delays (Lag), and consecutive state confirmations (Streak). The LLM only needs to combine these primitives like building blocks. The underlying Pandas operations, time alignment, and guardrails against look-ahead bias are all handled by the system.

In MinePilot's underlying engine, [CraftDAG](https://github.com/i365dev/CraftDAG), I did the exact same thing. I designed a DSL called `ComponentPlan`.

At the ComponentPlan layer, the LLM neither needs to nor should directly output low-level block coordinates. The LLM only needs to write high-level component planning. For instance, the LLM only needs to generate a JSON intent like this:
> "Create a 16x12x16 `RoomShell`. On the `front` wall, at `offset: 3`, install a `Door`. Place a `GableRoof` on top, and set `overhang: 1`."

### A "Semantic Scaffold" for Agents

Through this DSL, the LLM no longer needs to calculate "the coordinates of this wall are from (10, 5, 20) to (10, 10, 20)," nor does it need to compute "after the door is carved out, it needs to be replaced with air blocks." It utilizes high-level semantic attributes like `anchor`, `wall`, `offset`, and `overhang`.

The essence of a DSL is to build a "semantic scaffold" for the LLM. All low-level geometry, mathematical calculations, and engineering constraints are pushed down into the engine. The agent's cognitive load is vastly reduced, allowing it to invest its limited context window and reasoning budget into what it does best: high-level structure and logical reasoning.

The core principle of this phase is: **Agent is the author. Engine is the compiler.**

## Building Domain Engines Like Compilers

Since the LLM generates high-level DSL (intent), how should the system execute it? This brings us to the heaviest layer of the entire Agent-friendly workflow: Intermediate Representation (IR) and compiler design.

When the LLM hands over a `ComponentPlan` (Minecraft building plan) or a `Strategy primitives` map (quantitative strategy), this is merely an intermediate state between human intent and machine execution. It cannot be directly used to build a house, nor can it be directly used to backtest an investment portfolio.

You need to build your domain engine much like you would build a compiler for a programming language.

### Layered Compilation Pipeline

In CraftDAG, the building generation process is designed as a relatively strict compilation pipeline:
1. **Input Layer (BuildIntent)**: The user's natural language intent.
2. **Domain Plan Layer (ComponentPlan DSL)**: The building component blueprint generated by the LLM based on the intent.
3. **Intermediate Representation Layer (CraftDAG IR)**: The engine receives the ComponentPlan, parses macroscopic and local dependencies, and unfolds it into a verifiable, sortable Directed Acyclic Graph (DAG). Here, high-level semantics are lowered into deterministic bounding-box calculations and attachment relationships.
4. **Target Representation Layer (VoxelPlan)**: The compiler goes further down, computing the IR into the final 3D voxel collection (absolute block coordinates and states), generating a schematic export, or serving it directly for frontend/backend rendering.

Similarly, in MyInvestPilot, the "strategy primitives" assembled by the AI are also converted into a DAG by the engine. Through Topological Sorting, it decides which moving average indicators to compute first, followed by which comparison logics, before finally aggregating them into a trading signal. You can intuitively experience this by checking out the [MyInvestPilot Primitives Visual Editor](https://www.myinvestpilot.com/primitives-editor).

### Why Take the Compilation Route?

Initially, I felt that introducing concepts like "compilers" and "abstract syntax trees" to an application was over-engineering. However, subsequent iterations proved that this is an effective way to support the stable operation of Agent systems.

Only by lowering intents into a Directed Acyclic Graph (IR/DAG) does the system gain powerful **static analysis** and **deterministic execution** capabilities.
- In quantitative systems, having a DAG allows the system to cleanly trace signal dependencies, ensuring that no single component peeks at future data.
- In the Minecraft engine, having an IR allows the system to calculate extremely precise Material Lists. In theory, this IR layering also lays the groundwork for future incremental recalculation: when a user only modifies the roof material, the system only needs to reprocess the relevant sub-graph instead of having the LLM regenerate the entire building.

More importantly, having this compilation mechanism makes the next step—intercepting agent hallucinations—possible.

## Machine-Readable Contracts and Repair Loops

We have long been bound by the interactive inertia of Graphical User Interfaces (GUIs). In traditional software, if a user fills out a form incorrectly, the system pops up a red box or toast saying: "Invalid input format." The user sees the red box and relies on life experience to re-enter it.

However, if you merely return a simple `Invalid plan` or `Syntax Error` to an LLM, its repair efficiency will be remarkably low. It often relies on guessing to rewrite the entire JSON, and sometimes fixing one error inadvertently triggers a new one.

In an Agent-friendly system, **errors are not merely messages for humans. They become a strict, machine-readable Validation Contract between the system and the model.**

### Errors Are Not for Humans

While developing CraftDAG, I specifically designed a comprehensive, strongly-typed error feedback mechanism for the system. If the LLM, while generating the DSL, accidentally places a tower component's coordinates outside the global boundaries set for the building, the system's compiler will intercept it at the first line of defense and throw a precise JSON error:

```json
{
  "stage": "component-validation",
  "code": "ASSEMBLY_INSTANCE_OUT_OF_BOUNDS",
  "path": "instances[0].anchor",
  "componentId": "northwest_tower",
  "repairHint": "Move the instance inward or increase global bounds."
}
```

As you can see, this error is absolutely not meant for a human to read. It explicitly tells the LLM:
1. **stage**: The error occurred during component validation.
2. **code**: An out-of-bounds error.
3. **path**: The specific JSON path is the anchor of the first instance.
4. **repairHint**: Directly provides specific repair suggestions (move the anchor inward, or enlarge the global bounds).

### The LLM Collaboration Contract and Repair Loop

Besides precise post-validation errors, pre-authoring constraints are equally important.

In the CraftDAG project, there is a core file named `LLM_AUTHORING_CONTRACT.md`. This is not an API document for human developers in the traditional sense; rather, it is an authoring contract that can be read by LLMs or harness agents, and can also serve as part of a System Prompt or `llm.txt`.

Coincidentally, I did something similar in MyInvestPilot. To allow the Agent to independently develop strategy primitives, the system specifically provides a [machine-readable development document](https://www.myinvestpilot.com/docs/primitives/ai-assisted-development), and uses [llm-quickstart.txt](https://www.myinvestpilot.com/docs/primitives/_llm/llm-quickstart.txt) to provide the LLM with compact context and Few-shot examples. Furthermore, the engine exposes a strict [JSON Schema](https://media.i365.tech/myinvestpilot/primitives_schema.json), allowing the LLM to adhere to a deterministic, strongly-typed structural contract when generating the DSL.

In designing these contracts, a profound lesson I learned is: **Instead of telling the LLM "what you should do," it is better to directly set ABSOLUTE PROHIBITIONS.**
Do not attempt to exhaust all the creative possibilities an LLM might exercise; instead, draw absolute red lines. For instance, explicitly forbid inline nesting of other components within the `inputs` of a component; one must define the `id` first and reference it via `ref`.

Only when a system possesses both "strict pre-contracts" and "precise post-errors" does a crucial change occur: **The LLM begins to possess a much more stable self-repair path.**

When the LLM receives the aforementioned JSON error, it will instantly understand that something went wrong with `northwest_tower`. In the next iteration, it will only correct the coordinates of that specific component, and then submit it for compilation again. This forms a stable, convergent **Repair Loop**. Without this loop, an LLM is merely an occasionally useful toy; with this loop, the LLM becomes part of a more reliable production workflow.

## Handling the Boundary Between Ambiguity and Determinism

Having built the underlying engine, the final question remains: How should the user interact with this complex system?

Users won't write JSON DSLs, nor will they understand DAG diagrams. They will continue to ask natural language questions: "Help me adjust the risk of this portfolio," or "Add two larger windows to this castle."

This highlights a core architectural contradiction: user intents are often **ambiguous**, while the underlying financial calculations and spatial geometry are **deterministic and correctness-sensitive**. If you try to make an LLM handle both ends simultaneously, the system is quickly pulled in two incompatible directions: open-ended reasoning and rigorous computation.

### Separating Duties Between Local and Remote

In my architectural vision for such systems, the ideal approach is a **Hybrid Agent Architecture**. Future systems like MinePilot could adopt this split, enacting a hard separation of Agent duties:
* **Orchestrator/Local Agent**: Runs on the interaction layer closest to the user (e.g., frontend browser). It is essentially an advanced router and state machine. Its sole responsibility is handling **Ambiguity**. It chats with the user, infers user intent, and triggers explicit forms for user confirmation when necessary.
* **Processor/Remote Engine**: Runs in the backend asynchronous task queue. After the Local Agent clarifies the ambiguous intent, it sends the task to the remote engine as a structured job. The remote processor takes over the DSL, runs the compiler primitives, and executes the relatively complex deterministic validation, compilation, and calculation.

Queues, state machines, and compilation validations—things involving deterministic precision—must be implemented via traditional code. Conversely, intent guessing, error text explanation, and natural language translation—probabilistic tasks—should be fully delegated to LLMs.

**Mixing the deterministic layer with the probabilistic layer is an architectural mistake that many Agent systems are prone to making.**

## The Boundaries of This Paradigm

It must be emphasized that the Agent-friendly workflow is not a universal solution for all AI products. It is better suited for domains with the following characteristics:
1. Clear domain structures;
2. Stable primitives or components that can be abstracted;
3. Intermediate states that can be validated;
4. Final results that can be executed, compiled, or simulated;
5. Errors that can be localized and repaired;
6. Human oversight and decision-making is still possible.

If a task is inherently highly open-ended creative expression or lacks clear validation standards, prematurely introducing a DSL and DAG might inadvertently restrict the model's expressive capabilities. At the same time, more DSLs are not necessarily better; the real difficulty lies in finding a set of domain primitives that is small enough, orthogonal enough, yet capable of covering the primary tasks.

Even in suitable domains, Validation does not equate to correctness. An investment strategy passing a backtest does not mean it will be profitable in the future; a building blueprint passing compilation does not mean it is aesthetically pleasing. The value of a validation contract is narrowing the error space down to a manageable scope for the system, rather than replacing ultimate human judgment. Agent-friendly is not a substitute for Human-friendly; the more rational form is a dual interface: humans express intent, review results, and make decisions via UI; agents operate the system via DSL, tools, and validation contracts.

## Inverting the Software Architecture Perspective

Looking back at MyInvestPilot and MinePilot, one is serious financial investment, the other an open sandbox game. They are worlds apart in business logic, yet during the continuous process of development and refactoring, they naturally converged onto the exact same system paradigm:

`Intent` → `Domain DSL` → `Compiler (IR/DAG)` → `Validation Contract` → `Repair Loop` → `Execution`

Over the past few decades, the standard process for designing software has been: first draw the prototype interface for human interaction, then design the underlying database table structures, and finally write the intermediate glue APIs to connect them.

However, as AI agents begin to participate in more complex tasks, the order of software design may need to be rethought:

1. Ask yourself first: Can this complex business be abstracted into a Domain DSL that is sufficiently restrained toward agents and can consolidate low-level control?
2. Then ask yourself: Does the system have a robust compiler core capable of lowering the DSL into deterministic Directed Acyclic Graphs, providing precise machine-validation contracts?
3. Next ask yourself: When the model makes a mistake, can your system provide machine-readable error feedback to establish a closed-loop repair cycle?
4. Finally: It is only upon the foundation of this relatively stable workflow that we wrap a layer of interactive interfaces for human supervision and confirmation.

For now, I refer to this paradigm as: **Domain-shaped, not Domain-locked**.

Reshaping complex domain expertise into workflows that are readable, writable, verifiable, and iterable for agents might just be the key to the next generation of software architecture.

---

**Related Explorations & Resources:**
- [i365.tech: My AI-Native Systems and Product Laboratory](https://www.i365.tech/)
- Deep dive into the underlying design of the quantitative system: [*How I Built an AI-Native Quantitative Investment System*](/en/dev/ai-native-investment-system/)
- [MinePilot Official Website](https://www.myminepilot.com/)
- [CraftDAG Source Code (GitHub)](https://github.com/i365dev/CraftDAG)
