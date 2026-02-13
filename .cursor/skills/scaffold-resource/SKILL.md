---
name: scaffold-resource
description: Generate new Cursor rules or Agent Skills following project conventions. Use when creating rules, adding standards, setting up skills, or when the user asks to create a new rule or skill.
---

# Scaffold Resource

Generate properly structured Cursor rules (`.mdc` files) or Agent Skills (`SKILL.md` files) following project conventions.

## When to Use

- User asks to create a new rule
- User wants to add coding standards or conventions
- User asks to create a new skill
- User mentions scaffolding, generating, or setting up rules/skills

## Instructions

### Step 1: Determine Resource Type

Ask the user what type of resource they want to create using the AskQuestion tool:

- **Rule**: A `.mdc` file in `.cursor/rules/`
- **Skill**: A directory with `SKILL.md` in `.cursor/skills/`

### Step 2: Gather Information

#### For Rules

Ask using AskQuestion tool or conversationally:

1. **Category**: Which number range does this belong to?
   - 000-099: Core & Behavior
   - 100-199: Languages
   - 200-299: Frameworks
   - 300-399: Testing & QA
   - 400-499: Tools & Infra
   - 800-899: Project Specifics
   - 900-999: Meta & System

2. **Name**: What is the specific topic? (e.g., "typescript", "react-patterns")

3. **Scope**: Should this rule:
   - Always apply (alwaysApply: true)
   - Apply to specific files (provide glob patterns like `**/*.ts`)
   - Apply intelligently (agent decides when relevant)

4. **Content**: What standards or conventions should this rule enforce?

#### For Skills

Ask using AskQuestion tool or conversationally:

1. **Name**: What should the skill be called? (lowercase, hyphens only)
2. **Description**: What does it do and when should it be used?
3. **Invocation**: Should this be:
   - Automatic (agent decides when to use)
   - Manual only (disable-model-invocation: true)
4. **Scripts**: Does this skill need executable scripts?

### Step 3: Generate the File

#### For Rules

Create a file at `.cursor/rules/[prefix]-[name].mdc` using this template:

```markdown
---
description: [Action-oriented description with trigger keywords]
globs: [File patterns if applicable]
alwaysApply: [true/false]
---

# [Rule Name]

## Context

[When to apply this rule]

## Standards

- [Standard 1]
- [Standard 2]

## Examples

❌ **Bad**:
[Example of what not to do]

✅ **Good**:
[Example of correct approach]

## Common Commands

[Relevant terminal commands if applicable]
```

**Prefix Selection Guide**:
- 000-099: Core & Behavior → Use 000, 010, 020, etc.
- 100-199: Languages → Use 100, 110, 120, etc.
- 200-299: Frameworks → Use 200, 210, 220, etc.
- 300-399: Testing & QA → Use 300, 310, 320, etc.
- 400-499: Tools & Infra → Use 400, 410, 420, etc.
- 800-899: Project Specifics → Use 800, 810, 820, etc.
- 900-999: Meta & System → Use 900, 910, 920, etc.

#### For Skills

1. Create directory: `.cursor/skills/[skill-name]/`
2. Create `SKILL.md` using this template:

```markdown
---
name: [skill-name]
description: [What it does and when to use it. Include trigger keywords.]
disable-model-invocation: [true/false]
---

# [Skill Name]

## When to Use

- Use this skill when...
- This skill is helpful for...

## Instructions

[Step-by-step guidance for the agent]

1. First step
2. Second step
3. Third step

## Usage

[Examples of using this skill]

## Additional Resources

[Link to scripts or reference files if needed]
```

3. If scripts are needed, create `.cursor/skills/[skill-name]/scripts/` directory

### Step 4: Confirm Creation

After creating the file, inform the user:
- The file location
- The structure used
- Next steps (if any)

## Examples

### Example 1: Creating a TypeScript Rule

```
User: "Create a rule for TypeScript conventions"

Agent:
1. Asks: "Should this rule always apply or only to TypeScript files?"
2. User: "Only TypeScript files"
3. Asks: "What specific conventions should it enforce?"
4. Creates: .cursor/rules/100-lang-typescript.mdc
5. Confirms: "Created TypeScript rule at .cursor/rules/100-lang-typescript.mdc"
```

### Example 2: Creating a Deployment Skill

```
User: "I need a skill for deploying to production"

Agent:
1. Asks: "Should this be automatically invoked or manual only?"
2. User: "Manual only, it's sensitive"
3. Asks: "Does it need executable scripts?"
4. User: "Yes, for validation and deployment"
5. Creates:
   - .cursor/skills/deploy-production/SKILL.md
   - .cursor/skills/deploy-production/scripts/ directory
6. Confirms: "Created deployment skill. Add your scripts to the scripts/ directory."
```

## Best Practices

- Always follow the naming conventions exactly
- Ensure descriptions include trigger keywords
- Keep rules under 500 lines
- Keep SKILL.md under 500 lines
- Provide concrete examples in rules
- Make skill instructions actionable and clear
- Use the AskQuestion tool for structured input when available
