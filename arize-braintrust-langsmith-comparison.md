# Arize AX vs Braintrust vs LangSmith

This document summarizes findings from a review of the main public documentation surfaces for Arize AX, Braintrust, and LangSmith. It focuses on product similarities, differences, and practical positioning rather than marketing copy.

## Scope and method

I did not literally read every page in each documentation set. I reviewed the main product and workflow pages that define each platform's capabilities and product boundaries.

Sources reviewed:

- Arize AX overview: https://arize.com/docs/ax
- Arize core workflows: https://arize.com/docs/ax/core-workflows
- Braintrust getting started: https://www.braintrust.dev/docs
- Braintrust workflow: https://www.braintrust.dev/docs/workflow
- Braintrust playgrounds: https://www.braintrust.dev/docs/evaluate/playgrounds
- Braintrust human review: https://www.braintrust.dev/docs/annotate/human-review
- Braintrust gateway: https://www.braintrust.dev/docs/deploy/gateway
- Braintrust Loop: https://www.braintrust.dev/docs/loop
- LangSmith home: https://docs.langchain.com/langsmith/home
- LangSmith observability: https://docs.langchain.com/langsmith/observability
- LangSmith evaluation: https://docs.langchain.com/langsmith/evaluation
- LangSmith prompt engineering: https://docs.langchain.com/langsmith/prompt-engineering
- LangSmith deployment: https://docs.langchain.com/langsmith/deployment
- LangSmith platform setup: https://docs.langchain.com/langsmith/platform-setup

## Executive summary

All three products occupy the same general market category: platforms for building and improving LLM applications and agents using observability, evaluation, and iterative development workflows.

They overlap heavily on:

- tracing and observability
- offline evaluation on curated datasets
- online evaluation on production traffic
- human feedback and annotation
- prompt and model iteration
- support for multiple frameworks and model providers

The main differences are in product emphasis:

- Arize AX is most centered on the improvement loop: observe failures, form hypotheses, run experiments, measure with evals, and continue scoring in production.
- Braintrust is most centered on the end-to-end operational workflow: instrument, observe, annotate, evaluate, deploy, and feed production data back into the system. It also has the clearest inference gateway product.
- LangSmith is most centered on agent development plus deployment/runtime infrastructure. Its docs most clearly position deployment as a first-class product pillar alongside observability and evaluation.

## High-level similarities

### 1. All three are observability products for AI systems

All three platforms document tracing or logging as the core primitive for understanding LLM and agent behavior.

Common themes:

- capture inputs, outputs, tool calls, metadata, latency, and tokens
- inspect individual runs and spans
- analyze production behavior across many runs
- use traces as the basis for debugging and improvement

### 2. All three treat evaluation as a core workflow

These are not pure dashboards. Each product includes evaluation systems that let teams score outputs and compare changes.

Common patterns:

- offline evaluation with curated datasets
- online or continuous evaluation on live traffic
- LLM-as-judge style evaluation
- code-based evaluators or scorers
- experiment comparison or benchmarking

### 3. All three close the loop between production and development

Each product documents some version of this loop:

1. capture production behavior
2. identify issues
3. turn examples into datasets or experiments
4. evaluate changes
5. deploy or apply improvements
6. monitor the impact in production

### 4. All three are positioned for agents, not only single prompts

The reviewed docs consistently discuss agent traces, workflows, tools, or multi-step systems rather than only simple prompt playground use cases.

## High-level differences

### Arize AX

Arize AX presents itself as an AI engineering platform focused on observing, improving, and evaluating AI applications with confidence.

Its docs emphasize:

- tracing and span analysis
- finding problematic traces with filters and AI-assisted search
- reviewing failures and building datasets from bad runs
- experimenting on prompts, models, and retrieval configs
- measuring variants with evaluators
- online evals after deployment

The strongest pattern in the Arize docs is the improvement loop:

- Observe
- Annotate and evaluate
- Hypothesize
- Experiment
- Measure
- Apply or iterate

The product reads as an engineering optimization and diagnosis platform for AI systems.

Distinctive signals in the reviewed docs:

- Alyx, an AI assistant for exploring traces and issue patterns
- explicit workflow around forming hypotheses before changing the system
- strong linkage between traces, experiments, datasets, and online evals
- AX CLI and skills integration for use inside coding-agent workflows

