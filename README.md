# Smalltalk Best Practice Patterns

A [Claude Code](https://claude.com/claude-code) skill that applies the patterns from Kent Beck's *Smalltalk Best Practice Patterns* to code in **any language**: Go, JavaScript, Python, Java, and so on.

The goal is code that reads like a short story. Each function says one thing, names say what, and the reader never has to decode how.

## What it does

Claude follows a short procedure when it writes or refactors code:

1. Say in one sentence what the function does. "And" means two functions.
2. Mark lines that sit at a different level of abstraction.
3. Apply the matching pattern, largest readability gain first.
4. Rename last.
5. Run tests and linters before and after each step.

Patterns covered:

| Area | Patterns |
| --- | --- |
| Behavior | Composed Method, Intention Revealing Name, Query Method, Guard Clause, Conditional Expression Name, Execute Around Method, Method Object, Symmetry, and more |
| State | Role Suggesting Name, Explaining Temporary Variable, Direct / Indirect Variable Access, and more |
| Collections | Do / Each, Collect / Select / Reject / Detect / Inject, Enumeration, Copy rather than mutate |
| Types | Simple Superclass Name, Delegation, Pluggable Behavior, Decomposing Message |
| Formatting | Short lines, blank lines, defer to the project formatter |

It also includes a smell-to-pattern lookup table and explicit limits: it never changes behavior, and it never extracts a function whose name only restates its body.

## Install

### From the marketplace (recommended)

In Claude Code, add this repository as a marketplace, then install the plugin:

```text
/plugin marketplace add <your-github-user>/smalltalk-best-practice-patterns
/plugin install smalltalk-best-practice-patterns@smalltalk-best-practice-patterns
```

Any git URL works too:

```text
/plugin marketplace add https://github.com/<your-github-user>/smalltalk-best-practice-patterns.git
```

Or from a local clone:

```text
/plugin marketplace add ~/workspace/smalltalk-best-practice-patterns
```

Restart Claude Code if the skill does not appear.

### Share with a team

Add this to a project's `.claude/settings.json` so teammates are prompted to install it:

```json
{
  "extraKnownMarketplaces": {
    "smalltalk-best-practice-patterns": {
      "source": { "source": "github", "repo": "<your-github-user>/smalltalk-best-practice-patterns" }
    }
  },
  "enabledPlugins": {
    "smalltalk-best-practice-patterns@smalltalk-best-practice-patterns": true
  }
}
```

### Manual install (no plugin)

Copy the skill folder into your personal or project skills directory:

```bash
# personal: all projects
cp -r skills/smalltalk-best-practice-patterns ~/.claude/skills/

# project: this repo only
cp -r skills/smalltalk-best-practice-patterns <project>/.claude/skills/
```

## Use

Claude loads the skill on its own when you write new functions or types, refactor for readability, or review code for method size, naming, and level of abstraction. To call it directly:

```text
/smalltalk-best-practice-patterns
```

When installed as a plugin, the namespaced form `/smalltalk-best-practice-patterns:smalltalk-best-practice-patterns` also works.

Example prompts:

- "Refactor `buildReport` with the Smalltalk patterns."
- "Review this diff for function size and naming."

## Repository layout

```text
.claude-plugin/
  marketplace.json   marketplace catalog
  plugin.json        plugin manifest
skills/
  smalltalk-best-practice-patterns/
    SKILL.md         the skill
```

## Update

```text
/plugin marketplace update smalltalk-best-practice-patterns
```

Bump `version` in `.claude-plugin/plugin.json` and `.claude-plugin/marketplace.json` when you publish a change.

## Credits

Patterns come from Kent Beck, *Smalltalk Best Practice Patterns* (Prentice Hall, 1996). This skill is a language-neutral summary, not a substitute for the book.
