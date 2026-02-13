---
name: validate-system
description: Audit and validate the Cursor rules and skills system for consistency, naming conventions, and structural integrity. Use when checking rules, maintaining the system, or troubleshooting issues.
---

# Validate System

Audit the `.cursor/rules/` and `.cursor/skills/` directories to ensure consistency with project conventions.

## When to Use

- User asks to validate or check the rules system
- Before committing changes to rules or skills
- Troubleshooting rules that aren't being applied
- Maintaining system health
- User mentions auditing, checking, or validating rules/skills

## Instructions

### Step 1: Check Rules Directory

1. List all files in `.cursor/rules/`:
   ```bash
   ls -la .cursor/rules/
   ```

2. For each `.mdc` file, verify:
   - **Naming convention**: `[prefix]-[category]-[name].mdc`
   - **Prefix range**: Matches the category (see table below)
   - **Extension**: Must be `.mdc` or `.md`

3. Read each rule file and check:
   - **Frontmatter exists**: Has YAML frontmatter with `---` delimiters
   - **Required fields**: `description` is present
   - **Field validity**:
     - `description` is non-empty and under 1024 chars
     - `alwaysApply` is boolean (true/false)
     - `globs` is a string pattern (if present)
   - **Logic**: If `alwaysApply: true`, `globs` should not be set
   - **Length**: File is under 500 lines (excluding frontmatter)

### Step 2: Check Skills Directory

1. List all directories in `.cursor/skills/`:
   ```bash
   ls -la .cursor/skills/
   ```

2. For each skill directory, verify:
   - **SKILL.md exists**: Each directory must contain `SKILL.md`
   - **Name matches directory**: Folder name should match the skill name in frontmatter

3. Read each `SKILL.md` file and check:
   - **Frontmatter exists**: Has YAML frontmatter with `---` delimiters
   - **Required fields**: `name` and `description` are present
   - **Field validity**:
     - `name` is lowercase, hyphens only, max 64 chars
     - `name` matches parent directory name
     - `description` is non-empty and under 1024 chars
     - `disable-model-invocation` is boolean (if present)
   - **Length**: SKILL.md is under 500 lines (excluding frontmatter)

### Step 3: Report Findings

Create a structured report with these sections:

```markdown
# System Validation Report

## Summary
- Total rules: X
- Total skills: Y
- Issues found: Z

## Rules Analysis

### ✅ Valid Rules
- [List of valid rule files]

### ❌ Issues Found
- [File]: [Issue description]
- [File]: [Issue description]

## Skills Analysis

### ✅ Valid Skills
- [List of valid skill directories]

### ❌ Issues Found
- [Skill]: [Issue description]
- [Skill]: [Issue description]

## Recommendations
- [Actionable recommendations to fix issues]
```

### Step 4: Offer to Fix Issues

If issues are found, ask the user if they want you to fix them automatically.

## Validation Criteria Reference

### Rule Naming Convention

| Range       | Category              |
| ----------- | --------------------- |
| **000-099** | **Core & Behavior**   |
| **100-199** | **Languages**         |
| **200-299** | **Frameworks**        |
| **300-399** | **Testing & QA**      |
| **400-499** | **Tools & Infra**     |
| **800-899** | **Project Specifics** |
| **900-999** | **Meta & System**     |

### Rule Frontmatter Requirements

```yaml
description: [Required, non-empty, max 1024 chars]
globs: [Optional, string pattern]
alwaysApply: [Optional, boolean, default false]
```

### Skill Frontmatter Requirements

```yaml
name: [Required, lowercase/hyphens, max 64 chars]
description: [Required, non-empty, max 1024 chars]
disable-model-invocation: [Optional, boolean]
license: [Optional, string]
compatibility: [Optional, string]
```

## Common Issues

### Issue: Wrong Prefix Range
```
❌ File: .cursor/rules/100-react-patterns.mdc
   Issue: React is a framework (200-299), not a language (100-199)
   Fix: Rename to 200-react-patterns.mdc
```

### Issue: Missing Description
```
❌ File: .cursor/rules/150-python.mdc
   Issue: Frontmatter missing 'description' field
   Fix: Add description field to frontmatter
```

### Issue: Skill Name Mismatch
```
❌ Skill: .cursor/skills/my-skill/
   Issue: SKILL.md has name: "my_skill" (should use hyphens)
   Fix: Update name to "my-skill" in frontmatter
```

### Issue: File Too Long
```
❌ File: .cursor/rules/200-react.mdc
   Issue: File is 750 lines (limit: 500)
   Fix: Split into multiple rules or move details to reference files
```

## Usage Example

```
User: "Can you validate my rules system?"

Agent:
1. Lists all .mdc files in .cursor/rules/
2. Reads each file and checks frontmatter
3. Lists all directories in .cursor/skills/
4. Reads each SKILL.md and checks frontmatter
5. Generates validation report
6. Reports findings to user
7. Offers to fix issues if any found
```

## Best Practices

- Run validation before committing changes
- Fix issues immediately to maintain system health
- Use this skill after creating new rules/skills
- Validate periodically as the system grows
