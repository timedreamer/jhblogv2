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

AI coding usually goes wrong the same way: you let it start writing code **before it understands what it's working with**. It makes assumptions, fills gaps with common patterns, and hands you something that compiles but doesn't fit your actual architecture.

Boris Tane's fix is **front-loading the human review**. Force the AI to fully understand the codebase first, generate a concrete plan second, annotate and correct that plan third — and only then let it write code.

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

Make the AI read the relevant parts of the codebase and document what it finds. The key constraint: require it to **write the findings to a file**, not just summarize them in the chat.

**Prompt:**

```text
Read the [insert target directory/files] in depth. Understand the architecture,
data structures, domain-specific logic, and dependencies. Write a comprehensive
report of findings in `research.md`. Do not output a verbal summary in the chat.
```

The file is persistent, reviewable, and referenceable in later steps. **A chat summary evaporates; a file stays.**

---

## Step 2: Generate an Implementation Plan

Once research is done, **ask the AI to draft a concrete plan based strictly on what it found**, not on general best practices or guesswork.

**Prompt:**

```text
Draft a detailed implementation plan for [insert target objective] and save it
to `plan.md`. Base the plan strictly on the research phase and the actual
codebase. Include code snippets, modified file paths, algorithmic approaches,
and domain-specific trade-offs.
```

The key word is *concrete*. `plan.md` should name specific files, include actual code snippets, and acknowledge trade-offs relevant to your codebase. Vague plans ("update the auth module") leave too much room for the AI to improvise during execution.

---

## Step 3: Annotate the Plan

Your turn. Read through `plan.md` and add inline notes: flag wrong assumptions, fill in business logic the AI didn't know about, push back on approaches that conflict with your architecture, clarify ambiguous requirements.

Then hand it back:

**Prompt:**

```
I added inline notes to `plan.md`. Address all notes and update the document
accordingly. Do not implement any code yet.
```

That "do not implement any code yet" line matters more than it looks. Without it, the AI will often start writing code while it's supposedly just revising the plan. **Keeping planning and execution separate** is the whole discipline here.

---

## Step 4: Generate a Task Breakdown

Once the plan is solid, ask the AI to break it into a sequential checklist, appended to `plan.md`.

**Prompt:**

```
Append a detailed, sequential todo list to `plan.md` outlining all phases and
individual tasks necessary to complete the plan. Do not implement any code yet.
```

As tasks get checked off you can see exactly where the AI is. If the session gets cut off, you have a clear place to pick back up.

---

## Step 5: Execute Without Interruption

The plan is done. Let the AI implement:

**Prompt:**

```
You are the world's greatest coder. Try your absolute best. Do not stop until
you succeed. Iterate, debug, optimize, fix every issue until perfect. Never give
up. Implement all tasks. Mark each as completed in `plan.md` as you finish them.
Do not stop until all tasks are completed. Omit unnecessary comments. Maintain
strict coding standards and continuously run relevant tests or validation scripts
to ensure no regressions.
```

The blunt tone is intentional. The AI has full context and a verified task list — nothing to hedge on. Having it mark each task done as it goes keeps the execution traceable (although in practice, I found coding agents many times forgot to mark the to-do list). 

---

## Summary

The workflow is simple. The discipline is not skipping steps, especially not letting the AI write code before you've reviewed and corrected the plan.

| Step | Action | Output |
|------|--------|--------|
| **Research** | Deep-read the target codebase | `research.md` |
| **Plan** | Draft a concrete implementation plan | `plan.md` |
| **Annotate** | Insert inline corrections and constraints | `plan.md` (revised) |
| **Break down** | Decompose into a sequential task list | `plan.md` (with todo) |
| **Implement** | Execute all tasks without stopping | Code changes |

What makes it work is the structured handoff between steps and the **mandatory human checkpoint** before execution. No single prompt does that.
