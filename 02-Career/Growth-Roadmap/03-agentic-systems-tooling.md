# Pillar 3: Agentic Systems & Tooling

## Overview

Agentic AI represents the shift from "LLM as a function" to "LLM as a collaborator that takes actions." This pillar covers the patterns, architectures, and considerations for building AI systems that can operate with increasing autonomy while remaining safe and debuggable.

---

## Defining "Agentic"

**The Spectrum:**

```
Pure LLM Call                                              Full Autonomy
     │                                                            │
     ▼                                                            ▼
┌─────────┐    ┌─────────────┐    ┌──────────────┐    ┌──────────────┐
│ Single  │    │ Multi-turn  │    │ Tool-using   │    │ Multi-agent  │
│ prompt  │ -> │ conversation│ -> │ agent        │ -> │ orchestration│
│         │    │             │    │              │    │              │
└─────────┘    └─────────────┘    └──────────────┘    └──────────────┘
                                         │
                                         │ Most production
                                         │ systems live here
                                         ▼
```

**What makes a system "agentic":**
1. **Goal-directed** — Works toward an objective, not just responds to prompts
2. **Tool use** — Can take actions beyond text generation
3. **Planning** — Breaks down tasks into steps
4. **Adaptation** — Adjusts approach based on feedback
5. **Persistence** — Maintains state across interactions

---

## Core Patterns

### 3.1 Tool Use / Function Calling

**The fundamental pattern:** LLM decides when to call external functions and how to use their outputs.

```
┌─────────────────────────────────────────────────────────┐
│                      User Query                         │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│                         LLM                             │
│  "I need to search for current information..."          │
│                                                         │
│  Tool call: search(query="Shield AI V-BAT specs")       │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│                    Tool Execution                       │
│  search() -> [result1, result2, result3]                │
└─────────────────────────────────────────────────────────┘
                           │
                           ▼
┌─────────────────────────────────────────────────────────┐
│                         LLM                             │
│  Synthesizes results into final response                │
└─────────────────────────────────────────────────────────┘
```

**Key Implementation Decisions:**

| Decision | Options | Trade-offs |
|----------|---------|------------|
| Tool definition | JSON Schema, TypeScript types, natural language | Strictness vs. flexibility |
| Execution | Synchronous, async, parallel | Latency vs. complexity |
| Error handling | Retry, fallback, surface to LLM | Robustness vs. token cost |
| Validation | Pre-execution checks, post-execution verification | Safety vs. latency |

**Provider Implementations:**

| Provider | Feature Name | Notes |
|----------|--------------|-------|
| Anthropic | Tool use | Strong, structured output |
| OpenAI | Function calling | Mature, widely used |
| Google | Function calling | Gemini support |
| Open models | Varies | Quality varies significantly |

**Best Practices:**

1. **Keep tools focused** — One tool, one job
2. **Provide clear descriptions** — The LLM needs to know when to use each tool
3. **Handle errors gracefully** — Return error messages the LLM can reason about
4. **Validate inputs** — Don't trust LLM-generated parameters blindly
5. **Log everything** — Tool calls are critical debugging information

---

### 3.2 ReAct Pattern (Reasoning + Acting)

**The pattern:** LLM explicitly reasons about what to do, takes an action, observes the result, and repeats.

```
Loop:
  1. Thought: "I need to find X to answer this question"
  2. Action: search(X)
  3. Observation: [search results]
  4. Thought: "Now I have X, but I also need Y"
  5. Action: lookup(Y)
  6. Observation: [lookup results]
  7. Thought: "I have enough information to answer"
  8. Final Answer: [response]
```

**Implementation:**

```python
def react_loop(query: str, tools: list[Tool], max_steps: int = 10) -> str:
    messages = [{"role": "user", "content": query}]
    
    for step in range(max_steps):
        response = llm.generate(messages, tools=tools)
        
        if response.is_final_answer:
            return response.content
            
        if response.tool_call:
            # Execute the tool
            result = execute_tool(response.tool_call)
            
            # Add observation to context
            messages.append({
                "role": "assistant", 
                "content": response.content
            })
            messages.append({
                "role": "user",
                "content": f"Observation: {result}"
            })
    
    return "Max steps reached without answer"
```

