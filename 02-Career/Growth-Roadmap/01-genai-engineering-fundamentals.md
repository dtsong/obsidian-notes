# Pillar 1: GenAI Engineering Fundamentals

## Overview

This pillar covers the foundational knowledge required to build production-grade generative AI systems. The goal is to move beyond "prompting" as a skill and understand the engineering discipline behind reliable, cost-effective, and maintainable AI systems.

---

## Core Competencies

### 1.1 Prompt Engineering (Beyond Basics)

**Why it matters:** Prompt engineering is often dismissed as "just writing instructions," but at production scale, the difference between a good and great prompt is measured in cost, latency, reliability, and user satisfaction.

#### Techniques to Master

| Technique | When to Use | Trade-offs |
|-----------|-------------|------------|
| Zero-shot | Simple, well-defined tasks | May lack consistency |
| Few-shot | Tasks requiring specific format/style | Token cost, example selection matters |
| Chain-of-thought (CoT) | Complex reasoning, math, logic | Increased latency and cost |
| Self-consistency | High-stakes decisions | Multiple calls = higher cost |
| Tree-of-thought | Exploration problems | Significant complexity |
| Structured output (JSON mode) | API integrations, data extraction | Model-dependent support |
| System prompts | Persona, constraints, formatting | Can be overridden by user input |

#### Key Principles

1. **Be explicit over implicit** — Models don't infer intent well; spell out exactly what you want
2. **Constrain the output space** — Smaller valid output set = more reliable results
3. **Provide negative examples** — "Don't do X" is often as important as "Do Y"
4. **Test at the edges** — Adversarial inputs reveal prompt weaknesses
5. **Version control prompts** — Treat them as code, not configuration

#### Resources

