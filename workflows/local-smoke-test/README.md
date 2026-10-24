# Local smoke test

A minimal **Manual Trigger → JavaScript Code** workflow that checks local execution. It comes from the saved smoke test in the companion lab, with instance identity and private metadata removed for portability.

## Prerequisites

- n8n **2.44.6**, the initially verified version.
- A working JavaScript task runner. [n8n-lab](https://github.com/Light2288/n8n-lab/tree/main) already configures an external runner.
- No credentials, API keys, external services, network requests, or sample inputs.

## Import and run

1. Open [the local editor](http://localhost:5678) and create a **new workflow**.
2. In its actions menu choose **Import → From file**, then select [workflow.json](workflow.json).
3. Save the copy with a recognizable name; keep it unpublished.
4. Click **Execute workflow**. Open **Code in JavaScript** to inspect its output.

Expected result: **one item**, whose JSON is:

```json
{
  "message": "Local n8n works",
  "answer": 42
}
```

The Code node constructs this object directly and computes `6 * 7`. There are no scheduled triggers or external side effects. See the [shared workflow lifecycle](../../README.md#export-review-and-commit) before committing an edited export.