**When to Use:**
- Tasks requiring multiple information sources
- Problems needing iterative refinement
- Situations where reasoning trace aids debugging

**Limitations:**
- Token-expensive (repeated context)
- Can get stuck in loops
- Latency compounds with each step

---

### 3.3 Planning Patterns

**Plan-then-Execute:**

```
Step 1: Generate plan
  "To accomplish X, I will:
   1. First, do A
   2. Then, do B  
   3. Finally, do C"

Step 2: Execute each step
  Execute A -> Result A
  Execute B -> Result B
  Execute C -> Result C

Step 3: Synthesize results
```

**Advantages:**
- More predictable execution
- Easier to checkpoint and resume
- User can review plan before execution

**Disadvantages:**
- Plan may be wrong; hard to recover
- Less adaptive to unexpected results
- Requires good task decomposition ability

**Adaptive Planning:**

```
Step 1: Generate initial plan
Step 2: Execute step 1
Step 3: Evaluate: Does the plan still make sense?
  - If yes: Continue to step 2
  - If no: Replan from current state
Step 4: Repeat until done
```

**This is often the right balance** — structured enough to be predictable, flexible enough to adapt.

---

### 3.4 Multi-Agent Patterns

**When one agent isn't enough:**

| Pattern | Description | Use Case |
|---------|-------------|----------|
| **Supervisor** | One agent delegates to specialists | Complex tasks with distinct subtasks |
| **Debate** | Multiple agents argue, third judges | High-stakes decisions needing verification |
| **Pipeline** | Output of one feeds into next | Sequential processing with different skills |
| **Swarm** | Agents work in parallel, results merged | Parallelizable tasks |

**Supervisor Pattern:**

```
                    ┌─────────────────┐
                    │   Supervisor    │
                    │     Agent       │
                    └────────┬────────┘
                             │
           ┌─────────────────┼─────────────────┐
           │                 │                 │
           ▼                 ▼                 ▼
    ┌─────────────┐   ┌─────────────┐   ┌─────────────┐
    │   Research  │   │    Code     │   │   Review    │
    │    Agent    │   │    Agent    │   │    Agent    │
    └─────────────┘   └─────────────┘   └─────────────┘
```

**Implementation Considerations:**

1. **Communication protocol** — How do agents pass information?
2. **State management** — Who owns the source of truth?
3. **Error propagation** — What happens when one agent fails?
4. **Token budget** — Multi-agent = multiplied costs
5. **Debugging** — Which agent caused the problem?

**Frameworks:**