### Braintrust

Braintrust presents itself as an AI observability platform that helps teams measure, evaluate, and improve AI in production. The reviewed docs show a broader operational loop than the label alone suggests.

Its workflow is explicitly documented as:

- Instrument
- Observe
- Annotate
- Evaluate
- Deploy

Its docs emphasize:

- tracing both LLM calls and application logic
- production log analysis, filters, dashboards, and topics
- structured human review and annotation
- datasets promoted from logs and user feedback
- playground-driven iteration and experiments
- deployment of prompts and functions
- monitoring and alerting in production

Distinctive signals in the reviewed docs:

- very strong human review workflow with assignments, multiple reviewers, structured scores, comments, and review queues
- strong dataset curation pipeline from logs and feedback
- Loop, an AI assistant embedded across logs, traces, datasets, experiments, and playgrounds
- Braintrust Gateway, a unified inference layer for routing requests across providers with caching, logging, and provider abstraction

Braintrust reads like the most workflow-complete product for teams that want one place for observability, annotation, evaluation, prompt iteration, and production inference control.

### LangSmith

LangSmith presents itself as a framework-agnostic platform for building, debugging, and deploying AI agents and LLM applications.

Its docs emphasize four top-level pillars:

- observability
- evaluation
- prompt engineering
- deployment

Its reviewed pages emphasize:

- tracing and production monitoring
- offline and online evaluation
- dataset and evaluator management
- prompt engineering tooling
- deployment through Agent Server and related infrastructure
- cloud and self-hosted setup options

Distinctive signals in the reviewed docs:

- strongest deployment/runtime story of the three reviewed products
- explicit treatment of deployment as a major product category, not just a last-mile action
- agent runtime concepts such as assistants, threads, and runs
- operational docs for auth, customization, CI/CD, and deployment models

LangSmith reads most like an agent platform with observability and evals built in, rather than an observability product that later added deployment.

## Feature-by-feature comparison

| Capability | Arize AX | Braintrust | LangSmith |
| --- | --- | --- | --- |
| Core positioning | AI engineering platform | AI observability platform for measuring, evaluating, and improving AI in production | Framework-agnostic platform for building, debugging, and deploying agents and LLM apps |
| Primary workflow | Observe -> annotate/evaluate -> hypothesize -> experiment -> measure -> apply/iterate | Instrument -> observe -> annotate -> evaluate -> deploy | Observability + evaluation + prompt engineering + deployment |
| Tracing / observability | Strong trace/span analysis, filters, search, instrumentation | Strong logs/traces, filters, dashboards, topics, natural-language analysis | Strong trace-to-production visibility across many integrations |
| Human review / annotation | Human review and labeling queues tied to traces/evals | Most explicit review workflow in reviewed docs | Supported in evaluation workflow, but less operationally detailed in reviewed pages |
| Dataset creation | Golden datasets from reviewed traces | Strong promotion from logs, review, and user feedback | Datasets from curated examples, historical traces, and synthetic generation |
| Experimentation | Variant comparison across prompts/models/retrieval | Rich playgrounds, experiments, diff mode, scorers, datasets | Experiment-oriented evaluation workflow, prompt engineering surfaces |
| Automated evaluation | Code evaluators and LLM-as-judge, including online evals | Autoevals, LLM-as-judge, custom scorers, online scoring | Human, code, LLM-as-judge, pairwise, offline and online eval |
| Prompt iteration | Part of experiments and improvements | Strong interactive playground-based prompt iteration | First-class prompt engineering category |
| Production monitoring | Online evals continue after changes ship | Dashboards, monitoring, alerts, production scoring | Production observability and online evaluation |
| Embedded AI assistant | Alyx | Loop | Not a major theme in the reviewed pages |
| Deployment / runtime | Apply changes and keep evaluating in production, but runtime hosting is less central in reviewed pages | Deploy prompts/functions and monitor; gateway is a major deploy surface | Strongest deployment/runtime story with Agent Server and environment models |
| Inference gateway / proxy | Not a major reviewed theme | Major differentiator: unified gateway, caching, routing, logging | Not a major reviewed theme |
| Hosting / platform setup | Not established from the reviewed pages | Not established from the reviewed pages | Explicit cloud and self-hosted options |

## Detailed differences by category

