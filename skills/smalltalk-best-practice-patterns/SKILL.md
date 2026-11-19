---
name: smalltalk-best-practice-patterns
description: Write or refactor code in the style of Kent Beck's Smalltalk Best Practice Patterns (behavior, state, collections, classes, formatting), in any language. Use when writing new functions or types, refactoring for readability, or reviewing code for method size, naming, and level of abstraction.
---

# Smalltalk Best Practice Patterns

Apply the patterns from Kent Beck's *Smalltalk Best Practice Patterns* in whatever language the code is written in. The goal is code that reads like a short story: each function says one thing, names say what, and the reader never has to decode how.

Where a pattern names a Smalltalk construct (message, block, class, `ifTrue:`), read it as the nearest construct in the language at hand: a function or method, a closure or lambda, a type, a conditional.

Refactor one pattern at a time. Run the project's tests before and after each step, and stop if they go red.

## Procedure

1. Read the function. Say in one sentence what it does. If the sentence needs "and", it is more than one function.
2. Walk the lines. Mark any line that sits at a different level of abstraction from its neighbors.
3. Apply the patterns below that match what you marked. Pick the one with the largest effect on readability first.
4. Rename last. Once a function does one thing, its name is easy to choose.
5. Run the project's tests and linters. Respect any complexity limits the project sets; they are the pattern's pressure, not a goal in themselves.

## Behavior

- **Composed Method**: Divide a function into pieces that each do one identifiable task, with all operations in a piece at the same level of abstraction. Typical result: functions of 3 to 10 lines. The top-level function reads as a list of steps; the detail lives in the helpers.
- **Intention Revealing Name**: Name a function for what it means to the caller, not how it works. `overdueIssues`, not `filterByDueDateLessThanNow`.
- **Message / Method Comment**: Do not narrate code with comments. If a comment explains what a block does, extract the block and give it that name. Keep comments for why: a constraint, a trade-off, an external system's quirk.
- **Query Method**: A boolean-returning function reads as a question: `isWholeScope`, `isComputable`, `hasDrawn`. Prefix with `is`, `has`, `can` when the bare name is ambiguous. Follow the language's naming convention for predicates.
- **Testing Method**: A predicate is a Query Method with no side effects. Never mutate inside one.
- **Guard Clause**: Return early for the exceptional case instead of nesting the normal one. The happy path then runs down the left margin.
- **Conditional Expression Name**: When a condition needs a comment, extract it into a Query Method.
- **Execute Around Method**: Put paired set-up and tear-down (open/close, start/log the outcome, lock/unlock) in one wrapper that takes the middle as a function argument. Where the language has a scoped-cleanup construct (`defer`, `with`, `using`, `try`/`finally`, RAII), it handles the simple case; use a wrapper when the pairing has more than one step or when call sites repeat it.
- **Reversing Method**: When a call chain reads backwards, add a method on the other type so the code reads in the order the idea runs.
- **Constructor Method**: Build a value with a named function (`newScope`, `wholeScope`) when raw construction is long or repeated. The name states the situation, not the fields.
- **Constructor Parameter Method**: Set all fields in one construction call, so a value is never half-built.
- **Method Object**: When a function takes many parameters or shares many temporaries with its helpers, turn it into a small type whose fields hold those temporaries and whose methods are the steps. The entry point stays tiny.
- **Symmetry**: Write the same idea the same way everywhere. One divergence then signals a real difference. Mirror pairs: `open`/`close`, `add`/`remove`.
- **Collection Accessor Method**: Expose a collection through intent-named accessors (`add`, `each`, `includes`) instead of the raw list or map, when callers should not depend on the representation.

## State

- **Role Suggesting Name**: Name a variable for the role it plays, not its type: `fromSprint`, not `sprint1`; `assignee`, not `str`.
- **Intention Revealing Selector**: Name the function for the intent of the call, not the mechanism of the receiver.
- **Direct / Indirect Variable Access**: Prefer one consistent way to reach state within a type. If a field has a computed or lazy form, route all access through its accessor.
- **Common State / Variable State**: Put state that every instance has in a field. Put optional state in a small side structure so it does not bloat every value.
- **Explaining Temporary Variable**: Name an intermediate result when an expression is hard to read. If the name is also useful elsewhere, extract a function instead.
- **Temporary Variable**: Use a variable only for a value that is reused or that needs a name. Do not carry one across a long stretch of code.

## Collections

- **Do / Each**: Iterate with the simplest loop that shows intent. Replace a loop with an intent-named function when the loop body is more than a few lines.
- **Collect, Select, Reject, Detect, Inject**: Map, filter, filter-out, find-first, and reduce each have one job; do not mix them in a single pass unless profiling demands it. Use the language's own collection operations where it has them; where it has none, write a small named function per job.
- **Enumeration**: Hide the traversal behind a function that takes a callback, so callers write what to do with each element, not how to reach it.
- **Copy rather than mutate**: Return a new collection from a transforming function. Mutate only inside the type that owns it.

## Classes and types

- **Simple Superclass Name / Qualified Subclass Name**: Name a type by what it is, one noun. Where the language has interfaces or protocols, prefer small ones named for one behavior, defined where they are consumed.
- **Decomposing Message**: Break a large operation into messages sent to the same object, each of which a subtype could override. Where the language has no inheritance, express the variation point as an interface or a function value.
- **Intention Revealing Message**: Keep the interface about what the caller wants.
- **Delegation**: Hand a task to a collaborator rather than inheriting from it. Hold a field, or embed where the language supports it.
- **Pluggable Behavior**: Where behavior varies, pass it in (a function, an interface value) rather than branching on a flag. A flag argument is a hint to split the function in two.

## Formatting

- **Spacing and indentation**: Follow the project's formatter and linter settings; do not hand-format.
- **Short lines of code**: A line that needs scrolling hides a missing name. Extract.
- **Blank lines**: One blank line separates ideas inside a function; if you need several, the function holds several ideas.

## Smells that point to a pattern

| Smell | Pattern |
| --- | --- |
| Function over about 10 lines, or with sections separated by blank lines | Composed Method |
| Deep nesting, an `else` after a `return` | Guard Clause |
| A comment explaining a block | Composed Method plus Intention Revealing Name |
| Long parameter list, or helpers needing the same temporaries | Method Object |
| The same open/close or start/log pair at several call sites | Execute Around Method |
| `if` condition longer than one clause | Conditional Expression Name |
| Boolean flag parameter | Pluggable Behavior, or split the function |
| Variables named `data`, `tmp`, `list`, `obj` | Role Suggesting Name |
| Two code paths for the same idea written differently | Symmetry |

## Limits

- This skill drives refactors and new code. It does not change behavior. If a change would alter behavior, stop and ask.
- Do not extract a function used once whose name restates its body. Extraction must raise the level of abstraction.
- Match the surrounding code's comment density and idiom. The patterns serve readability in the project, not the other way round.
- The pattern list comes from the book's well-known catalog, adapted where Smalltalk idioms (blocks, classes, `ifTrue:`) have no direct form in the language at hand.
