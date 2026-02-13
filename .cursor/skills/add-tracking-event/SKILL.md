---
name: add-tracking-event
description: Guide for adding new tracking events to the client tracking system. Use when the user needs to track a new action that doesn't exist in the current enums.
disable-model-invocation: false
---

# Add Tracking Event

## When to Use

- Use this skill when you need to track a user action (click, view, etc.) but the appropriate `ClientTrackingCategory` or `ClientTrackingName` does not exist.
- Use this skill when the user asks to "add a new tracking event".

## Instructions

### 1. Verify Existence

First, check if a suitable category or name already exists in the `@swipejobs/fe-client-tracking` package.

- Check `ClientTrackingCategory` enum.
- Check `ClientTrackingName` enum.

### 2. Define the New Event

If it doesn't exist, you need to define it.

- **Category**: Should represent the feature or domain (e.g., `MatchingFactors`, `JobSearch`, `UserProfile`).
- **Name**: Should represent the specific action in Title Case (e.g., `SetSkills`, `ViewScorePanel`, `SubmitApplication`).

### 3. Update `fe-client-tracking` (External Repo)

**Note:** You cannot directly edit the `fe-client-tracking` repo from this workspace. You must instruct the user to do this.

**Instructions for the User:**

1.  Clone or open the `fe-client-tracking` repository.
2.  Locate the definitions for `ClientTrackingCategory` and `ClientTrackingName` (usually in `src/enums` or similar).
3.  Add your new keys.
4.  Commit, push, and create a Pull Request.
5.  Once merged and a new version is published, update the dependency in your current project.

### 4. Update Dependency

Once the new version of `@swipejobs/fe-client-tracking` is available:

```bash
npm install @swipejobs/fe-client-tracking@latest
```

### 5. Implement Tracking

Now you can use the new event in your code:

```typescript
import {
  ClientTrackingCategory,
  ClientTrackingName,
  trackClick,
} from '@swipejobs/fe-client-tracking';

// ...
trackClick(ClientTrackingCategory.MyNewCategory, ClientTrackingName.MyNewAction);
```

## Usage Example

**User:** "I need to track when a user clicks the 'Export PDF' button, but there's no event for it."

**Agent:**
"I see. We need to add a new tracking event. Since I can't modify the tracking library directly, here is the plan:

1.  **Plan the Event**:
    *   Category: `MatchingFactors` (existing)
    *   Name: `ExportPdf` (new)

2.  **Action Required**: Please update the `fe-client-tracking` repository to include `ExportPdf` in the `ClientTrackingName` enum.

3.  **After Update**: Once published, run `npm install @swipejobs/fe-client-tracking@latest` here.

4.  **Implementation**:
    ```typescript
    trackClick(ClientTrackingCategory.MatchingFactors, ClientTrackingName.ExportPdf);
    ```
"
