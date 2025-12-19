---
sidebar_position: 8
---

# Chapter 8: Advanced Features and Subagents

So far, our application has been built around a single, powerful AI model. But what happens when a task becomes too complex for a single line of thought? To build truly advanced applications, we need to move from a monolithic mindset to a modular one, employing a team of specialized "subagents" that work together. This chapter introduces the concept of reusable intelligence and showcases how to design a system of collaborating subagents.

## The Principle of Reusable Intelligence

Reusable intelligence is an architectural pattern where complex problems are broken down and delegated to specialized, independent agents. Instead of one "generalist" agent trying to do everything, you have a team of "specialist" subagents, each an expert in its domain.

A robust way to implement this is with a **Controller-Worker** architecture, much like the one described in the code snippets you've seen.

- **The `Task`**: A `Task` defines a specific job to be done, like "perform an advanced web search" or "evaluate the credibility of this text."
- **The `TaskWorker`**: A `TaskWorker` is a dedicated agent that knows how to perform a specific `Task`. It's a self-contained unit of intelligence.
- **The `TaskController`**: The `TaskController` is the orchestrator. It receives a high-level goal, breaks it down into tasks, and dispatches them to the appropriate available workers. It manages the whole process, collecting results and ensuring the final goal is met.

This design gives us incredible flexibility. We can add new capabilities simply by creating new `Task` and `TaskWorker` classes, and we can scale our application by running more workers.

```python
// A conceptual look at the TaskController's role
class TaskController:
    async def start_sample(self, data: StartSampleRequest):
        # 1. Identify the task type from the request
        task_name = data.name

        # 2. Find an available worker for that task
        target_worker = self.find_available_worker(task_name)
        if target_worker is None:
            raise HTTPException(406, "Error: No workers available")

        # 3. Assign the job to the selected worker
        result = await self._call_worker(
            task_name,
            target_worker.id,
            "/start_sample",
            {"index": data.index, ...},
        )
        return result
```

## A Team of Specialist Subagents

Let's explore some concrete examples of subagents we could build for our application. These are inspired by the powerful classes you've already seen.

### 1. The Advanced Search Subagent

When a simple database lookup isn't enough, we need an expert researcher. The `AdvancedSearchSystem` is a perfect blueprint for this subagent.

**Role**: To perform deep, multi-faceted searches to gather comprehensive information on a topic.

**Capabilities**:
- **Strategic Searching**: It's not just a simple search. It can employ multiple strategies, like `"source-based"`, `"parallel"`, or `"recursive-decomposition"`, depending on the query's nature. This is configured during its initialization.
- **Question Generation**: It can break a broad topic down into a series of specific questions to guide its search, ensuring thorough coverage.
- **Link and Finding Management**: It keeps track of all sources and findings, creating a well-documented research trail.

```python
# A subagent worker based on AdvancedSearchSystem
class AdvancedSearchWorker(Task):
    def __init__(self):
        # This worker specializes in source-based search
        self.search_system = AdvancedSearchSystem(strategy_name="source-based")
        super().__init__(name="advanced_search")

    async def start_sample(self, index: SampleIndex, session: Session):
        query = index.query # Assume the query is part of the sample index
        search_result = self.search_system.analyze_topic(query)
        # Return the structured findings
        return TaskSampleExecutionResult(result=search_result)
```

### 2. The Evidence Evaluator Subagent

Information is useless without trust. Once the search agent has gathered data, we need a skeptic to validate it. The `EvidenceEvaluator` is designed for this critical role.

**Role**: To assess the credibility and relevance of a piece of information against a specific claim or constraint.

**Capabilities**:
- **Evidence Extraction**: It can read a block of text and extract a specific `CLAIM`, its `SOURCE`, and a `CONFIDENCE` score.
- **Type Classification**: It categorizes evidence by type (e.g., `OFFICIAL_RECORD`, `NEWS_REPORT`, `SPECULATION`), which helps in weighing its importance.
- **Reasoning**: It provides a rationale for its confidence score, making its decisions transparent.

```python
# An example of how the EvidenceEvaluator would be used
class EvidenceEvaluator:
    def extract_evidence(self, search_result: str, candidate: str, constraint: Constraint) -> Evidence:
        prompt = f"""
Extract evidence regarding whether "{candidate}" satisfies this constraint:
Constraint: {constraint.description}
Search Results: {search_result}
...
"""
        response = self.model.invoke(prompt)
        # ... parsing logic ...
        return Evidence(...)
```

### 3. The Chat Orchestrator Subagent

Some tasks are not single actions but multi-step processes. The `AdvancedChatService` provides a model for a "meta-agent" or orchestrator that manages a complex conversational workflow.

**Role**: To manage the entire lifecycle of a user's request, from understanding it to generating, validating, and formatting the final answer.

**Capabilities**:
- **Requirement Enhancement**: It first uses an LLM to clarify and enrich the user's initial prompt, adding necessary context.
- **Iterative Refinement**: It can operate in a loop, generating a solution and then passing it to a validator. If the solution is invalid, it uses the feedback to try again.
- **Multi-Phase Processing**: It breaks the problem down into distinct phases: **Enhance -> Generate -> Validate -> Format**. This structured approach leads to more reliable and higher-quality outputs.

## Orchestration in Action: A Walkthrough

Imagine a user asks, "What are the most reliable, recently-published Python libraries for asynchronous web scraping, and what are their pros and cons?"

Here's how our team of subagents, managed by the `TaskController`, might handle it:

1.  **Request Ingestion**: The `TaskController` receives the request and sees that it's a complex conversational query. It assigns the main job to an `AdvancedChatService` worker.
2.  **Enhancement**: The `Chat Orchestrator` first enhances the query, perhaps clarifying "recently-published" to "published in the last 18 months" and "reliable" to "having good test coverage and active maintenance."
3.  **Delegation to Search**: The orchestrator now needs data. It sends a new task to the `TaskController`: `"advanced_search"`, with the query "asynchronous Python web scraping libraries published since late 2024."
4.  **Information Gathering**: The `TaskController` finds an available `AdvancedSearchWorker`. This worker executes its source-based strategy, finding blogs, tutorials, and GitHub repositories.
5.  **Validation**: For each library found (e.g., "ScrapeFlow," "AsyncWeb"), the `Chat Orchestrator` can delegate tasks to an `EvidenceEvaluator` worker. It asks it to verify claims like "Is ScrapeFlow actively maintained?" using the search results as context.
6.  **Synthesis and Generation**: Armed with a validated list of libraries and evidence of their pros and cons, the `Chat Orchestrator` synthesizes all the information. It constructs a final, detailed prompt for the `GeminiModel`.
7.  **Final Response**: The `GeminiModel` generates a well-structured answer comparing the libraries. The `Chat Orchestrator` performs a final formatting pass and returns the result to the user.

This modular, collaborative approach allows us to build far more capable and robust AI systems than we could with a single agent.

## What's Next?

This chapter concludes our primary journey through spec-driven development and advanced AI application design. You are now equipped with the principles and patterns to build sophisticated, modular, and intelligent systems. The path forward is one of continuous learning, experimentation, and refinement. Happy building!