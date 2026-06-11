# Self-Hosted Deployment Comparison: Arize, Braintrust, and LangSmith

This document summarizes the documented self-hosted deployment models for Arize, Braintrust, and LangSmith based on the vendor documentation reviewed on 2026-06-09.

## Important framing

These three vendors are not fully apples-to-apples on self-hosting.

- LangSmith documents a full self-hosted platform option.
- Braintrust documents a self-hosted data plane with a vendor-managed control plane.
- Arize, from the pages reviewed, clearly documents self-hosting for Phoenix, but not a clearly equivalent self-hosted AX deployment model.

Because of that, the most accurate comparison is by deployment model, not just by a simple yes or no for self-hosting.

## Quick comparison

| Vendor | Documented self-hosted model | What runs in your infrastructure | What remains vendor-managed | Practical takeaway |
| --- | --- | --- | --- | --- |
| LangSmith | Full self-hosted platform | Core services and storage stack | None required for the base self-hosted instance | Most complete documented self-hosted platform story |
| Braintrust | Self-hosted data plane | Sensitive AI data and data-plane services | Control plane, web UI, auth, metadata management | Strong data residency option, but not fully self-hosted end to end |
| Arize | Clear self-hosting for Phoenix | Phoenix services in local, Docker, Kubernetes, or your cloud | AX self-hosting not clearly established in reviewed AX docs | Strong self-hosted OSS path, but not clearly a self-hosted AX equivalent |

## LangSmith

### Documented self-hosting model

LangSmith has the clearest and most complete self-hosted story in the docs reviewed.

The docs explicitly state that LangSmith can be deployed in two modes:

- cloud
- self-hosted

The self-hosted option is available on the Enterprise plan.

### What is included

The self-hosted LangSmith docs describe a full stack that runs in your infrastructure, including:

- LangSmith frontend
- LangSmith backend
- LangSmith platform backend
- LangSmith queue
- LangSmith Playground
- LangSmith ACE backend

The documented storage layer includes:

- ClickHouse for traces and feedback
- PostgreSQL for operational data
- Redis or Valkey for queuing and caching
- optional blob storage for large artifacts

The docs also provide cloud-specific self-hosted guides for:

- AWS
- GCP
- Azure

### Operational model

LangSmith self-hosting is a real platform operations commitment. You own:

- infrastructure management
- upgrades
- storage services
- network exposure for the frontend
- production operations of the stack

The docs also mention that you can optionally enable LangSmith Deployment on top of the self-hosted base to deploy and manage agents through the LangSmith UI.

### Practical interpretation

LangSmith is the strongest option if you want to run the observability and evaluation platform itself inside your own infrastructure.

The tradeoff is complexity. It appears to be the heaviest self-hosted operational footprint of the three.

## Braintrust

### Documented self-hosting model

Braintrust documents a self-hosted deployment option, but it is not a fully self-hosted platform in the same sense as LangSmith.

Braintrust uses a split architecture:

- self-hosted data plane
- Braintrust-managed control plane

### What runs in your infrastructure

The docs state that the data plane stores sensitive data, including:

- experiment records
- logs
- traces
- spans
- datasets
- prompt completions

The data plane consists of:

- Braintrust API
- PostgreSQL
- Redis
- object storage
- Brainstore, Braintrust's high-performance query and ingestion engine

The docs say that when you self-host Braintrust, you deploy the data plane in your own infrastructure using Terraform.

Deployment options documented:

- AWS: Terraform with Lambda and EC2
- GCP: Terraform with Kubernetes and Helm
- Azure: Terraform with Kubernetes and Helm

### What remains vendor-managed

The control plane remains Braintrust-managed. According to the docs, it includes:

- web UI
- authentication
- user management
- metadata storage and platform settings

The docs also note that self-hosted deployments send telemetry back to the Braintrust-managed control plane by default for health monitoring, metrics, and billing usage telemetry.

### Operational model

