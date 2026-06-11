# Observability Platform Recommendations by Scenario

This document turns the broader comparison of Arize AX, Braintrust, and LangSmith into scenario-based recommendations.

It is based on the documentation review captured in `arize-braintrust-langsmith-comparison.md` and is intended as a practical decision guide rather than a full product deep dive.

## Quick recommendations

| Scenario | Best fit | Why |
| --- | --- | --- |
| Running agent, not using LangGraph | Braintrust | Best general fit for adding observability, review, evals, and optional inference control without replatforming |
| Running agent, main need is debugging and improvement | Arize AX | Strongest fit for diagnosing failures, forming hypotheses, and running controlled experiments |
| LangGraph-based agent | LangSmith | Most natural alignment with LangChain/LangGraph plus deployment/runtime integration |
| Review-heavy human-in-the-loop workflow | Braintrust | Strongest documented review operations, assignment, scoring, and dataset curation flow |
| Agent platform with hosted runtime concerns | LangSmith | Strongest deployment/runtime story with Agent Server and environment models |
| Existing production system where quality optimization matters most | Arize AX | Strongest improvement-loop framing tying traces, datasets, experiments, and online evals |
| Team wants provider routing/caching as part of the platform | Braintrust | Gateway is the clearest inference-layer differentiator in the reviewed docs |

## 1. Running agent not using LangGraph

### Recommendation: Braintrust

For a running agent that is not based on LangGraph, Braintrust is the safest default choice from the reviewed docs.

Why it fits best:

- works well as a layer on top of an already-running system
- supports tracing both model calls and application logic
- has strong human review, dataset creation, and evaluation workflows
- does not require buying into a LangGraph-oriented deployment model
- can later add inference control through the Braintrust Gateway if desired

Practical adoption path:

1. instrument the current agent
2. analyze logs and traces
3. turn real failures and feedback into datasets
4. run experiments and evaluations
5. optionally unify model traffic through the gateway later

Why not LangSmith as the default here:

LangSmith can absolutely be used with an already-running non-LangGraph agent, but its biggest differentiator is the tighter link between deployment/runtime and observability/evaluation. If you are not using LangGraph and do not want to move closer to LangSmith's deployment model, a meaningful part of its advantage matters less.

Why not Arize AX as the default here:

Arize is strong here too, but it is more specialized toward debugging and improvement than Braintrust's broader operational workflow.

## 2. Running agent where debugging and quality improvement are the main problem

### Recommendation: Arize AX

Arize AX is the best fit when the system is already running and the main question is: what is breaking, why is it breaking, and did the fix actually improve behavior?

Why it fits best:

- strong trace and span analysis
- improvement loop is the center of the product
- explicit workflow around hypothesis formation before changing the system
- experiments tied directly to datasets and evaluators
- online evals continue after changes ship

Use Arize AX when your team wants to:

- find failure patterns quickly
- isolate bad tool calls or retrieval behavior
- compare prompt or model changes rigorously
- treat AI quality improvement as an engineering optimization loop

## 3. LangGraph-based agent

### Recommendation: LangSmith

If the agent is built with LangGraph or already sits close to the LangChain ecosystem, LangSmith is the strongest fit.

Why it fits best:

- strongest product alignment with LangChain and LangGraph workflows
- first-class observability and evaluation support for those systems
- deployment/runtime model is a feature, not just an add-on
- easier path to keeping development, deployment, and production analysis in one platform

This is the case where LangSmith's tighter runtime coupling is a real advantage rather than extra complexity.

## 4. Review-heavy human-in-the-loop workflow

### Recommendation: Braintrust

Braintrust is the strongest fit when people are deeply involved in judging quality.

Why it fits best:

- explicit human review workflows
- assignments and multi-reviewer support
- structured review scores, comments, and tags
- clear dataset promotion flow from reviewed production items
- operationally richer review UX in the reviewed docs than the others

This is the best fit for:

- support copilots with SME review
- regulated or high-stakes workflows
- evaluation programs where human judgment is central

## 5. Team wants one platform for runtime plus observability plus evaluation

### Recommendation: LangSmith

LangSmith is strongest when the team wants the observability platform to be close to how the agent is actually deployed and run.

Why it fits best:

- explicit deployment product area
- Agent Server runtime model
- deployment environments and platform setup options
- operational docs for auth, customization, CI/CD, and runtime concerns

This is the right fit when the agent is treated as an application platform problem, not just a prompt-quality problem.

## 6. Team wants provider routing, caching, and inference abstraction

### Recommendation: Braintrust

Braintrust stands out because the gateway is a real product surface in the reviewed docs, not just an integration detail.

Why it fits best:

- one API layer across providers
- ability to use one provider SDK against another provider's models
- built-in caching and logging
- endpoint and credential controls
- natural path from observability into inference standardization

If you want your observability vendor to also help standardize model access, Braintrust is the strongest fit from the reviewed docs.

## 7. Existing production system where improvement quality loop matters more than platform operations

### Recommendation: Arize AX

Choose Arize AX when the central job is improving output quality in a live system, not building a broader operations platform around it.

Why it fits best:

- clearest observe -> hypothesize -> experiment -> measure loop
- strong emphasis on diagnosing why outputs went wrong
- direct tie between reviewed traces, golden datasets, experiments, and evals
- best conceptual fit for engineering-led quality optimization

## Practical summary

If you want the shortest version of the decision:

- choose Braintrust for the best all-around fit for an already-running non-LangGraph agent
- choose Arize AX if your main need is deep diagnosis and experiment-driven improvement
- choose LangSmith if you want deployment/runtime and observability/evaluation to live in the same platform, especially for LangGraph systems

## Caveats

- These recommendations are based on documentation review, not hands-on implementation.
- Enterprise-only capabilities or deeper product areas may shift the recommendation in specific environments.
- The best choice can still depend on existing framework choices, data residency constraints, and pricing.