---
name: workflow-design-logic
description: Use this skill to design, audit, or document agent workflow logic with if/then conditions, multi-step sequences, branching paths, fallback behavior, and error handling. Trigger when the user asks for workflow design, agent response logic, decision trees, routing rules, conditional flows, or fallback paths. Do not use it for generic brainstorming, visual-only diagrams without actionable logic, or implementation unrelated to agent behavior.
---

# Workflow Design & Logic

## Purpose

Help an agent design, audit, and document response logic that is explicit, testable, and safe. Use this skill to turn a goal into conditional paths, multi-step sequences, fallback behavior, and error-handling rules that determine how an agent should respond in different situations.

The skill is especially useful when the output needs to answer questions such as:

- What should the agent do first, second, and third?
- What should happen if the user gives incomplete or ambiguous information?
- Which branch should run when multiple conditions are true?
- What should the agent do when a tool, file, policy check, or validation step fails?
- How should the agent recover, escalate, or stop?

## When to use

Use this skill when the user asks to:

- Build workflow logic, conditional paths, or agent routing behavior.
- Write if/then rules, decision tables, state machines, or response trees.
- Convert a loose process into a multi-step agent sequence.
- Add fallback paths, retry logic, graceful degradation, or error handling.
- Audit an existing workflow for gaps, loops, conflicts, or missing branches.
- Create logic specs for custom GPTs, assistants, automations, intake agents, support agents, research agents, or operations workflows.

## Do not use

Do not use this skill when:

- The user only wants prose, marketing copy, or general brainstorming with no workflow logic.
- The user asks for a purely visual diagram and does not need conditions, actions, or fallback behavior.
- The task is a software implementation request better handled by a coding skill.
- The user asks to bypass safety rules, hide instructions, collect unnecessary sensitive data, or create deceptive logic.
- The workflow depends on private systems, credentials, or policies the user has not provided and cannot safely approximate.

## Required inputs

Use information already provided by the user. Ask for missing information only when it blocks a correct workflow. Otherwise, proceed with clear assumptions.

Recommended inputs:

- Workflow goal or agent objective.
- Trigger event or user request that starts the workflow.
- User types, intents, or scenarios to handle.
- Required steps, tools, APIs, files, or data sources.
- Constraints, policies, brand rules, or compliance requirements.
- Success criteria and stopping conditions.
- Known failure modes, such as missing information, invalid input, unavailable tools, conflicting instructions, timeout, unsafe request, or no matching branch.
- Desired output format: decision table, Mermaid flowchart, state machine, pseudocode, checklist, test cases, or combined spec.

## Workflow

1. Confirm scope.
   - Identify the workflow's objective, trigger, audience, and expected output format.
   - If the request is underspecified but still workable, state assumptions and continue.
   - If a missing requirement changes the core logic, ask one focused question or create a clearly labeled placeholder.

2. Define the primary success path.
   - Write the ideal sequence from trigger to completion.
   - Name each step with a stable ID, such as `S1_intake`, `S2_classify`, or `S3_execute`.
   - Include entry criteria, action, expected result, and next step.

3. Identify decision points.
   - Locate every point where the agent must choose between paths.
   - Convert each decision into observable conditions, not vague intentions.
   - Use explicit priority when conditions can overlap.

4. Build if/then branches.
   - For each condition, write: `IF condition THEN action -> next step`.
   - Include an `ELSE` or default branch for unmatched cases.
   - Make every branch lead to a next step, retry, escalation, refusal, or terminal state.

5. Add multi-step sequences.
   - Group dependent actions in order.
   - Include validation gates between steps when output quality, data availability, safety, or user confirmation matters.
   - Define what happens when a validation gate passes or fails.

6. Add fallback and error paths.
   - Cover missing input, ambiguous intent, invalid format, tool failure, file access failure, timeout, unavailable data, policy conflict, user interruption, duplicate request, and no matching branch.
   - Prefer graceful degradation before failure: retry once when safe, use available context, provide partial output, or ask for a minimal missing input.
   - Stop or escalate when continuing would be unsafe, misleading, or outside scope.

