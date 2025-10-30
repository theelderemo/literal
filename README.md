# Literal: The Readable Automation Language

**Write powerful automations for your devices in a language you can actually read.**

[![Project Status: Pre-Alpha](https://img.shields.io/badge/status-pre--alpha-red)]()
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)]()
[![Contributions Welcome](https://img.shields.io/badge/contributions-welcome-brightgreen.svg)]()

---

## What is Literal?

Literal is a simple, readable, domain specific language for personal and device automation. It's designed to be written and understood by everyone, from tech enthusiasts to busy professionals.

Instead of building another automation app, **Literal compiles to the formats of powerful, existing automation engines** like MacroDroid, Tasker, and n8n. This gives you the best of both worlds: a simple, text-based writing experience and the proven power of a mature automation backend.

My philosophy is simple:

- **Readability is Paramount:** Automation recipes should be as clear as a checklist.
- **Shareability is a Feature:** `.literal` files are easy to read, share, and version control with tools like Git.
- **No Magic:** The language is explicit and predictable. What you see is what you get.

## A Quick Example

Here's a simple recipe that saves battery when you're at work.

```literal
(This recipe tests persistent variables, time comparisons, and device control.)

set persistent low_battery_count to 0

when battery level is less than 15:
    if time is between "9:00 AM" and "5:00 PM":
        (At work - aggressive power saving)
        set brightness to 20
        set wifi to off
        display notification with title "Battery Saver" and message "Work mode active"
    otherwise:
        (At home or night - moderate saving)
        set brightness to 40
        set bluetooth to off
    end if

    set persistent low_battery_count to low_battery_count plus 1
end when
```

## Key Features

- **Persistent Variables:** Keep track of state between runs with the `persistent` keyword.
- **Advanced Triggers:** Create automations based on time, events, location, and device state, with complex conditions (`at "10 PM" if location is "Home":`).
- **Readable Syntax:** No cryptic symbols or confusing syntax. Just plain, explicit commands.
- **Detailed Error Handling:** A simple `on error with error_message:` block lets you build robust, fallback logic.
- **Rich List Operations:** A full, consistent suite of tools for working with lists of items.

## How It Works: The Compiler

Literal is not a runtime. It's a compiler built with a modern, modular architecture.

1.  **Lexer & Parser:** Converts `.literal` code into an Abstract Syntax Tree (AST).
2.  **Semantic Analyzer:** Validates the code against the capabilities of the target platform (e.g., MacroDroid).
3.  **Intermediate Representation (IR):** Creates a platform-agnostic model of the automation logic.
4.  **Target Emitters:** Converts the IR into the specific JSON or data format required by the target platform.

This architecture will allow us to add new compilation targets in the future without rewriting the core logic.

## Project Status Goals

Literal is currently in the design and specification phase. The MVP is focused on a single target: **MacroDroid**.

1. [ ] **Compiler:** A CLI tool (`literalc`) that compiles `.literal` files into MacroDroid-compatible `.json` files.
2. [ ] **VS Code Extension:** A basic extension for syntax highlighting in `.literal` files.
3. [ ] **Example Library:** A public repository of `.literal` recipes to get users started.

**Documentation:** A complete `MAP.md` file documenting the mapping between Literal and MacroDroid. More will be added as project expands.

## Future Goals

- **More Emitters:** Create compilers for other popular automation platforms like Tasker, n8n, and iOS Shortcuts.
- **Expanded Vocabulary:** Add more triggers, actions, and conditions as supported by the target platforms.
- **Advanced IDE Integration:** Move beyond syntax highlighting to include autocomplete, error checking, and inline documentation.
- **Two-Way Compilation:** Explore the possibility of a tool that can decompile a platform's native format back into a `.literal` file.

## Why Not Just Use MacroDroid's and other platforms built in Editor?

MacroDroid is fantastic, and Literal doesn't replace it - it enhances it. Here's what Literal adds:

- **Shareability:** Post `.literal` files in Reddit threads, send them in messages, or publish them as gists. Most JSON exports are unreadable to non programmers.
- **Version Control:** Track changes with Git, see diffs between versions, collaborate on complex automations.
- **Writeability:** Type on a real keyboard with your preferred editor instead of tapping on a phone screen.
- **Cross-Platform:** Write once, compile to MacroDroid, Tasker, or n8n. (Future goal)

| Feature            | MacroDroid GUI | Tasker | IFTTT | Literal |
| ------------------ | -------------- | ------ | ----- | ------- |
| Readable configs   | ❌              | ❌      | ✅     | ✅       |
| Shareable as text  | ❌              | ❌      | ❌     | ✅       |
| Version control    | ❌              | ❌      | ❌     | ✅       |
| Full device access | ✅              | ✅      | ❌     | ✅*      |
| No learning curve  | ✅              | ❌      | ✅     | ✅       |

*Inherits capabilities from compilation target

## Contributing

This project is in its early stages. I are actively seeking feedback on the language design and compiler architecture. Please open an issue to share your thoughts, suggestions, and ideas!

Here's how you can help:

**Language Design:**
- Review [literal.md](literal.md) and propose syntax improvements
- Share automation use cases that current syntax can't express
- Suggest triggers/actions/conditions for your target platform

**Compiler Development:**
- Help reverse engineer platform specific JSON export formats
- Help create the compilor architecture.
- Contribute parser or emitter code (coming soon)

**Documentation:**
- Expand [MAP.md](MAP.md) with more MacroDroid mappings
- Write example `.literal` recipes for common automation scenarios
- Translate documentation to other languages

Open an issue or PR to get started!

