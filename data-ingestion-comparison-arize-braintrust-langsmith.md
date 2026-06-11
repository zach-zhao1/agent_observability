# Data Ingestion Comparison: Arize AX vs Braintrust vs LangSmith

This document compares how Arize AX, Braintrust, and LangSmith ingest observability data from AI applications and agents.

It focuses on the ingestion layer: how traces, spans, model calls, metadata, and related telemetry get from an application into each platform.

## Executive summary

The three vendors support similar observability outcomes, but they differ in how ingestion is designed.

- Arize AX and LangSmith are closer to each other on tracing capability than an earlier draft of this document implied. Both can trace model calls, tool calls, retrieval steps, nested workflow structure, inputs, outputs, latency, and metadata.
- Arize AX has the most standards-oriented ingestion story.
- Braintrust has the most product-integrated ingestion story.
- LangSmith has the most wrapper- and framework-oriented ingestion story.

In practice:

- Arize AX is strongest when open telemetry standards, OpenInference semantics, and portability matter.
- Braintrust is strongest when ingestion should be tightly connected to broader platform workflows such as gateway routing, review, datasets, and evaluations.
- LangSmith is strongest when you want an easy developer-facing ingestion path, especially in LangChain or LangGraph-adjacent ecosystems.

The key distinction between Arize AX and LangSmith is not that one traces application structure and the other does not. Both do. The difference is that Arize frames and transports that tracing more as standards-based telemetry, while LangSmith frames and captures it more through platform-native wrappers, decorators, and integrations.

## Important clarification

Arize AX and LangSmith are both doing AI-native application tracing.

Both can capture:

- model calls
- tool calls
- retrieval steps
- nested workflow structure
- inputs and outputs
- timing and metadata

So the difference is not:

- Arize only captures isolated model-call telemetry
- LangSmith captures the application tree

That would be inaccurate.

The more accurate difference is:

- Arize preserves application structure using OpenTelemetry spans enriched with OpenInference semantics
- LangSmith preserves application structure using LangSmith-native tracing abstractions such as wrapped clients, traceable functions, runs, and integrations

This is why the gap between them is more about ingestion architecture and trace representation than about basic tracing capability.

## Quick comparison

| Dimension | Arize AX | Braintrust | LangSmith |
| --- | --- | --- | --- |
| Primary ingestion model | Instrumentation that emits OpenTelemetry and OpenInference spans via OTLP | Braintrust SDK tracing plus gateway-based ingestion | Wrappers, decorators, and framework integrations |
| Auto-instrumentation | Yes | Yes | Yes |
| Manual instrumentation | Yes | Yes | Yes |
| App logic tracing | Yes | Yes | Yes |
| Standards emphasis | Highest | Moderate | Moderate |
| Gateway or proxy ingestion | Not a major reviewed theme | Yes, major differentiator | Not a major reviewed theme |
| Best fit | Teams wanting standards-based telemetry and portability | Teams wanting integrated observability plus inference control | Teams wanting framework-native, developer-friendly tracing |
| Mental model | Instrument the app so it emits standardized spans | Log and trace through the Braintrust platform, optionally through the gateway | Wrap clients and functions so LangSmith captures runs and traces |

## 1. Arize AX ingestion model

### Core model

From the reviewed docs, Arize AX has the clearest standards-based ingestion pipeline.

The documented flow is:

1. instrument the application using auto-instrumentation or manual instrumentation
2. emit spans using OpenTelemetry-compatible instrumentation
3. enrich spans with OpenInference semantic conventions for GenAI use cases
4. export spans to Arize AX using OTLP, typically over gRPC
5. ingest and visualize the spans in the Arize collector and UI

### What stands out

Arize is explicit about:

- OpenTelemetry as the telemetry layer
- OpenInference as the GenAI semantic layer
- OTLP as the transport protocol
- native instrumentation for many providers and frameworks

At the application level, this still means Arize can capture nested AI workflow structure such as agents, tools, retrievers, evaluators, and model calls. The important distinction is that Arize represents and transports that structure through standardized spans rather than primarily through a LangSmith-style wrapper and run model.

The docs also emphasize that this approach is portable and not locked to Arize as a tracing backend.

### Strengths

- strongest standards-based story among the three vendors reviewed
- good fit for teams already using OpenTelemetry
- good portability across backends and telemetry pipelines
- well suited for end-to-end system tracing, not just model calls

### Tradeoffs

- more telemetry-pipeline flavored than product-opinionated
- may require more engineering setup than gateway-style ingestion
- does not inherently provide inference-layer features like routing or caching

### Best fit

Arize AX is best when the ingestion architecture itself matters, especially if your team wants vendor-neutral telemetry and strong alignment with open observability standards.

## 2. Braintrust ingestion model

### Core model

From the reviewed docs, Braintrust has the most product-integrated ingestion model.

It supports multiple ingestion paths:

- auto-instrumentation for supported LLM libraries
- manual wrapping of model clients
- manual tracing of application logic
- automatic tracing through the Braintrust Gateway when model calls go through the gateway

