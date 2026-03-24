---
name: "Publish Craft Config"
description: "Safely sanitize, validate, commit, and push Craft Agent configuration files to a Git/GitHub repository."
alwaysAllow:
  - "Bash"
  - "Read"
  - "Write"
---

# Publish Craft Config

Use this skill when the user wants to publish a Craft Agent configuration file to a Git repository or GitHub repository.

## Primary goal

Take a local Craft Agent config file, optionally sanitize it for sharing, write it into the target repository, verify the result, commit it, and push it only when the prerequisites are satisfied.

## Default execution rules

You must follow these rules strictly:

1. **Prefer the shortest correct path**
   - Do not over-investigate when the task is operationally straightforward.
   - For a simple publish task, go directly through: repo check → source file check → sanitize if needed → write → validate → commit → push.

2. **Check prerequisites first**
   - Confirm the target repository, branch, and destination path.
   - Check whether git is available.
   - Check whether authentication is available before attempting push.
   - Confirm whether the file should be uploaded raw or sanitized.

3. **Sanitize by default for public sharing**
   - Preserve reusable behavioral preferences and workflow rules.
   - Remove or generalize clearly personal, environment-specific, or sensitive details.
   - If sanitization scope is ambiguous, summarize the intended sanitization before writing.

4. **Do not misstate status**
   - Only say a file was uploaded if push succeeded.
   - Only say a file was verified if validation or explicit inspection succeeded.
   - Clearly distinguish between: preparing, attempted, completed, blocked.

5. **Stop after repeated failure**
   - If the same category of failure happens twice, stop and switch strategy or ask for user help.
   - Never mechanically retry push/auth/config operations without a concrete change.

6. **Verify before reporting success**
   - Verify file content after writing.
   - Verify git diff before commit when practical.
   - Verify commit hash after commit.
   - Verify remote branch state after push.

## Recommended workflow

### Step 1: Confirm inputs
Collect or confirm:
- local source file path
- target repository URL or local repo path
- target branch
- destination path in repo
- whether sanitization is required

### Step 2: Check repository state
- clone the repository if needed
- detect whether it is empty
- confirm the current branch or create the requested branch if appropriate
- inspect working tree status before making changes

### Step 3: Prepare output file
- read the source config
- create a sanitized version if requested
- write the result to the destination path in the repository
- validate syntax/format if applicable

### Step 4: Commit carefully
- inspect status/diff
- use a clear commit message
- when creating git commits, include:

```text
Co-Authored-By: Craft Agent <agents-noreply@craft.do>
```

### Step 5: Push and verify
- push only when authentication is available
- if push fails due to auth, stop and explain the minimum user action needed
- after push, verify the remote branch points to the expected commit

## Output expectations

When the task completes, report:
- target repository
- destination path
- whether sanitization was applied
- commit hash
- push status
- verification result

## Good use cases

- publish `.craft-agent/preferences.json`
- publish a sanitized example of `config.json`
- sync reusable Craft Agent setup files to GitHub
- create public example repos for Craft Agent configuration

## Avoid

- blindly pushing unsanitized personal files to public repositories
- repeated retry loops for the same git/auth error
- saying “uploaded” when only the local commit exists
- expanding the task into unrelated reverse engineering or broad repo investigation
