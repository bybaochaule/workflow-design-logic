# Workflow Design & Logic

## Overview

This skill helps an agent design, audit, and document workflow logic for agent behavior. It focuses on if/then conditions, multi-step sequences, branching paths, fallback behavior, validation gates, and error handling.

Use it when a user needs clear logic for how an agent should respond across different situations, including normal success paths, missing information, ambiguity, tool failure, policy conflicts, and recovery paths.

## When to use

Use this skill for requests such as:

- "Design the workflow logic for my agent."
- "Create if/then branches for this assistant."
- "Map the fallback and error paths."
- "Turn this process into a decision tree."
- "Audit this workflow for missing branches."
- "Write the multi-step response sequence for a support or intake agent."

## Contents

- `SKILL.md`: agent-facing instructions, trigger description, boundaries, workflow, output formats, quality checklist, and safety rules.
- `README.md`: human-readable overview of the skill package.
- `agents/openai.yaml`: OpenAI metadata and invocation policy.
- `assets/workflow-template.json`: reusable JSON template for workflow logic specs.
- `references/style-guide.md`: workflow-writing rules, branch patterns, examples, and validation guidance.
- `scripts/example_helper.py`: deterministic validator for workflow JSON files.

## Package structure

```text
workflow-design-logic/
|-- SKILL.md
|-- README.md
|-- agents/
|   `-- openai.yaml
|-- assets/
|   |-- .gitkeep
|   `-- workflow-template.json
|-- references/
|   `-- style-guide.md
`-- scripts/
    `-- example_helper.py
```

## Suggested output produced by the skill

When no format is specified, the skill produces a compact workflow logic spec with:

1. Objective.
2. Assumptions.
3. Primary sequence.
4. Decision table.
5. Error and fallback paths.
6. Validation checklist.
7. Test scenarios.

## Using the validator

The helper script validates JSON workflow definitions that follow the template in `assets/workflow-template.json`.

```bash
python scripts/example_helper.py assets/workflow-template.json
```

The validator checks for:

- Missing required fields.
- Duplicate state IDs.
- Transitions that point to missing states.
- Missing fallback/default behavior.
- Unreachable states.
- States that cannot reach a terminal outcome.

## Notes

This package is intentionally implementation-neutral. It can support custom GPTs, OpenAI agents, support bots, intake assistants, research assistants, and operations automations without requiring a particular runtime or API.
