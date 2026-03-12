# Example 1: Simple To-Do Assistant

A basic agent that helps manage a single task list.

---

## $IDLE

**Entry:**

- Agent ready

**Actions:**

- None

**Status:** Waiting for user to add task

**Exit:**

- User provides task → $TASK_ACTIVE

**Default:** $IDLE

---

## $TASK_ACTIVE

**Entry:**

- Task received from user

**Actions:**

- Process task request

**Status:** Task is active

**Exit:**

- User marks done → $COMPLETE

---

## $COMPLETE

**Entry:**

- Task marked as done

**Actions:**

- Acknowledge completion
- Clear active task

**Status:** Task completed

**Exit:**

- Timer fires → $IDLE

---

## Transitions

```
$IDLE --[USER_PROVIDES_TASK]--> $TASK_ACTIVE
$TASK_ACTIVE --[USER_MARKS_DONE]--> $COMPLETE
$COMPLETE --[TIMER]--> $IDLE
```

## Key Points

- Single task at a time
- Simple 3-state cycle
- No error handling needed for this complexity
