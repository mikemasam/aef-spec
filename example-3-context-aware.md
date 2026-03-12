# Example 3: Context-Aware Assistant with Memory

An agent that maintains conversation history, handles interruptions, and manages memory.

---

## $IDLE

**Entry:**

- Agent ready

**Actions:**

- None

**Status:** Waiting for user message

**Exit:**

- User sends message → $UNDERSTANDING

**Default:** $IDLE

---

## $UNDERSTANDING

**Entry:**

- User message received

**Actions:**

- Parse user intent

**Status:** Processing input

**Exit:**

- Intent clear → $EXECUTING
- Need more info → $AWAITING_CONFIRMATION

---

## $EXECUTING

**Entry:**

- Task identified

**Actions:**

- Work on task

**Status:** Task in progress

**Exit:**

- Task done → $RESPONDING
- User interrupts → $PAUSED
- Needs user input → $AWAITING_CONFIRMATION

---

## $PAUSED

**Entry:**

- Interrupt detected

**Actions:**

- Store current_task → interrupted_task

**Status:** Paused

**Exit:**

- User resumes → $EXECUTING
- User cancels → $IDLE

---

## $AWAITING_CONFIRMATION

**Entry:**

- Need user input

**Actions:**

- Send prompt

**Status:** Waiting for reply

**Exit:**

- User responds → $UNDERSTANDING
- Timer expires → $IDLE

---

## $RESPONDING

**Entry:**

- Task complete

**Actions:**

- Format response

**Status:** Sending response

**Exit:**

- Response sent → $IDLE

---

## Transitions

```
$IDLE --[USER_SENDS_MESSAGE]--> $UNDERSTANDING
$UNDERSTANDING --[INTENT_CLEAR]--> $EXECUTING
$UNDERSTANDING --[NEED_MORE_INFO]--> $AWAITING_CONFIRMATION
$EXECUTING --[TASK_DONE]--> $RESPONDING
$EXECUTING --[USER_INTERRUPTS]--> $PAUSED
$EXECUTING --[NEEDS_USER_INPUT]--> $AWAITING_CONFIRMATION
$AWAITING_CONFIRMATION --[USER_RESPONDS]--> $UNDERSTANDING
$AWAITING_CONFIRMATION --[TIMER]--> $IDLE
$PAUSED --[USER_RESUMES]--> $EXECUTING
$PAUSED --[USER_CANCELS]--> $IDLE
$RESPONDING --[RESPONSE_SENT]--> $IDLE
```

## State Data

Maintained across all transitions:

| Variable | Type | Description |
|----------|------|-------------|
| `conversation_history` | List | All past messages |
| `current_task` | Object | What's being worked on |
| `interrupted_task` | Object | Paused work (if any) |
| `context_summary` | String | Compacted history |

## Memory Handling

### On Every Response
- Append user/assistant messages to `conversation_history`

### Compaction Trigger
Every 10 messages:
1. Summarize old messages → `context_summary`
2. Keep recent messages in `conversation_history`
3. Archive old messages to cold storage

### Interruption ($PAUSED)
- Save `current_task` → `interrupted_task`
- Clear `current_task`
- Handle new request
- On resume: restore `interrupted_task` → `current_task`

## Key Points

- Handles user interruptions gracefully
- Maintains context across long conversations
- Memory compaction prevents context overflow
- Timeout prevents hanging forever
- Paused tasks can be resumed or cancelled