### Observability

#### Arize AX

Arize frames observability as the first step in an engineering improvement cycle. The docs emphasize traces, spans, filters, and AI-assisted querying to find problematic runs.

Interpretation: Arize treats observability less as passive monitoring and more as the diagnostic entry point for controlled improvement work.

#### Braintrust

Braintrust's observability surface includes logs, filters, dashboards, topics, and a strong natural-language analysis layer through Loop.

Interpretation: Braintrust emphasizes both traditional observability and interactive operational analysis by product teams.

#### LangSmith

LangSmith emphasizes visibility from individual traces to production-wide metrics. The docs highlight broad provider/framework integration and tie observability closely to evaluation and deployment.

Interpretation: LangSmith's observability is presented as one pillar of a broader agent-development platform.

### Evaluation

#### Arize AX

Evaluation in Arize is tightly connected to experiments and the improvement loop. Evaluators measure whether a change was actually better, and online evals continue after a change is shipped.

Interpretation: Arize emphasizes using evals to validate hypotheses and compare variants in a disciplined engineering loop.

#### Braintrust

Braintrust supports prebuilt autoevals, LLM-as-judge, and custom scorers. It provides online scoring and integrates evals deeply with playgrounds and production traces.

Interpretation: Braintrust treats evals as a continuous operational system with strong interactive tooling.

#### LangSmith

LangSmith clearly distinguishes offline and online evaluation and documents datasets, evaluators, experiments, concurrency, caching, and comparison patterns.

Interpretation: LangSmith provides a formal evaluation system that looks mature and structured, especially for development and deployment workflows.

### Human feedback and annotation

#### Arize AX

Arize documents human review and labeling queues, using trace review to build golden datasets and identify common error patterns.

Interpretation: Human feedback is important, but it is framed mainly as part of the model-improvement loop.

#### Braintrust

Braintrust has the most fully articulated review system in the pages reviewed. It supports assignments, multiple reviewers, structured scores, comments, tags, review queues, and promotion of reviewed items into datasets.

Interpretation: Braintrust appears strongest for teams that rely heavily on human review operations and data curation.

#### LangSmith

LangSmith supports human evaluators and curated datasets, but the reviewed pages were less operationally detailed on review management than Braintrust.

Interpretation: Human evaluation is present and useful, but not the standout differentiator in the reviewed docs.

### Prompt iteration and experimentation

#### Arize AX

Arize supports experimentation on prompts, models, and retrieval configurations using curated datasets and evals.

Interpretation: The experimentation model is engineering-oriented and tied to explicit hypotheses.

#### Braintrust

Braintrust playgrounds are a major product surface. They support tasks, scorers, datasets, side-by-side comparison, diff mode, annotations, and promotion into immutable experiments.

Interpretation: Braintrust appears strongest for interactive, browser-based iteration across prompts, workflows, and scorers.

#### LangSmith

LangSmith treats prompt engineering as a first-class docs category and includes AI-assisted optimization in its playground/chat surfaces.

Interpretation: Prompt work is clearly important, though the reviewed pages emphasize the platform framing more than a single experimentation workbench.

### Deployment and runtime

#### Arize AX

Arize discusses applying changes and continuing online evals in production, but the reviewed pages do not make deployment runtime infrastructure a major centerpiece.

Interpretation: Arize is more about improving deployed AI systems than being the runtime platform that hosts them.

#### Braintrust

Braintrust includes deployment of prompts and functions, environments, monitoring, and especially the gateway.

Interpretation: Braintrust has meaningful deployment features, but the most differentiated deployment surface is its inference control plane rather than a full agent runtime.

#### LangSmith

LangSmith deployment is a major product area. The docs explicitly cover Agent Server, deployment environments, customization, auth, and operations.

Interpretation: LangSmith appears strongest if your team wants observability and evals tightly coupled to how agents are actually hosted and run.

### Inference gateway and provider abstraction

#### Arize AX

The reviewed AX pages do not foreground a unified inference gateway or proxy layer.

#### Braintrust

Braintrust Gateway is a notable differentiator. It provides:

- one gateway URL for multiple providers
- support for using one provider SDK to call another provider's models
- built-in caching
- logging into Braintrust traces
- routing and endpoint controls
- credential scoping by organization or project