Braintrust documents:

- shared responsibility for uptime
- infrastructure monitoring through automatic telemetry and an infra dashboard
- recurring upgrades of the data plane
- detailed hardware requirements
- optional remote access for faster support resolution

The self-hosted docs are operationally mature and detailed, especially around deployment and performance requirements.

### Practical interpretation

Braintrust is a strong choice if your main requirement is to keep sensitive AI data in your own cloud account or private network while still using the Braintrust-managed control plane.

This is not a pure end-to-end self-hosted model. It is better described as vendor-managed control plane plus customer-managed data plane.

## Arize

### Documented self-hosting model from reviewed pages

From the AX docs reviewed, I did not find a clearly documented self-hosted deployment model for Arize AX itself.

What was clearly documented is self-hosting for Phoenix.

The Phoenix materials reviewed describe:

- self-host Phoenix locally
- run Phoenix with Docker
- deploy Phoenix on Kubernetes
- deploy Phoenix in your own cloud

The Phoenix site also explicitly states:

- your traces stay in your environment
- Phoenix is open source
- you can deploy anywhere in seconds

### What this means for comparison

Arize clearly has a strong self-hosted story through Phoenix.

However, that is not the same as saying that Arize AX, the hosted AI engineering platform discussed in the main comparison document, has a clearly documented equivalent self-hosted deployment model based on the pages reviewed here.

So the careful interpretation is:

- Arize has a strong self-hosted and open-source option through Phoenix
- the reviewed AX pages did not clearly establish AX as a self-hosted enterprise platform in the same way LangSmith and Braintrust do for their hosted products

### Practical interpretation

If your requirement is self-hosting and you are open to the Phoenix product path, Arize is strong.

If your requirement is specifically a self-hosted equivalent of the hosted AX platform, that was not clearly established from the reviewed material.

## Best fit by self-hosting requirement

### 1. I want the full platform in my infrastructure

Best fit: LangSmith

Why:

- clearest documented full self-hosted platform model
- full service and storage stack runs in your environment
- documented cloud-specific deployment guides

### 2. I want my sensitive AI data in my infrastructure, but I am okay with vendor-managed UI and control plane

Best fit: Braintrust

Why:

- designed specifically around self-hosted data plane plus managed control plane
- strong data residency and private-network story
- detailed deployment guidance across AWS, GCP, and Azure

### 3. I want a strong self-hosted open-source option

Best fit: Arize Phoenix

Why:

- explicit self-hosting paths for local, Docker, Kubernetes, and cloud
- open-source product path
- keeps traces in your environment

## Bottom line

The vendors differ in kind, not just in degree.

- LangSmith is the clearest choice for a fully self-hosted platform deployment.
- Braintrust is the clearest choice for a self-hosted data plane with vendor-managed control plane.
- Arize is the clearest choice for a self-hosted open-source path through Phoenix, but not clearly a documented self-hosted AX equivalent from the pages reviewed.

## Caveats

- This comparison is based only on documentation reviewed on 2026-06-09.
- It does not include private enterprise materials or sales guidance.
- Arize AX may have enterprise deployment options not established in the reviewed AX docs.
- Braintrust and LangSmith self-hosting options may have additional contractual or support requirements beyond what is stated in the public docs.

## Sources reviewed

- LangSmith platform setup: https://docs.langchain.com/langsmith/platform-setup
- LangSmith self-hosted: https://docs.langchain.com/langsmith/self-hosted
- Braintrust manage organizations: https://www.braintrust.dev/docs/admin/organizations
- Braintrust self-hosting: https://www.braintrust.dev/docs/admin/self-hosting
- Braintrust plans and limits: https://www.braintrust.dev/docs/plans-and-limits
- Arize AX overview: https://arize.com/docs/ax
- Arize skills and MCP: https://arize.com/docs/ax/set-up-with-ai-assistants
- Arize Phoenix: https://arize.com/phoenix