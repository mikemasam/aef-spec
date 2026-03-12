# Agent Execution Framework (AEF) — State Machine for AI Agents

## What is an Agent Execution Framework?

An Agent Execution Framework (EF) governs how an AI agent:
- Maintains awareness of its current situation (state)
- Responds to inputs (events)
- Takes appropriate actions

Think of this as the "operating system" for your ai agent.

---

## Concepts

### State

A finite condition representing what the agent is currently doing or waiting for.

**Entry:**

- What runs when entering this state (initialize, start timers, etc.)

**Actions:**

- What the agent does while in this state (no transitions)

**Status:**

- Current condition indicator

**Exit:**

- What triggers leaving this state → next state

**Default:**

- The state to return to when no other transition applies (typically IDLE)

---

### Event
Something that happens — user input, timer triggers, external data.

### Action
What the agent does in response to an event, either when entering a state or leaving it.

### Transition
Moving from one state to another, triggered by an event.

---

## State Machine Structure

```
$STATE_A --[event]--> $STATE_B

$STATE_A:
  Entry: ...
  Actions: ... 
  Status: ...
  Exit: event --> $STATE_B
  Default: $DEFAULT_STATE
```

---

## Common States (Examples)

These are typical states you might need. Pick what fits your use case.

| State | Purpose |
|-------|---------|
| `IDLE` | Waiting for input or new work |
| `PROCESSING` | Actively working on a task |
| `WAITING` | Blocked, awaiting external response |
| `ERROR` | Something went wrong |
| `PAUSED` | Temporarily halted, can resume |
| `COMPLETE` | Finished successfully |
| `AWAITING_INPUT` | Need more information from user |

---

## Event Types (Examples)

Define specific events for your use case. Generic events like `USER_INPUT` should be specialized:

| Event | When it fires |
|-------|---------------|
| `USER_SENDS_MESSAGE` | User sends a message |
| `USER_PROVIDES_TASK` | User adds a task |
| `USER_MARKS_DONE` | User marks task complete |
| `TIMER` | A timeout or scheduled trigger |
| `EXTERNAL_DATA` | Webhook, API response, file change |
| `ERROR_OCCURRED` | Something failed |
| `USER_CONFIRMS` | User approves something |
| `USER_DENIES` | User rejects something |

---

## Transition Rules

### Valid Transitions

Each state should clearly define:
1. **What events** can trigger a transition out of this state
2. **What conditions** must be met (guards)
3. **What happens** during the transition (actions)


## Entry and Exit Actions

Every state can have entry and exit actions defined in the state definition:

**Entry actions** — run when entering the state
- Initialize variables
- Start timers
- Send initial response

**Exit actions** — run when leaving the state
- Clean up resources
- Stop timers
- Log state changes

---

## Designing Your State Machine

### Step 1: List all possible situations
What can your agent be doing? Start with 3-5 main states.

### Step 2: Define transitions
For each state, define: "What can happen next?"

### Step 3: Add actions
What must happen when entering/exiting each state?

### Step 4: Handle edge cases
- What if something fails?
- What if the user goes silent?
- What if input is unexpected?

---

## Examples

- [Example 1: Simple To-Do Assistant](example-1-todo.md) — Basic 3-state cycle
- [Example 2: Multi-Step Task Handler](example-2-multi-step.md) — Workflow with external API calls
- [Example 3: Context-Aware Assistant](example-3-context-aware.md) — Memory management and interruptions

---

## Summary

An Execution Framework gives:

- **Where** the agent is (state)
- **What** triggered the move (event)
- **What** happens during transitions (action)

---
