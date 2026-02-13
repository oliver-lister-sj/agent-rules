---
name: add-resource
description: Add a new resource key to the project's resource system. Use when adding new user-facing text or modifying existing resource definitions.
disable-model-invocation: false
---

# Add Resource

## When to Use

- Use this skill when you need to add a new string for the UI.
- Use this skill when you need to update an existing resource key.
- Use this skill when you are creating a new feature and need to define its resources.

## Instructions

### 1. Identify the Feature

Determine which feature the new resource belongs to. Resources are typically grouped by feature in `src/resources/`.

- If the feature already exists (e.g., `Filters`), use the existing file (e.g., `src/resources/FiltersResources.ts`).
- If it's a new feature, you will need to create a new resource file named `{FeatureName}Resources.ts`.

### 2. Define the Resource Key

Add the new key to the resource definition file.

**For existing files:**
1.  Open the resource file (e.g., `src/resources/FiltersResources.ts`).
2.  Add a new entry to the `enum` or object definition.
    -   Key: camelCase (e.g., `myNewLabel`)
    -   Value: dot.notation string (e.g., `myFeature.newLabel`)

**For new files:**
1.  Create a new file in `src/resources/` named `{FeatureName}Resources.ts` (e.g., `src/resources/MyFeatureResources.ts`).
2.  Define a context string: `export const myFeatureContext = 'myFeature';`
3.  Define an enum: `export enum MyFeatureResources { ... }`

### 3. Use the Resource in Code

Update the component to use the new resource.

1.  Import `useResources` from `@swipejobs/react-hooks`.
2.  Import the resource enum and context from the resource file.
3.  Call `useResources(MyFeatureResources, { context: myFeatureContext })`.
4.  Access the string using the enum member (e.g., `resources.myNewLabel`).

## Usage Examples

### Adding a new label to `Filters`

```typescript
// src/resources/FiltersResources.ts

export enum FiltersResources {
  // ... existing keys
  'myNewLabel' = 'filters.myNewLabel', // Add this line
}
```

### Creating a new resource file

```typescript
// src/resources/NewFeatureResources.ts

export const newFeatureContext = 'newFeature';

export enum NewFeatureResources {
  title = 'newFeature.title',
  description = 'newFeature.description',
}
```

### Using in a Component

```tsx
import { useResources } from '@swipejobs/react-hooks';
import { NewFeatureResources, newFeatureContext } from '@resources/NewFeatureResources';

const MyComponent = () => {
  const resources = useResources(NewFeatureResources, {
    context: newFeatureContext,
  });

  return <div>{resources.title}</div>;
};
```
