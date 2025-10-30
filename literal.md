The Literal Automation Language (v0.7)

---

## Philosophy & Strategy

**Literal is a language that compiles to existing automation engines.** Its purpose is not to be a new runtime, but to be a human-readable, shareable, and version-control-friendly frontend for powerful backends like MacroDroid, Tasker, n8n, and other automation platforms. The end goal is cross-platform export.

This strategy dictates our design:
1.  **Compiler-First:** A feature only exists if it can be reliably compiled to a target platform's format.
2.  **Readability is Paramount:** The syntax must be clear enough for non-programmers to understand the logic.
3.  **Shareability is a Feature:** `.literal` files are meant to be shared, unlike unreadable JSON exports.

---

## Compiler Architecture

The official compiler architecture is designed for modularity and extensibility:
1.  **Lexer & Parser:** Converts `.literal` source code into an Abstract Syntax Tree (AST).
2.  **Semantic Analyzer:** Validates the AST, ensuring that triggers, actions, and conditions are supported by the specified target platform (e.g., checking against `MAP.md`).
3.  **Intermediate Representation (IR) Generator:** Creates a platform-agnostic representation of the automation logic. This IR represents universal automation concepts.
4.  **Target Emitters:** Convert the IR into the specific JSON or data format required by the target platform (e.g., a MacroDroid Emitter, an n8n Emitter).

---

## Language Specification (v0.7)

### Runtime Behavior

- **Unhandled Errors:** If a command that can fail (e.g., an action that requires a missing app) is executed outside of an `on error` block, **the execution of the current trigger will halt immediately.** This is the safest default and prevents unexpected behavior.

### Variables: Transient and Persistent

1.  **Transient (default):** Reset after each trigger run. Corresponds to a *local variable* in the target platform.
    `set my_variable to "Hello"`

2.  **Persistent:** State is saved across trigger runs. This maps directly to the **global variable system** of the target platform (e.g., MacroDroid Global Variables).
    `set persistent visit_counter to 0`

### Error Handling

The `on error` block can capture the specific error message from a failed action.

Syntax:
<command_that_might_fail>
on error with error_message:
    display notification with title "Automation Failed" and message error_message
end on

### Advanced Triggers

**a) Conditional Triggers (Compilation Note):**
Any trigger can have an additional `if` condition. The compiler will attempt to map this to a **native trigger constraint** on the target platform. It will *not* be compiled into a simple `if` statement in the action block. If the target does not support the constraint, the compiler will raise an error.

`at "10:00 PM" if location is "Home": ... end at`

**b) Time Triggers:**
- `at "7:00 AM" on "weekdays":`
- `when time is "sunset":`
- `every 15 minutes:`
- `every 1 hour between "9:00 AM" and "5:00 PM":`

**c) Event Triggers:**
`when a notification arrives from "Gmail":`
`when a call is missed from "555-123-4567":`

### Data Types and Operations
(Data types, expressions, lists, and control flow remain as defined in v0.5.)

### Core Actions and Conditions
(This is a non-exhaustive list designed to map to the capabilities of common automation platforms.)

---

## MVP Implementation Plan

1.  **Compiler:** Create a CLI tool (`literalc`) that takes a `.literal` file and a target platform flag (e.g., `--target macrodroid`) and outputs a compatible `.json` file.
2.  **VS Code Extension:** Develop an extension providing syntax highlighting and potentially autocomplete based on the target's `MAP.md`.
3.  **Documentation:** Maintain a `MAP.md` file for each target, documenting the direct mapping between Literal commands and their corresponding platform-specific implementations.
4.  **Examples:** Create a repository of `.literal` recipes for various targets.
