# Evaluation Capability Comparison: Arize AX vs Braintrust vs LangSmith

This note compares evaluation capabilities among Arize AX, Braintrust, and LangSmith based on their public documentation.

## Executive summary

All three platforms support the core evaluation loop:

- human review
- offline evaluation on curated datasets or experiments
- online evaluation on production traces
- LLM-as-a-judge evaluators
- deterministic code-based evaluators

The main differences are in emphasis:

- Braintrust presents the strongest end-to-end evaluation workflow, with explicit support for playground iteration, immutable experiments, CI/CD regression testing, online scoring, classifiers, and review operations.
- Arize AX presents evaluation as part of a broader debugging and improvement loop, tightly connecting trace review, annotation, experiments, and production monitoring.
- LangSmith offers a balanced evaluation stack with strong experiment comparison, reusable evaluators, and especially strong documented pairwise evaluation workflows.

## Quick comparison

| Capability | Arize AX | Braintrust | LangSmith |
| --- | --- | --- | --- |
| Offline evaluation | Yes, on experiments with golden datasets | Yes, on datasets and experiments | Yes, on datasets and experiments |
| Online evaluation | Yes, continuous tasks on traces | Yes, online scoring on production traces | Yes, online evaluators on runs and threads |
| LLM-as-judge | Yes | Yes | Yes |
| Code-based evaluators | Yes, Python code evaluators | Yes, custom code scorers | Yes, code evaluators |
| Prebuilt evaluators | Some prebuilt templates in Eval Hub | Strongest documented prebuilt story via autoevals | Templates for LLM-as-judge evaluators |
| Classification support | Human annotations and evaluator labels | Explicit classifiers as first-class feature | Categorical feedback supported in evaluators |
| Human review | Yes, annotations and labeling queues | Yes, strong review workflow with assignments and multiple reviewers | Yes, inline review plus annotation queues |
| Pairwise evaluation | Not emphasized in reviewed docs | Possible through human review and experiments, but not surfaced as a flagship evaluation mode in the reviewed docs | Explicit automated pairwise evals and pairwise annotation queues |
| CI/CD regression workflow | Yes | Yes | Yes |
| Best-documented evaluation emphasis | Debugging and improvement loop | End-to-end evaluation operating system | Balanced eval + observability with strong experiment comparison |

## Arize AX

Arize AX frames evaluation as part of an AI improvement loop:

1. observe traces
2. annotate and review failures
3. form a hypothesis
4. run experiments
5. measure with evaluators
6. keep the same criteria running online after deployment

Key evaluation capabilities documented:

- Versioned evaluators with reusable definitions
- Evaluator scopes at span, trace, session, and experiment level
- LLM-as-a-judge evaluators
- Python code evaluators
- Online evaluation tasks on production traces with sampling and filtering
- Offline evaluation on experiments using the same evaluators
- Side-by-side comparison of experiment runs with evaluation results
- Human annotations on spans, dataset examples, and experiment results
- Labeling queues for routed review workflows and benchmark curation
- CI/CD workflow guidance for running experiments and comparing scores before merge

Where Arize stands out:

- Evaluation is tightly connected to debugging and trace analysis.
- The same evaluator definition is reused across online monitoring and offline experiments.
- Scope is richer than simple request-level scoring because the docs explicitly include span, trace, session, and experiment targets.

Where Arize is less differentiated in the reviewed docs:

- Pairwise evaluation is not surfaced as a major first-class workflow the way it is in LangSmith.
- The docs emphasize evaluator reuse and improvement loops more than a large catalog of specialized eval primitives.

## Braintrust

Braintrust presents the most evaluation-centric workflow in the reviewed docs.

Its documented evaluation cycle is:

1. iterate in playgrounds
2. promote to immutable experiments
3. automate in CI/CD
4. score production traces continuously
5. feed production findings back into datasets

Key evaluation capabilities documented:

- Offline evaluation on datasets before deployment
- Immutable experiments for comparison over time
- Browser playgrounds for rapid iteration
- Remote evals and sandboxes for complex agents or custom code
- Scorers that return numeric values
- Classifiers that return categorical labels
- Three scorer types: autoevals, LLM-as-a-judge, custom code
- Online scoring rules for production traces
- Span and trace scope for scorers and classifiers
- SQL filters, sampling, and rule preview for production scoring
- Rewind support for trace-scoped automations
- Human review workflows across logs, experiments, and datasets
- Assignment workflows, review scores, multiple reviewers, and dataset curation from reviewed logs

Where Braintrust stands out:

- Strongest documented end-to-end evaluation workflow.
- Broadest explicit evaluator taxonomy in one place: autoevals, LLM judges, custom scorers, classifiers.
- Strong review operations for teams doing structured human evaluation at scale.
- Strong production evaluation controls, including filters, sampling, and automation lifecycle features.

Where Braintrust is less differentiated:

- Pairwise comparison exists conceptually through experiment and review flows, but the reviewed docs do not foreground automated pairwise evals as clearly as LangSmith does.

## LangSmith

LangSmith presents evaluation as a lifecycle spanning development and production.

The documented lifecycle is:

1. use offline evals during development
2. use online evals after deployment
3. feed online findings back into offline datasets for continuous improvement

Key evaluation capabilities documented:

- Offline evaluations on datasets and examples
- Experiments capturing outputs, evaluator scores, and traces for each example
- Side-by-side comparison of multiple experiments
- Online evaluations on production runs and threads
- Reusable workspace-level evaluators
- Human evaluation via inline annotations and annotation queues
- Code evaluators
- LLM-as-a-judge evaluators
- Pairwise evaluators for automated comparison between experiments
- Pairwise annotation queues for human A/B review
- Reference-based and reference-free evaluator patterns
- Dataset versions and splits for evaluation organization
- Online evaluator filters, sampling, and optional backfill

Where LangSmith stands out:

- Best documented pairwise evaluation workflow.
- Strong experiment comparison and dataset/versioning model.
- Clear distinction between offline reference-based evaluation and online reference-free monitoring.
- Human review is well integrated with both single-run and pairwise annotation queues.

Where LangSmith is less differentiated:

- Compared with Braintrust, the review and scoring workflow reads somewhat less like a dedicated review operations system.
- Compared with Arize, evaluation is less explicitly framed as part of a debugging-first improvement loop.

## Bottom line

If your primary question is which vendor is strongest specifically on evaluation:

1. Braintrust has the strongest documented end-to-end evaluation system.
2. LangSmith is very strong and especially differentiated on pairwise evaluation and experiment comparison.
3. Arize AX is also strong, but its evaluation story is most differentiated when paired with debugging, trace inspection, and iterative quality improvement.

If your team is debugging agent failures and turning those findings into evaluators, Arize AX is especially compelling.

If your team wants evaluation itself to be the operating center of the workflow, Braintrust is the clearest fit.

If your team wants strong evaluation plus strong observability and excellent comparison workflows, LangSmith is a strong fit.

## Sources reviewed

Arize AX:

- https://arize.com/docs/ax/core-workflows
- https://arize.com/docs/ax/evaluate/create-evaluators
- https://arize.com/docs/ax/evaluate/run-evals-on-traces
- https://arize.com/docs/ax/evaluate/run-evals-on-experiments
- https://arize.com/docs/ax/evaluate/human-review

Braintrust:

- https://www.braintrust.dev/docs/evaluate
- https://www.braintrust.dev/docs/evaluate/write-scorers
- https://www.braintrust.dev/docs/evaluate/score-online
- https://www.braintrust.dev/docs/annotate/human-review

LangSmith:

- https://docs.langchain.com/langsmith/evaluation
- https://docs.langchain.com/langsmith/evaluation-concepts
- https://docs.langchain.com/langsmith/llm-as-judge
- https://docs.langchain.com/langsmith/evaluate-pairwise
- https://docs.langchain.com/langsmith/annotation-queues
- https://docs.langchain.com/langsmith/online-evaluations-llm-as-judge