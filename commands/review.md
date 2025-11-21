---
description: "Triggers a code review for specific files or the current context."
argument-hint: "[file paths or description of changes]"
---

# /c2:review

This command initiates a code review process using the `reviewer` agent.

## When to use

- **Use when:** You have completed a coding task and want to verify the quality and security of your changes.
- **Use when:** You want to audit specific files for potential issues.

## Actions

1.  **Step 1: Identify Scope**
    - Determine which files need to be reviewed based on the user's input or recent activity.

2.  **Step 2: Launch Reviewer**
    - Invoke the `reviewer` agent with the identified scope.
    - Pass any specific focus areas mentioned by the user (e.g., "check for security issues").

3.  **Step 3: Report Findings**
    - The `reviewer` agent will output a prioritized list of issues and suggestions.
    - Present this report to the user and ask if they want to proceed with fixing the issues (which would typically be handled by a `worker` or `scout` -> `worker` flow).
