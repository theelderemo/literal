# Literal-to-MacroDroid Mapping (MAP.md)

This document outlines the mapping between Literal language constructs and their corresponding concepts and automation engines. The Literal compiler uses this logic to generate platform compatible JSON.

---

# MacroDroid

## Variable Mapping

| Literal Construct | MacroDroid Equivalent | Notes |
|---|---|---|
| `set my_var to ...` | Local Variable | A variable private to the specific macro run. It is created at the start of the action block and destroyed at the end. |
| `set persistent my_var to ...` | Global Variable | A MacroDroid Global Variable that persists across all macros and device reboots. The compiler will generate actions to `Set MacroDroid Variable` for this. |

---

## Trigger Mapping

| Literal Construct | MacroDroid Equivalent | Compilation Strategy |
|---|---|---|
| `when battery level is less than 15:` | Trigger: `Battery Level` | The compiler maps this to the `Battery Level` trigger with the "Below %" option. |
| `at "7:00 AM" on "weekdays":` | Trigger: `Day/Time Trigger` | The compiler configures the trigger for the specific time and checks the boxes for the specified days. |
| `every 15 minutes:` | Trigger: `Regular Interval` | The compiler sets the interval to 15 minutes. |
| `at "10:00 PM" if location is "Home":` | Trigger with Constraint | The compiler creates an `at "10:00 PM"` trigger and adds a `Location` constraint to it, set to the "Home" area. **This is a native trigger constraint, not an `If` statement in the actions.** |

---

## Error Handling Mapping

| Literal Construct | MacroDroid Equivalent | Compilation Strategy |
|---|---|---|
| (No `on error` block) | `Stop Actions` | If a command fails (e.g., a required app is not installed), the default behavior is to stop further execution of the macro. |
| `on error with error_message:` | Action Block with `try/catch` Logic | This is a more complex compilation. The compiler wraps the action in a way that it can catch a failure. It then uses a MacroDroid `If` condition on a success/failure variable. The `error_message` is populated from a known error output of the failed action. This feature may have limited support depending on the specific action. |

---

## Action & Condition Mapping

| Literal Construct | MacroDroid Equivalent |
|---|---|
| `send text message to ...` | Action: `Send SMS` |
| `set brightness to 20` | Action: `Set Brightness` |
| `if screen is on:` | Condition: `Screen On/Off` |
| `if time is between ...` | Condition: `Time of Day` |

*(This document is a living specification and will be expanded as more mappings are defined.)*
