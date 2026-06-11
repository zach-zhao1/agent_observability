# Data Ingestion Models for AI Observability

This note explains three different ingestion models used by AI observability platforms:

- gateway or proxy-based ingestion
- OpenTelemetry and OpenInference spans via OTLP
- wrapper, decorator, and framework-integration-based ingestion

The goal is to explain how these approaches differ conceptually and operationally, especially in the context of Braintrust, Arize AX, and LangSmith.

## Short version

- Gateway or proxy-based ingestion captures model traffic because requests pass through the platform.
- OTel and OpenInference via OTLP capture telemetry because your application emits standard spans.
- Wrappers, decorators, and framework integrations capture telemetry because your code uses vendor-aware tracing hooks.

These approaches can overlap, but they place observability at different control points in the system.

## 1. Gateway or proxy-based ingestion

### What it is

Gateway-based ingestion means the observability platform sits in the request path for model calls.

Your application sends model requests to the platform's gateway endpoint instead of sending them directly to the model provider.

The gateway then:

- forwards the request to the provider
- receives the provider response
- captures request and response data for observability
- logs metadata such as latency, token usage, errors, and sometimes cost

In this model, ingestion happens because traffic physically passes through the platform-controlled layer.

### Braintrust example

From the reviewed docs, Braintrust Gateway provides a unified API endpoint for model access. Requests sent through the gateway are automatically traced, and the gateway can also provide caching, routing, and provider abstraction.

That means the gateway is not just a networking feature. It is also part of the observability ingestion path.

### Typical flow

1. The app sends a request to the gateway URL.
2. The gateway forwards the request to the model provider.
3. The gateway captures inputs, outputs, latency, token usage, and errors.
4. The platform stores that data as part of logs, traces, or spans.

### Strengths

- low instrumentation effort for model calls
- centralized visibility across providers
- easy to add routing, caching, retries, and endpoint controls
- useful when many teams or services use different model SDKs

### Limitations

- only automatically captures what passes through the gateway
- does not automatically capture full application behavior outside the model call
- still benefits from app-level tracing for retrieval, tools, orchestration, and business logic

### Best fit

Gateway ingestion is strongest when the main observability need is to centralize and standardize model-call visibility.

## 2. OpenTelemetry and OpenInference spans via OTLP

### What it is

This is a telemetry pipeline model.

Your application emits spans using OpenTelemetry-compatible instrumentation. GenAI-specific details are added through semantic conventions such as OpenInference. Those spans are then exported over OTLP to a collector or backend.

In this model, the platform is not in the request path. It receives telemetry emitted by the application.

### Arize AX example

From the reviewed docs, Arize AX is explicit about this flow:

- instrumentation creates spans
- OpenInference semantic conventions enrich the spans for GenAI use cases
- spans are exported via OTLP, usually over gRPC
- the Arize collector ingests and visualizes them

This is the clearest standards-based ingestion story among the three vendors reviewed.

### Typical flow

1. The app is instrumented with OpenTelemetry-compatible tracing.
2. Spans are created for LLM calls, tools, retrieval, agents, chains, and other steps.
3. OpenInference attributes enrich the spans with GenAI-specific metadata.
4. An exporter sends spans via OTLP.
5. The backend ingests and visualizes the data.

### Strengths

- standards-based and vendor-neutral
- portable across backends
- captures full-system execution, not just model calls
- works well if your broader engineering stack already uses OpenTelemetry
- good foundation for combining AI telemetry with the rest of application telemetry

### Limitations

- usually more engineering setup than gateway-based ingestion
- depends on correct instrumentation and exporter setup
- does not provide inference-layer control such as routing or caching by itself

### Best fit

OTel and OpenInference ingestion is strongest when you care about open standards, telemetry portability, and end-to-end application observability.

## 3. Wrappers, decorators, and framework integrations

### What it is

This is an application-level instrumentation model.

Instead of relying primarily on a gateway or a full telemetry pipeline, the platform provides code-level helpers that developers add directly to the application:

- wrappers around provider clients
- decorators around functions
- framework-native integrations that automatically emit traces

The platform is still not in the request path. Observability data is created because the application is using the platform's tracing APIs or integrations.