Interpretation: Braintrust has the clearest answer if you want to standardize inference routing and logging in addition to evaluation.

#### LangSmith

The reviewed LangSmith pages do not position a gateway/proxy layer as a primary product differentiator.

## Using these platforms with an already-running agent

If you already have an agent running in production, the most practical question is not which platform can host a new agent from scratch. It is which platform can add value to the system you already operate.

### Arize AX

Arize AX appears well suited for teams that already have a running agent and want to improve it without changing the serving stack first.

Most natural adoption path:

- instrument the running agent to send traces and spans into Arize
- inspect failures, latency issues, tool-call problems, and bad outputs
- review traces and turn failure cases into datasets
- run experiments on prompt, model, or retrieval changes
- score the changes with evaluators and continue online evals after shipping

Practical read: Arize fits well when the runtime already exists and the main problem is diagnosing failures and improving quality over time.

### Braintrust

Braintrust also fits an already-running agent well, especially if the team wants to layer in observability, annotation, evaluation, and eventually inference control.

Most natural adoption path:

- instrument the running agent to log traces, model calls, and application logic
- review logs and production traces to identify issues
- collect human feedback and promote real production cases into datasets
- use playgrounds and experiments to compare prompt, model, or workflow changes
- optionally move model calls behind the Braintrust gateway for unified routing, caching, and logging

Practical read: Braintrust fits well when the runtime already exists and the team wants to add a strong operational workflow around review, datasets, evals, and provider routing.

### LangSmith

LangSmith can also be used with an already-running agent without immediately moving hosting to LangSmith's deployment model.

Most natural adoption path:

- instrument the existing agent so LangSmith captures traces, runs, inputs, outputs, tool calls, and metadata
- use production traces to debug issues and create datasets from real behavior
- add offline and online evaluators to measure the current system and compare future changes
- keep the existing runtime in place at first, then decide later whether tighter deployment integration with LangSmith is useful

Practical read: LangSmith can start as an observability and evaluation layer on top of your current runtime, but it becomes most differentiated when you want deployment, execution, observability, and evaluation tied together in one platform.

### Practical distinction for existing systems

If the agent is already running, the first-step difference is usually:

- Arize AX: add tracing and use it to diagnose and improve the system you already run
- Braintrust: add tracing, review, datasets, evals, and optionally unify inference through the gateway
- LangSmith: add tracing and evals first, then optionally move closer to LangSmith's deployment/runtime model later

This is why LangSmith's deployment story matters, but does not require an immediate migration. Its extra value is highest when a team eventually wants one platform to cover development, deployment, runtime execution, observability, and evaluation together.

## Product positioning summary

### Arize AX is best described as

An AI engineering and optimization platform centered on diagnosing issues in production systems and improving them through experiments and evaluation.

Best fit if you care most about:

- root-cause analysis for agent failures
- hypothesis-driven iteration
- tying traces directly to experiments and online evals
- improving behavior of already-deployed AI systems

### Braintrust is best described as

An end-to-end AI quality workflow platform with strong observability, annotation, evaluation, prompt iteration, and inference gateway capabilities.

Best fit if you care most about:

- operational review workflows
- curating datasets from real logs and feedback
- rich playground iteration
- one platform for tracing, evaluation, and inference control

### LangSmith is best described as

An agent development and deployment platform with strong observability and evaluation built in.

Best fit if you care most about:

- building and running agents as an application platform concern
- deployment/runtime infrastructure
- coupling observability and evals directly to deployed agent systems
- cloud or self-hosted operational models

## Practical conclusion

These products compete in the same category, but they are not identical.

- If your center of gravity is debugging and systematically improving AI behavior, Arize AX looks strongest.
- If your center of gravity is continuous evaluation plus human review plus inference control, Braintrust looks strongest.
- If your center of gravity is agent runtime and deployment infrastructure, LangSmith looks strongest.

The overlap is real, especially around tracing and evals. The meaningful buying differences are less about whether they support observability and more about where each vendor puts the center of the workflow.

## Caveats

- This comparison is based on documentation review, not product hands-on testing.
- The comparison reflects the reviewed pages as of 2026-06-09.
- Some capabilities may exist in deeper docs or enterprise materials that were not reviewed here.
- Arize's broader relationship between AX and Phoenix was not fully established from the reviewed AX pages alone.