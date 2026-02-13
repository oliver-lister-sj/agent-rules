# Agent Rules & Skills System

This project uses a structured system for managing Cursor rules and skills to provide consistent, high-quality AI assistance.

## Overview

The rules system is organized into numbered categories stored in `.cursor/rules/` and complemented by executable skills in `.cursor/skills/`.

## Directory Structure

```
.cursor/
├── rules/           # Rule files (.mdc format)
│   ├── 000-core-behavior.mdc
│   ├── 100-lang-typescript.mdc
│   ├── 200-react-patterns.mdc
│   └── 900-meta-create-rule.mdc
└── skills/          # Agent skills (directories with SKILL.md)
    ├── scaffold-resource/
    │   └── SKILL.md
    └── validate-system/
        └── SKILL.md
```

## Rule Naming Convention

Rules follow the pattern: `[prefix]-[category]-[name].mdc`

### Number Ranges

| Range       | Category              | Purpose                                                      | Examples                                    |
| ----------- | --------------------- | ------------------------------------------------------------ | ------------------------------------------- |
| **000-099** | **Core & Behavior**   | How the agent works (persona, workflow, universal standards) | `000-core-behavior.mdc`                     |
| **100-199** | **Languages**         | Programming language syntax and base conventions             | `100-lang-typescript.mdc`                   |
| **200-299** | **Frameworks**        | Libraries and frameworks built on languages                  | `200-react-patterns.mdc`                    |
| **300-399** | **Testing & QA**      | Testing standards, mocking, quality assurance                | `300-testing-jest.mdc`                      |
| **400-499** | **Tools & Infra**     | External tools, DevOps, version control                      | `400-tool-git.mdc`                          |
| **800-899** | **Project Specifics** | Business logic, domain knowledge, project workflows          | `800-domain-billing.mdc`                    |
| **900-999** | **Meta & System**     | Maintaining the rule system itself                           | `900-meta-create-rule.mdc`                  |

## Rule Structure

Each rule is a `.mdc` (Markdown with frontmatter) file containing:

```markdown
---
description: Action-oriented description with trigger keywords
globs: **/*.ts              # File patterns (optional)
alwaysApply: false          # true = always active
---

# Rule Name

## Context
When to apply this rule

## Standards
- Standard 1
- Standard 2

## Examples
Bad vs Good code examples

## Common Commands
Relevant terminal commands
```

## Skills

Skills are executable packages that teach the agent how to perform specific tasks. Each skill lives in its own directory with a `SKILL.md` file.

### Available Skills

- **scaffold-resource**: Generate new rules or skills following our conventions
- **validate-system**: Audit the rules and skills directories for consistency

### Using Skills

- Skills are automatically invoked by the agent when relevant
- Manually invoke with `/skill-name` in chat
- Skills can include executable scripts in a `scripts/` subdirectory

## Creating New Rules

1. Use the `scaffold-resource` skill: Type `/scaffold-resource` in chat
2. Or manually create a file following the naming convention
3. See `900-meta-create-rule.mdc` for detailed guidelines

## Creating New Skills

1. Use the `scaffold-resource` skill to generate the skeleton
2. Follow the Agent Skills standard (see [agentskills.io](https://agentskills.io))
3. See `901-meta-create-skill.mdc` for detailed guidelines

## Best Practices

### For Rules

- Keep under 500 lines
- One concern per rule
- Provide concrete examples
- Use Bad ❌ vs Good ✅ format
- Don't duplicate what linters already enforce

### For Skills

- Keep `SKILL.md` focused (under 500 lines)
- Use progressive disclosure for detailed docs
- Include trigger keywords in descriptions
- Make scripts self-contained with good error messages

## Maintenance

The system is self-documenting and self-maintaining:

- Meta-rules (900-999) teach the agent how to create new rules/skills
- The `validate-system` skill checks for consistency
- All rules and skills are version-controlled

## Resources

- [Cursor Rules Documentation](https://docs.cursor.com/context/rules)
- [Agent Skills Standard](https://agentskills.io)
- [Cursor Skills Documentation](https://docs.cursor.com/context/skills)

---

**Last Updated**: 2026-02-13  
**Maintained By**: Project team
