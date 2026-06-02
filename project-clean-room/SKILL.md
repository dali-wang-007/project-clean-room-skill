---
name: project-clean-room
description: Use when the user says 净化项目, 接管项目, 净化{项目名}项目, 接管{项目名}项目, or asks to continue a long-running project from a project state file, GitHub-shared skill, Cursor rule, WorkBuddy prompt, or compacted context.
---

# Project Clean Room

## Purpose

Use this skill to continue or hand over long-running project work without relying on old chat history. The project state file is the source of truth. Chat history is secondary.

This skill is generic. It must not assume any specific project, company, business domain, local path, or document URL.

## Trigger Phrases

| User phrase | Meaning | Required behavior |
|---|---|---|
| `净化项目` | Continue the current project after a long or compacted conversation. | Ask for, locate, or read the project state file; then continue from it. |
| `接管项目` | Take over a project in a new chat, new tool, or new team context. | Read the project state file; summarize the current state; then continue the user's task. |
| `净化{项目名}项目` | Same as `净化项目`, with a named project. | Use the named project's state file if available. |
| `接管{项目名}项目` | Same as `接管项目`, with a named project. | Use the named project's state file if available. |

## Required Workflow

1. Identify whether the user wants cleanup (`净化`) or takeover (`接管`).
2. Find the project state source:
   - explicit file path;
   - attached file;
   - pasted state text;
   - shared document link the tool can access;
   - a project file with a name like `当前协作状态`, `project-state`, `handoff`, or `context`.
3. If the state source is missing, ask the user to provide it or offer to create one from available project materials.
4. Read the state source before answering the project task.
5. Treat the state source and the latest user message as higher priority than old chat history.
6. If old chat history conflicts with the state source, follow the state source unless the user explicitly updates it.
7. After major outputs, remind or help the user update the project state with:
   - new deliverables and links;
   - changed rules or preferences;
   - decisions and open questions;
   - recommended next steps.

## Output Rules

When producing formal project documents:

- Do not include AI work-process explanations unless the user asks for them.
- Do not show document metadata time in the body, such as output time, creation time, generation time, or last updated time. Use timestamps only for file naming or backend file management.
- Do not include visible code blocks in business-facing formal documents unless the user asks for technical documentation.
- Prefer product-ready structure: goals, scope, users, process, rules, fields, exceptions, metrics, validation, and open questions.
- If the user's statement may be wrong, make a product judgment first instead of simply agreeing.

## Project State Template

If the user needs a state file, use `references/project-state-template.md` as the starting structure. Fill it with the user's project facts only.

Do not copy another project's context into a new project. Reuse the structure, not the facts.