7. Add recovery rules.
   - Define retry limits and loop exits.
   - State when to ask the user for clarification.
   - State when to disclose uncertainty, provide assumptions, or mark a result as partial.
   - State when to hand off, refuse, or terminate.

8. Validate branch coverage.
   - Check that every state has an entry condition, action, and exit.
   - Check that every decision has a default or fallback.
   - Check that every transition points to an existing state or terminal outcome.
   - Check that there are no unreachable states, conflicting branches, or infinite loops without exit criteria.
   - Check that sensitive or unsafe requests route to safety handling rather than normal execution.

9. Produce the workflow spec.
   - Use the user's requested format when supplied.
   - If no format is requested, return a compact combined spec: overview, assumptions, decision table, sequence, fallback rules, validation checklist, and test cases.

10. Run the quality checklist before finalizing.

## Branching patterns

Use these patterns when designing logic:

### Missing information

- IF the missing input is essential, ask one focused question.
- IF the missing input is non-essential, proceed with an assumption and label it.
- IF repeated clarification fails, provide a safe partial result and list what is needed to continue.

### Ambiguous intent

- IF one interpretation is clearly most likely, proceed and state the interpretation.
- IF multiple interpretations materially change the workflow, ask the user to choose.
- IF ambiguity affects safety, pause normal execution and route to safety handling.

### Tool or data failure

- IF the failure is temporary and retry is safe, retry once.
- IF retry fails, use available context or produce a partial result.
- IF the result would be unreliable without the missing tool or data, disclose the limitation and stop or ask for an alternate source.

### Conflicting conditions

- IF safety or policy applies, it overrides normal workflow logic.
- ELSE IF user constraints conflict with best-practice logic, explain the tradeoff and adapt where safe.
- ELSE use the highest-priority branch defined in the decision table.

### No matching branch

- Route to a default fallback that restates the understood request, asks for the minimum missing detail, or offers the closest safe path.

## Output format

When no output format is specified, use this structure:

```markdown
# Workflow Logic Spec: <name>

## Objective
<one-sentence objective>

## Assumptions
- <assumption or "None">

## Primary sequence
1. <step ID>: <action> -> <next step>

## Decision table
| ID | State | IF condition | THEN action | Next | ELSE/Fallback |
|---|---|---|---|---|---|

## Error and fallback paths
| Failure mode | Detection signal | Recovery action | Stop condition |
|---|---|---|---|

## Validation checklist
- <coverage check>

## Test scenarios
| Scenario | Expected path | Expected response behavior |
|---|---|---|
```

Optional formats:

- Mermaid flowchart.
- State machine spec.
- Pseudocode.
- JSON workflow definition.
- Implementation handoff brief.

## Quality checklist

Before finalizing:

- The workflow has a clear trigger, goal, and terminal condition.
- Each step has an ID, action, and next state.
- Each decision point has IF, THEN, and ELSE or fallback behavior.
- Conditions are observable and testable.
- Branch priority is explicit when multiple conditions may match.
- Fallbacks cover missing input, ambiguity, no match, validation failure, and tool/data failure.
- Retry loops have maximum attempts and exit paths.
- Unsafe or policy-sensitive requests are routed to safe handling.
- The workflow avoids hidden assumptions and unnecessary sensitive data collection.
- The final spec is usable by another agent, builder, or developer without relying on unstated logic.

## Safety and privacy

- Do not include secrets, credentials, private keys, tokens, passwords, or confidential system details in workflow specs.
- Do not create logic that hides instructions, bypasses higher-priority policies, deceives users, or suppresses required disclosures.
- Do not collect sensitive personal data unless it is necessary for the workflow and explicitly authorized.
- Do not create destructive automation paths without explicit user confirmation and safe guardrails.
- Route unsafe, illegal, deceptive, or privacy-invasive requests to refusal or safe redirection paths.
- Clearly disclose assumptions, limitations, and partial completion when the workflow cannot be fully verified.