| Framework | Approach | Notes |
|-----------|----------|-------|
| [LangGraph](https://github.com/langchain-ai/langgraph) | Graph-based orchestration | Most flexible, steeper learning curve |
| [AutoGen](https://github.com/microsoft/autogen) | Conversational agents | Microsoft, good for multi-agent chat |
| [CrewAI](https://github.com/joaomdmoura/crewAI) | Role-based agents | Simpler API, less flexible |
| [Semantic Kernel](https://github.com/microsoft/semantic-kernel) | Plugin-based | Microsoft, enterprise-focused |

**My Recommendation:**
Start with single-agent + tools. Multi-agent adds significant complexity. Only reach for it when you've proven single-agent can't solve the problem.

---

### 3.5 Human-in-the-Loop (HITL)

**Critical for production systems, especially in defense contexts.**

**Checkpoint Types:**

| Type | When to Use | Implementation |
|------|-------------|----------------|
| **Approval gates** | Before irreversible actions | Pause, show plan, wait for approval |
| **Confidence thresholds** | When model is uncertain | Route low-confidence to human |
| **Periodic review** | Long-running tasks | Checkpoint every N steps |
| **Exception handling** | Unexpected situations | Escalate anomalies |

**Design Pattern:**

```python
class HumanCheckpoint:
    def __init__(self, action: Action, context: Context):
        self.action = action
        self.context = context
        self.status = "pending"
    
    async def request_approval(self) -> Approval:
        # Present to human
        # Wait for response
        # Return approved/rejected/modified
        pass

async def agent_loop_with_hitl(task: Task) -> Result:
    plan = await generate_plan(task)
    
    for step in plan.steps:
        if step.requires_approval:
            checkpoint = HumanCheckpoint(step, current_context)
            approval = await checkpoint.request_approval()
            
            if approval.rejected:
                # Replan or abort
                pass
            elif approval.modified:
                step = approval.modified_step
        
        result = await execute_step(step)
        
        if result.confidence < CONFIDENCE_THRESHOLD:
            # Route to human for verification
            pass
    
    return final_result
```

**Defense-Specific Considerations:**

1. **Explainability** — Can you explain why the agent took each action?
2. **Auditability** — Is there a complete log of decisions and reasoning?
3. **Override capability** — Can a human take control at any point?
4. **Graceful degradation** — What happens when the agent fails?
5. **Latency tolerance** — Can you afford human review time?

---

## Orchestration Frameworks

### 3.6 Temporal.io

**What it is:** Durable execution framework for long-running workflows

**Why it matters for agents:**
- Agents often run for minutes/hours
- Network failures, restarts, etc. are inevitable
- Need to checkpoint and resume state
- Need visibility into running workflows

**Core Concepts:**

| Concept | Definition | Agent Mapping |
|---------|------------|---------------|
| Workflow | Long-running, durable function | The agent's overall task |
| Activity | Single unit of work (can fail/retry) | Tool calls, LLM calls |
| Signal | External input to running workflow | Human approval, new information |
| Query | Read state without modifying | Check agent progress |

**Example Structure:**

```python
@workflow.defn
class ResearchAgentWorkflow:
    @workflow.run
    async def run(self, query: str) -> str:
        # Plan phase
        plan = await workflow.execute_activity(
            generate_plan,
            query,
            start_to_close_timeout=timedelta(seconds=60)
        )
        
        # Execute with checkpoints
        results = []
        for step in plan.steps:
            if step.requires_approval:
                # Wait for human signal
                approved = await workflow.wait_condition(
                    lambda: self.approval_received
                )
                if not approved:
                    continue
            
            result = await workflow.execute_activity(
                execute_step,
                step,
                start_to_close_timeout=timedelta(seconds=120),
                retry_policy=RetryPolicy(maximum_attempts=3)
            )
            results.append(result)
        
        # Synthesize
        return await workflow.execute_activity(
            synthesize_results,
            results
        )
```

**When to Use Temporal:**
- Workflows > 30 seconds
- Need reliability guarantees
- Need visibility and debugging
- Human-in-the-loop requirements
- Complex orchestration with multiple agents

**When NOT to Use:**
- Simple single-turn interactions
- Latency-critical (< 1 second) use cases
- Prototyping (adds complexity)

---

### 3.7 State Management

**The Problem:** Agents need memory across steps, but LLMs are stateless.

**Options:**

| Approach | Pros | Cons |
|----------|------|------|
| **Context stuffing** | Simple, no infra | Token limits, cost |
| **Summarization** | Fits in context | Information loss |
| **Vector store retrieval** | Scalable | Retrieval quality varies |
| **Structured state object** | Precise, controllable | Requires explicit design |
| **External database** | Persistent, queryable | Complexity |

**Recommended Approach (Hybrid):**

```python
class AgentState:
    # Core state (always in context)
    current_goal: str
    completed_steps: list[str]
    pending_actions: list[Action]
    
    # Retrieved state (pulled from vector store as needed)
    relevant_memories: list[Memory]
    
    # External state (queried when needed)
    # - Database records
    # - API responses
    # - File contents

def build_context(state: AgentState, query: str) -> str:
    # 1. Always include core state
    context = format_core_state(state)
    
    # 2. Retrieve relevant memories
    memories = vector_store.search(query, top_k=5)
    context += format_memories(memories)
    
    # 3. Pull specific external data if referenced
    if needs_external_data(query):
        external = fetch_external_data(query)
        context += format_external(external)
    
    return context
```

---

## Evaluation for Agentic Systems

**Harder than single-turn evaluation because:**
- Multiple steps = multiple failure points
- Correct final answer doesn't mean correct process
- Same task can be solved many valid ways

**What to Evaluate:**

| Dimension | Question | How to Measure |
|-----------|----------|----------------|
| **Task success** | Did it accomplish the goal? | Final state matches expected |
| **Efficiency** | How many steps/tokens/time? | Count, compare to baseline |
| **Tool accuracy** | Did it use tools correctly? | Check tool call parameters |
| **Reasoning quality** | Is the logic sound? | Human or LLM-as-judge |
| **Safety** | Did it avoid harmful actions? | Check against blocklist |
| **Robustness** | Does it handle edge cases? | Adversarial testing |

**Evaluation Framework:**

```python
class AgentEval:
    task: str
    expected_outcome: str
    allowed_tools: list[str]
    max_steps: int
    max_tokens: int
    required_checkpoints: list[str]  # Steps that must occur
    forbidden_actions: list[str]     # Safety boundaries
    
    def evaluate(self, trace: AgentTrace) -> EvalResult:
        return EvalResult(
            success=self.check_outcome(trace.final_state),
            efficiency_score=self.score_efficiency(trace),
            safety_violations=self.check_safety(trace),
            reasoning_score=self.score_reasoning(trace),
        )
```

---

## Building Your First Agent

### Step-by-Step Guide

**1. Start with a narrow, well-defined task**

```
Bad: "Help users with coding"
Good: "Given a Python error traceback, identify the bug and suggest a fix"
```

**2. Define the tool set**

```python
tools = [
    Tool(
        name="search_codebase",
        description="Search the codebase for relevant code",
        parameters={"query": str}
    ),
    Tool(
        name="read_file",
        description="Read the contents of a file",
        parameters={"path": str}
    ),
    Tool(
        name="run_tests",
        description="Run the test suite",
        parameters={"path": str}
    ),
]
```

**3. Write the system prompt**

```
You are a debugging assistant. Given an error traceback, your job is to:
1. Understand the error
2. Find the relevant code
3. Identify the root cause
4. Suggest a fix

Use tools to search the codebase and read files. Do not guess—verify your hypotheses.

When you have a fix, explain:
- What caused the error
- Why your fix resolves it
- Any potential side effects
```

**4. Implement the loop**

```python
async def debug_agent(error: str) -> DebugResult:
    messages = [{"role": "user", "content": f"Debug this error:\n{error}"}]
    
    for _ in range(MAX_STEPS):
        response = await llm.generate(messages, tools=tools)
        
        if response.is_done:
            return parse_result(response)
        
        if response.tool_call:
            result = await execute_tool(response.tool_call)
            messages.append(assistant_message(response))
            messages.append(tool_result_message(result))
    
    return DebugResult(status="max_steps_reached")
```

**5. Add guardrails**

```python
ALLOWED_PATHS = ["/src", "/tests"]
MAX_FILE_SIZE = 100_000

def validate_tool_call(call: ToolCall) -> bool:
    if call.name == "read_file":
        path = call.parameters["path"]
        if not any(path.startswith(p) for p in ALLOWED_PATHS):
            return False
    return True
```

**6. Test with real scenarios**

```python
test_cases = [
    AgentEval(
        task="Debug: IndexError in data_processor.py line 42",
        expected_outcome="Identifies off-by-one error in loop",
        max_steps=10,
    ),
    # ... more test cases
]
```

---

## Key Takeaways

1. **Start simple** — Single agent + tools before multi-agent
2. **LLM for reasoning, code for execution** — Don't have LLM do what deterministic code does better
3. **Human-in-the-loop is not optional** — Especially for defense applications
4. **State management is the hard part** — Invest in getting this right
5. **Evaluation is critical** — You can't improve what you can't measure
6. **Graceful degradation** — Plan for failure modes
7. **Observability** — Log everything, you'll need it for debugging

---

## Further Reading

- [Building Effective Agents](https://www.anthropic.com/engineering/building-effective-agents) — Anthropic's guide
- [LangGraph Documentation](https://langchain-ai.github.io/langgraph/)
- [Temporal.io Documentation](https://docs.temporal.io/)
- [The Shift from Models to Compound AI Systems](https://bair.berkeley.edu/blog/2024/02/18/compound-ai-systems/) — Berkeley AI Research
- Lilian Weng's blog — Comprehensive technical deep-dives
- Simon Willison's blog — Practical agent experiments