### What stands out

Braintrust ingestion is designed as part of a larger operational system.

The docs show that Braintrust can ingest:

- LLM calls
- application logic
- metadata and tags
- scores and classifiers
- user feedback

The gateway is especially important because it changes the ingestion model. If calls go through the Braintrust Gateway, observability data is captured as part of the inference path itself.

### Strengths

- very integrated experience across tracing, feedback, datasets, and evals
- gateway can reduce instrumentation burden for model calls
- strong fit for teams that want one operational platform rather than a separate telemetry pipeline
- self-hosted data plane is explicitly documented

### Tradeoffs

- less standards-first in the product narrative than Arize
- more Braintrust-native as an ingestion model
- gateway-based ingestion still benefits from app-level tracing to capture non-LLM logic fully

### Best fit

Braintrust is best when you want ingestion to connect directly into a broader platform workflow that includes inference control, evaluation, and review operations.

## 3. LangSmith ingestion model

### Core model

From the reviewed docs, LangSmith's ingestion model is centered on wrappers, decorators, and framework integrations.

The common path is:

1. enable tracing with environment variables and API keys
2. wrap model clients such as OpenAI or Anthropic
3. decorate or wrap functions with tracing helpers such as `traceable`
4. send traces to LangSmith endpoints

LangSmith also documents broad integrations for frameworks and providers.

### What stands out

LangSmith ingestion is very developer-facing.

It is especially convenient when:

- you already use LangChain or LangGraph
- you want nested traces that follow application code structure
- you prefer lightweight code-level integration over setting up a fuller telemetry pipeline

LangSmith is not unique in preserving application structure. Arize also preserves it. The difference is that LangSmith makes wrappers, decorators, and framework integrations the primary developer mental model, whereas Arize makes instrumentation, spans, and export the primary mental model.

### Strengths

- very approachable setup path
- strong ecosystem integration model
- easy for teams already near the LangChain ecosystem
- good for app-level logical traces based on functions, tools, and workflows

### Tradeoffs

- less explicitly standards-centered than Arize
- no major gateway or proxy ingestion story in the reviewed docs
- more framework- and integration-dependent than a pure OTel-first approach

### Best fit

LangSmith is best when easy application-level tracing and strong framework integration matter more than standards-first telemetry architecture or gateway-based inference control.

## Core difference by ingestion model

The cleanest way to compare the three is by how observability data gets captured.

### Arize AX

Captures data because the app is instrumented to emit standard telemetry spans.

### Braintrust

Captures data through SDK tracing, and in some cases through the gateway because traffic passes through Braintrust's inference layer.

### LangSmith

Captures data because the application uses LangSmith-aware wrappers, decorators, or integrations.

## Ingestion model comparison by control point

| Vendor | Main control point for ingestion | Practical description |
| --- | --- | --- |
| Arize AX | Telemetry layer | The application is instrumented so it emits OpenTelemetry and OpenInference spans to Arize |
| Braintrust | Application layer plus inference layer | The app can emit traces through the SDK, and the gateway can capture model traffic directly |
| LangSmith | Application code and framework layer | The app emits traces through wrappers, decorators, and integrations |

## Practical takeaways

### If you care most about standards and portability

Choose Arize AX.

Why:

- strongest OpenTelemetry and OpenInference story
- clearest OTLP-based ingestion path
- best fit for vendor-neutral telemetry architecture

### If you care most about integrated operational workflows

Choose Braintrust.

Why:

- ingestion ties naturally into gateway routing, evaluation, datasets, feedback, and review
- strong SDK path plus gateway-based capture for model calls

### If you care most about developer-friendly framework-native tracing

Choose LangSmith.

Why:

- easiest wrapper and decorator story
- strong integration with popular agent and LLM frameworks
- especially natural for LangChain and LangGraph users

## Bottom line

All three vendors can ingest traces and support observability, but they emphasize different design philosophies.

- Arize AX emphasizes open telemetry standards and standardized span semantics.
- Braintrust emphasizes integrated platform workflows and gateway-based capture.
- LangSmith emphasizes application-level wrappers, run-oriented tracing abstractions, and framework-native tracing.

The best choice depends on whether you want the ingestion layer to behave more like:

- a telemetry pipeline
- an integrated product workflow
- a developer-facing tracing SDK and framework integration layer

## Sources reviewed

- Arize AX tracing: https://arize.com/docs/ax/observe/tracing
- Arize AX tracing concepts: https://arize.com/docs/ax/instrument/what-are-traces
- Braintrust instrumentation: https://www.braintrust.dev/docs/instrument
- Braintrust trace LLM calls: https://www.braintrust.dev/docs/instrument/trace-llm-calls
- Braintrust trace application logic: https://www.braintrust.dev/docs/instrument/trace-application-logic
- LangSmith observability: https://docs.langchain.com/langsmith/observability
- LangSmith tracing quickstart: https://docs.langchain.com/langsmith/observability-quickstart
- LangSmith integrations: https://docs.langchain.com/langsmith/integrations