- [Anthropic Prompt Engineering Guide](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/overview)
- [OpenAI Prompt Engineering Guide](https://platform.openai.com/docs/guides/prompt-engineering)
- [Brex's Prompt Engineering Guide](https://github.com/brexhq/prompt-engineering) — practical, production-focused
- [Learn Prompting](https://learnprompting.org/) — comprehensive, academically-grounded

---

### 1.2 Model Selection & Trade-offs

**Why it matters:** The "best" model changes monthly. Understanding the trade-off space lets you make defensible choices without chasing benchmarks.

#### Decision Framework

```
Task Complexity
     │
     ├── Simple (classification, extraction, formatting)
     │   └── Use smallest model that works (Haiku, GPT-4o-mini)
     │       └── Why: Cost, latency, sufficient capability
     │
     ├── Medium (summarization, code generation, analysis)
     │   └── Use mid-tier model (Sonnet, GPT-4o)
     │       └── Why: Balance of capability and cost
     │
     └── Complex (reasoning, planning, novel problems)
         └── Use frontier model (Opus, o1, Gemini Ultra)
             └── Why: Capability ceiling matters more than cost
```

#### Key Dimensions

| Dimension | Considerations |
|-----------|----------------|
| **Latency** | Time to first token, total generation time, streaming support |
| **Cost** | Input tokens, output tokens, cached vs. uncached |
| **Context window** | How much can fit, but also how well does it use long context? |
| **Capability** | Reasoning, code, multilingual, vision, tool use |
| **Reliability** | Uptime, rate limits, consistent behavior across versions |
| **Compliance** | Data residency, SOC 2, HIPAA, FedRAMP (critical for defense) |

#### Provider Landscape (as of early 2026)

| Provider | Strengths | Considerations |
|----------|-----------|----------------|
| **Anthropic (Claude)** | Reasoning, safety, long context, tool use | Smaller ecosystem |
| **OpenAI** | Broad capability, ecosystem, GPT-4o speed | Pricing pressure |
| **Google (Gemini)** | Multimodal, long context, Google integration | API stability concerns |
| **Meta (Llama)** | Open weights, self-hosting option | Requires infrastructure |
| **Mistral** | European, efficient, open-ish models | Smaller scale |
| **Cohere** | Enterprise focus, RAG-optimized | Narrower use case |

#### Anti-Pattern: Vendor Lock-in

Your EY experience with provider-agnostic solutions is the right instinct. Strategies:

1. **Abstract at the interface level** — Define your own `generate()` function that wraps providers
2. **Avoid provider-specific features** — Unless the ROI is overwhelming
3. **Test across providers regularly** — Monthly benchmark on your actual tasks
4. **Store prompts separately from provider config** — Swap models without rewriting prompts

---

### 1.3 Evaluation & Testing

**Why it matters:** "It works" is not a shipping criterion. GenAI systems need rigorous evaluation to catch regressions, measure improvements, and justify investments.

#### Evaluation Types

| Type | What It Measures | When to Use |
|------|------------------|-------------|
| **Unit evals** | Single input/output correctness | Every prompt change |
| **Regression evals** | Did we break something? | Before deployment |
| **Capability evals** | Can it do X at all? | New feature development |
| **Safety evals** | Harmful outputs, jailbreaks | Continuous monitoring |
| **Human evals** | Subjective quality, preference | Periodic, for calibration |
| **A/B evals** | Which version performs better in production? | Post-deployment optimization |

#### Building an Eval Suite

```python
# Conceptual structure for an eval suite
class Eval:
    name: str
    input: str
    expected_output: str | None  # None for open-ended
    grader: Callable[[str, str], float]  # (output, expected) -> score
    tags: list[str]  # ["regression", "safety", "code", etc.]

class EvalSuite:
    evals: list[Eval]
    
    def run(self, model: Model) -> EvalResults:
        results = []
        for eval in self.evals:
            output = model.generate(eval.input)
            score = eval.grader(output, eval.expected_output)
            results.append(EvalResult(eval, output, score))
        return EvalResults(results)
```

#### Grading Strategies

1. **Exact match** — Output must equal expected (brittle, use sparingly)
2. **Contains** — Output must contain key phrases
3. **Regex** — Output must match pattern
4. **LLM-as-judge** — Another model grades the output (powerful but expensive)
5. **Code execution** — For code gen, does it run and pass tests?
6. **Human-in-the-loop** — Sample and have humans rate

#### Resources

- [Hamel Husain's LLM Eval Guide](https://hamel.dev/blog/posts/evals/) — practical, opinionated
- [Anthropic's Eval Guidelines](https://docs.anthropic.com/en/docs/build-with-claude/develop-tests)
- [OpenAI Evals Framework](https://github.com/openai/evals)
- [Braintrust](https://www.braintrust.dev/) — eval platform
- [Promptfoo](https://promptfoo.dev/) — open source eval tool

---

### 1.4 Production Considerations

**Why it matters:** Demo-quality and production-quality are different planets. This section covers what it takes to ship and operate GenAI systems.

#### Reliability Patterns

| Pattern | Purpose | Implementation |
|---------|---------|----------------|
| **Retry with backoff** | Handle transient failures | Exponential backoff, jitter |
| **Fallback models** | Provider outages | Secondary provider, smaller model |
| **Timeout handling** | Prevent hung requests | Aggressive timeouts, streaming |
| **Rate limiting** | Stay within quotas | Token bucket, queue management |
| **Circuit breaker** | Prevent cascade failures | Fail fast when provider is down |
| **Caching** | Reduce cost and latency | Semantic cache for similar queries |

#### Cost Management

```
Monthly cost = (input_tokens × input_price) + (output_tokens × output_price)

Levers to reduce cost:
├── Reduce input tokens
│   ├── Shorter prompts
│   ├── Compression techniques
│   └── Better retrieval (less context stuffing)
├── Reduce output tokens
│   ├── Constrain output length
│   ├── Structured output (less verbose)
│   └── Stop sequences
├── Use cheaper models
│   ├── Route simple tasks to small models
│   └── Fine-tune small model for specific tasks
└── Cache aggressively
    ├── Exact match cache
    └── Semantic similarity cache
```

#### Observability

What to log:
- Request ID, timestamp, user ID
- Model, prompt version, parameters
- Input tokens, output tokens
- Latency (time to first token, total time)
- Success/failure, error type
- (Sampled) actual input and output

Tools:
- [LangSmith](https://smith.langchain.com/) — if using LangChain ecosystem
- [Weights & Biases](https://wandb.ai/) — experiment tracking
- [Helicone](https://helicone.ai/) — LLM observability proxy
- [Braintrust](https://www.braintrust.dev/) — logging + evals
- Custom: OpenTelemetry + your existing observability stack

#### Security Considerations

| Threat | Mitigation |
|--------|------------|
| Prompt injection | Input validation, output filtering, least privilege |
| Data leakage | Don't include sensitive data in prompts, use private deployments |
| Model extraction | Rate limiting, monitoring for systematic probing |
| Jailbreaks | Safety evals, content filtering, human review for high-stakes |

---

## Learning Path

### Beginner (Weeks 1-4)
- [ ] Complete Anthropic's prompt engineering documentation
- [ ] Build 3 different prompts for the same task, measure quality differences
- [ ] Implement basic retry logic with exponential backoff
- [ ] Set up logging for all LLM calls in a project

### Intermediate (Weeks 5-8)
- [ ] Build a simple eval suite with 20+ test cases
- [ ] Implement LLM-as-judge grading
- [ ] Add a fallback model to an existing integration
- [ ] Analyze cost breakdown for a real workload, identify optimization opportunities

### Advanced (Weeks 9-12)
- [ ] Implement semantic caching with similarity threshold
- [ ] Build a model router that selects based on task complexity
- [ ] Create a prompt versioning system with A/B testing capability
- [ ] Design and run a human eval study

---

## Project Ideas

### Project 1: Prompt Optimization Lab
Build a tool that takes a prompt and systematically tests variations:
- Different instruction phrasings
- With/without examples
- Different output formats
- Measure: quality (via eval), cost, latency

### Project 2: Multi-Provider Abstraction Layer
Build a thin SDK that:
- Provides unified interface across OpenAI, Anthropic, Google
- Handles retries, fallbacks, rate limiting
- Logs all calls with consistent schema
- Supports easy provider swapping via config

### Project 3: Cost Dashboard
Build a dashboard that:
- Tracks daily/weekly/monthly LLM spend
- Breaks down by model, use case, user
- Projects future costs based on growth
- Alerts on anomalies

---

## Key Takeaways

1. **Prompts are code** — Version them, test them, review them
2. **Evaluation is non-negotiable** — You can't improve what you can't measure
3. **Production is a different game** — Reliability, cost, and observability matter as much as capability
4. **Avoid lock-in** — The best model today won't be the best model tomorrow
5. **Start simple** — Add complexity only when simple approaches fail

---

## Further Reading

- "Building LLM Applications for Production" — Chip Huyen
- "What We Learned from a Year of Building with LLMs" — O'Reilly (various authors)
- Simon Willison's blog — ongoing commentary on practical LLM usage
- Hamel Husain's blog — especially evaluation content
- Eugene Yan's blog — ML systems and LLM engineering
