# Core Code Review Guidelines

## SOLID Principles

- **Single Responsibility**: Each class/function should do ONE thing only. Flag functions that mix concerns (e.g., fetching data AND rendering).
- **Open/Closed**: Logic should be extendable without modifying existing code. Flag direct modification of core logic instead of using abstraction.
- **Liskov Substitution**: Subclasses must behave like their parent. Flag overrides that break expected behaviour.
- **Interface Segregation**: Prefer small, focused interfaces. Flag components or classes that take on too many unrelated responsibilities.
- **Dependency Inversion**: Depend on abstractions, not concrete implementations. Flag tight coupling between modules.

## Uncle Bob's Clean Code

- **Names**: Variables, functions and classes must reveal intent. Flag vague names like `data`, `temp`, `handleStuff`.
- **Functions**: Keep functions small (ideally under 20 lines). Flag functions doing multiple distinct things.
- **Comments**: Code should explain itself. Comments should explain WHY, not WHAT. Flag comments that restate the code.
- **No Dead Code**: Flag unused variables, commented-out code blocks, and unreachable logic.
- **Error Handling**: Errors must be handled explicitly. Flag empty catch blocks and silent failures.
- **DRY**: Flag duplicated logic that should be extracted into a shared utility or function.

## General

- No hardcoded values — use constants or environment variables.
- No `console.log` / `print` statements left in production code.
- Security: Flag exposed secrets, unsanitised user input, or missing auth checks.
- Performance: Flag obvious inefficiencies such as loops inside loops or repeated expensive operations.
