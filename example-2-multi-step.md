# Example 2: Multi-Step Task Handler

An agent that handles a workflow with external API calls.

---

## $IDLE

**Entry:**

- Agent ready

**Actions:**

- None

**Status:** Waiting for user request

**Exit:**

- User makes request → $GATHERING

**Default:** $IDLE

---

## $GATHERING

**Entry:**

- User request received

**Actions:**

- Ask clarifying questions

**Status:** Gathering requirements

**Exit:**

- Enough info → $PROCESSING
- User provides more info → $GATHERING

---

## $PROCESSING

**Entry:**

- Requirements gathered

**Actions:**

- Process data

**Status:** Working on task

**Exit:**

- Needs external call → $WAITING_API
- Done → $RESPONDING
- Error occurred → $ERROR

---

## $WAITING_API

**Entry:**

- API call initiated

**Actions:**

- Poll for response

**Status:** Blocked on external service

**Exit:**

- API responds → $PROCESSING

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

## $ERROR

**Entry:**

- Error occurred, increment retry counter

**Actions:**

- Log error details

**Status:** Error state

**Exit:**

- User confirms retry → $PROCESSING
- User denies retry → $RESPONDING

---

## Transitions

```
$IDLE --[USER_MAKES_REQUEST]--> $GATHERING
$GATHERING --[USER_PROVIDES_INFO]--> $PROCESSING
$PROCESSING --[EXTERNAL_DATA]--> $WAITING_API
$WAITING_API --[EXTERNAL_DATA]--> $PROCESSING
$PROCESSING --[TASK_COMPLETE]--> $RESPONDING
$RESPONDING --[RESPONSE_SENT]--> $IDLE
$PROCESSING --[ERROR_OCCURRED]--> $ERROR
$ERROR --[USER_CONFIRMS_RETRY]--> $PROCESSING
$ERROR --[USER_DENIES_RETRY]--> $RESPONDING
```

## Key Points

- Multiple states for different phases
- User confirms or denies retry attempts
- Can loop between $PROCESSING and $WAITING_API
- Graceful failure when user gives up
