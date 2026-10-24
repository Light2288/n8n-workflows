# n8n-workflows

Reviewed, portable n8n workflow examples for local experiments. The companion [n8n-lab](https://github.com/Light2288/n8n-lab/tree/main) repository owns the Podman runtime, installation, updates, backups, and recovery.

This repository stores workflow definitions and instructions. The running instance stores its accounts, credentials, workflows, and execution history in its own database. You can import these examples into the existing lab; a separate n8n instance is unnecessary.

## Examples

| Example | Purpose | Requirements |
|---|---|---|
| [Local smoke test](workflows/local-smoke-test/README.md) | Manual Trigger → JavaScript Code, returning a deterministic result | n8n 2.44.6; working JavaScript task runner; no credentials or external services |

Group future examples under `workflows/<descriptive-name>/`, with a `workflow.json` and a README explaining purpose, prerequisites, import, execution, and expected output. Personal automations live here; an officially maintained product integration can belong with that product when it becomes part of its release/support contract.

## Use an example

1. Start the existing [local lab](https://github.com/Light2288/n8n-lab/tree/main#daily-operation) and open [the editor](http://localhost:5678).
2. Clone this repository's implementation branch while the initial work is unmerged:

   ```sh
   git clone --branch chore/workflow-repositories https://github.com/Light2288/n8n-workflows.git
   cd n8n-workflows
   ```

3. Read the example's README and inspect its JSON, triggers, and destinations.
4. Create a **new workflow**, open its actions menu, and select **Import → From file**. Select the example's `workflow.json`; menu labels can vary by n8n version.
5. Give the imported copy a recognizable name and configure any required local credentials. The smoke test needs none.
6. Execute manually and compare its result with the documented output. Review schedules and destinations before publishing any future workflow. Importing a file is separate from publishing or enabling triggers.

The explicit branch above contains the reviewed examples before a separate merge into main. After that merge, main can become the normal clone target.

## Export, review, and commit

1. Edit, save, and test your workflow in the editor.
2. Use **Export JSON** in its actions menu (called **Download** in some versions). Keep the raw export in a private location outside the checkout until reviewed.
3. Inspect the complete JSON, including Code nodes and request parameters. Remove private `pinData`, sensitive samples, tokens, API keys, passwords, authorization headers, and private URLs. Unpinning data alone does not sanitize the rest of a definition.
4. Remove unnecessary instance identity and ownership metadata: workflow/version/instance IDs, timestamps, shared-owner details, and private tags. Retain functional node definitions, connections, settings, and internal node IDs. Review credential references too: IDs and names can disclose private information even when secret values are omitted. Document how importers select their own credentials.
5. Keep the portable example inactive/unpublished. Import it into a new workflow and verify it before replacing the tracked JSON. Do not overwrite the original during verification.
6. Move only the reviewed definition into its example directory and update the README when its behavior or prerequisites change.
7. Inspect exactly what Git will publish, then commit on a branch:

   ```sh
   git diff -- workflows/
   git add -- workflows/your-example/workflow.json workflows/your-example/README.md
   git diff --cached -- workflows/your-example/
   git diff --cached --check
   git commit -m "feat: add reviewed workflow example"
   ```

For new files, inspect the complete contents before staging; the staged diff then includes the full addition. Repeat the review for every export. Ignore rules exclude common local/private paths; they do not detect secrets embedded in an otherwise trackable JSON file. Do not force-add private or credential exports.

## Applying Git changes to n8n

**Saving in the editor updates the database; committing JSON updates Git. Neither operation updates the other.** A `git pull` does not update the running workflow. Import the chosen JSON explicitly, preferably into a new copy for review, and configure credentials locally. Export again to capture later editor changes.

Workflow JSON is not an instance backup. Accounts, credential secrets, settings, and execution history need the lab's [backup and recovery procedure](https://github.com/Light2288/n8n-lab/blob/main/docs/backup-and-restore.md), including secure retention of its encryption key.

There is no automated Git synchronization, credential provisioning, or deployment service in this repository.
