---
title: A Four-Step Workflow for AI Coding That Actually Works
author: Ji Huang
date: '2026-02-25'
categories:
    - misc
tags:
    - AI
    - claude
    - coding
    - workflow
    - llm
description: Inspired by Boris Tane's post, a research → planning → annotation → implementation cycle for getting reliable results from AI coding assistants, with copy-paste prompts for each step.
---

> This post is inspired by [Boris Tane's blog post on how he uses Claude Code](https://boristane.com/blog/how-i-use-claude-code).

The most common way AI coding goes wrong isn't that the model is incapable — it's that you let it start writing code **before it understands what it's working with**. AI will make assumptions, fill in gaps with common patterns, and hand you something that looks plausible but doesn't fit your actual architecture.

Boris Tane's workflow fixes this by **front-loading the human review**. The core idea: force the AI to fully understand the codebase first, generate a concrete plan second, let you annotate and correct that plan third, and only then give it the green light to execute.

```text
┌────────────┐   ┌────────────┐   ┌────────────┐   ┌────────────┐   ┌────────────┐
│  Research  │──▶│    Plan    │──▶│  Annotate  │──▶│ Break down │──▶│  Implement │
│            │   │            │   │  (human)   │   │            │   │            │
│ research   │   │  plan.md   │   │  plan.md   │   │  plan.md   │   │    code    │
│   .md      │   │  (draft)   │   │ (revised)  │   │ (+ todos)  │   │  changes   │
└────────────┘   └────────────┘   └────────────┘   └────────────┘   └────────────┘
```

---

## Step 1: Force Deep Research

Before any code is written, make the AI read the relevant parts of the codebase and document what it finds. Critically, require it to **write the findings to a file** — not just summarize them in the chat.

**Prompt:**

```
Read the [insert target directory/files] in depth. Understand the architecture,
data structures, domain-specific logic, and dependencies. Write a comprehensive
report of findings in `research.md`. Do not output a verbal summary in the chat.
```

Writing to `research.md` serves a few purposes: the output is persistent, reviewable, and can be explicitly referenced in later steps. **A chat summary evaporates; a file stays.**

---

## Step 2: Generate an Implementation Plan

With the research in place, **ask the AI to draft a concrete plan based strictly on what it found** — not on general best practices or guesswork.

**Prompt:**

```
Draft a detailed implementation plan for [insert target objective] and save it
to `plan.md`. Base the plan strictly on the research phase and the actual
codebase. Include code snippets, modified file paths, algorithmic approaches,
and domain-specific trade-offs.
```

The key word is *concrete*. `plan.md` should name specific files, include actual code snippets, and acknowledge trade-offs relevant to your codebase. Vague plans ("update the auth module") leave too much room for the AI to improvise during execution.

---

## Step 3: Annotate the Plan

This is where you step in. Read through `plan.md` and insert inline notes directly into the document: flag wrong assumptions, add business logic the AI didn't know about, reject approaches that conflict with your architecture, or clarify ambiguous requirements.

Then hand it back:

**Prompt:**

```
I added inline notes to `plan.md`. Address all notes and update the document
accordingly. Do not implement any code yet.
```

The "do not implement any code yet" constraint matters. **Keeping planning and execution separate** prevents the AI from quietly starting to write code while ostensibly just updating the plan.

---

## Step 4: Generate a Task Breakdown

Once the plan is solid, ask the AI to decompose it into a sequential, trackable checklist — appended to `plan.md`.

**Prompt:**

```
Append a detailed, sequential todo list to `plan.md` outlining all phases and
individual tasks necessary to complete the plan. Do not implement any code yet.
```

This checklist does two things: it makes execution auditable as each task gets checked off, and it gives you a clean resume point if the session gets interrupted mid-way.

---

## Step 5: Execute Without Interruption

With a reviewed, annotated, and checksummed plan in hand, release the AI to implement:

**Prompt:**

```
You are the world's greatest coder. Try your absolute best. Do not stop until
you succeed. Iterate, debug, optimize, fix every issue until perfect. Never give
up. Implement all tasks. Mark each as completed in `plan.md` as you finish them.
Do not stop until all tasks are completed. Omit unnecessary comments. Maintain
strict coding standards and continuously run relevant tests or validation scripts
to ensure no regressions.
```

The assertive tone is deliberate. By this point the AI has full context and a verified task list — there's no reason to hedge. Requiring it to mark tasks complete in `plan.md` as it goes keeps the execution traceable.

---

## Summary

The workflow isn't complicated. The discipline is in not skipping steps — especially not letting the AI start writing code before the plan has been reviewed and corrected.

| Step | Action | Output |
|------|--------|--------|
| Research | Deep-read the target codebase | `research.md` |
| Plan | Draft a concrete implementation plan | `plan.md` |
| Annotate | Insert inline corrections and constraints | `plan.md` (revised) |
| Break down | Decompose into a sequential task list | `plan.md` (with todo) |
| Implement | Execute all tasks without stopping | Code changes |

The real value isn't any single prompt — it's the structured handoff between steps, and the **mandatory human checkpoint** before execution begins.