### LangSmith example

From the reviewed docs, LangSmith commonly uses:

- environment variables to enable tracing
- wrappers for model clients like OpenAI
- `traceable` decorators or wrappers for functions
- framework integrations for LangChain, LangGraph, and other ecosystems

This creates nested traces that reflect the application's logical execution structure.

### Typical flow

1. The developer enables tracing with configuration and API keys.
2. Provider clients are wrapped.
3. Functions, tools, or workflows are wrapped with decorators or traceable helpers.
4. Traces are sent directly to the platform endpoint.

### Strengths

- easy for developers to adopt in supported ecosystems
- traces align well with application code structure
- lower conceptual overhead than a full OpenTelemetry pipeline
- especially effective in framework ecosystems like LangChain and LangGraph

### Limitations

- more vendor-specific than a standards-based telemetry pipeline
- does not provide inference-layer controls like a gateway
- portability depends more on code changes and framework support than on open telemetry standards

### Best fit

Wrapper and decorator ingestion is strongest when fast developer adoption and framework-native tracing matter more than standards-first telemetry architecture.

## Core difference by control point

The cleanest way to distinguish the three models is by where observability is attached.

| Model | Where observability attaches | Why data is captured |
| --- | --- | --- |
| Gateway or proxy | At the model request boundary | Requests pass through the platform |
| OTel and OpenInference via OTLP | In the app telemetry layer | The app emits standard spans |
| Wrappers and decorators | In the application code layer | The code uses tracing-aware hooks |

## Practical analogy

Imagine you want to observe package deliveries.

- Gateway model: Every package passes through your warehouse, so you inspect and log it there.
- OTel model: Every truck and sorting station emits standardized tracking events to a central system.
- Wrapper model: The delivery app logs milestones because developers added tracking code into the app.

All three approaches can work. They just place the point of observation in different parts of the system.

## What each model is best at

### Gateway or proxy-based ingestion

Best at:

- centralized model-call observability
- provider abstraction
- routing and caching
- consistent logging across teams and services

Less ideal for:

- full non-LLM application tracing unless paired with app instrumentation

### OTel and OpenInference via OTLP

Best at:

- end-to-end system observability
- standards-based telemetry
- backend portability
- combining AI tracing with broader observability systems

Less ideal for:

- lowest-friction setup
- inference control features such as routing and caching

### Wrappers, decorators, and framework integrations

Best at:

- developer-friendly setup
- application-structure-aware traces
- framework-native adoption
- quick enablement in supported ecosystems

Less ideal for:

- vendor-neutral portability
- inference control and request interception

## Why Braintrust Gateway is materially different

Braintrust's gateway is different because observability is tied to the serving path itself.

With wrappers or OTel instrumentation, the application emits telemetry about its own behavior.

With a gateway, observability happens at the point where model requests are executed and routed. That makes it easier to:

- capture model calls without per-call instrumentation
- standardize inference behavior across providers
- apply caching and routing at the same point where data is logged
- observe model traffic consistently across different applications and teams

However, gateway ingestion does not replace application tracing when you want to understand everything around the model call.

## Practical recommendation

- Choose gateway-based ingestion if you want centralized visibility and control over model traffic.
- Choose OTel and OpenInference if you want standards-based, end-to-end telemetry across your whole system.
- Choose wrappers, decorators, and framework integrations if you want the fastest adoption path inside your current application code and frameworks.

## Vendor-oriented summary

### Arize AX

Most aligned with:

- OTel and OpenInference spans via OTLP

Why:

- strongest standards-based ingestion story in the reviewed docs
- explicit OpenTelemetry and OpenInference framing
- explicit collector and OTLP export flow

### Braintrust

Most aligned with:

- gateway or proxy-based ingestion, plus SDK tracing

Why:

- gateway is a major product differentiator
- SDK tracing and app-logic tracing are also documented
- ingestion is tied closely to broader operational workflows

### LangSmith

Most aligned with:

- wrappers, decorators, and framework integrations

Why:

- easy tracing via wrapped clients and `traceable` functions
- strong ecosystem integration model
- especially natural for LangChain and LangGraph-based applications