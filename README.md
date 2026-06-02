# Project Clean Room Skill

`project-clean-room` is a generic AI Agent Skill for long-running project handoff and context cleanup.

It helps teams stop relying on long chat history. Instead, the agent first reads a project state file, summarizes the current project state, and then continues the user's task from that source of truth.

## What Problem It Solves

Long-running product, engineering, and business projects usually suffer from:

- slow or compacted conversations;
- outdated context mixed with current decisions;
- new AI chats that do not understand the project;
- teammates repeating the same project background in WorkBuddy, Cursor, ChatGPT, or Codex;
- inconsistent document output preferences.

This skill introduces a simple pattern:

1. Maintain one project state file.
2. Use short trigger phrases such as `净化项目` or `接管项目`.
3. Make the agent read the project state before continuing.
4. Update the project state after major deliverables.

## Repository Structure

```text
project-clean-room/
├── SKILL.md
└── references/
    └── project-state-template.md
```

This follows the common Skill pattern used by modern agent tools: each skill is a directory containing a `SKILL.md` file with YAML frontmatter and Markdown instructions.

## Who Should Use It

| User | Recommended Use |
|---|---|
| Product managers | Use it to keep PRD, reports, process docs, and project knowledge consistent across long AI conversations. |
| Project owners | Use it as a lightweight project handoff protocol. |
| Cursor users | Use the project state template with Cursor Chat, or convert the skill into a Cursor Project Rule. |
| Codex users | Install the skill into a local skills directory. |
| WorkBuddy / ChatGPT users | Use the prompts and project state template, even if the tool does not support local skills. |

## Core Trigger Phrases

| Trigger | When to Use |
|---|---|
| `净化项目` | Current conversation is long, slow, or compacted. |
| `接管项目` | New chat, new teammate, or new AI tool needs to take over. |
| `净化{项目名}项目` | Same as cleanup, but for a named project. |
| `接管{项目名}项目` | Same as takeover, but for a named project. |

## Installation

### Codex

Copy the skill folder into your Codex skills directory:

```bash
cp -R project-clean-room ~/.codex/skills/
```

Restart Codex or open a new conversation if your environment only loads skills at session start.

### Cursor

Cursor users can use this in two ways.

Simple mode:

1. Copy `references/project-state-template.md` into your project.
2. Rename it to `{project-name}-current-state.md` or `当前协作状态.md`.
3. In Cursor Chat, mention the file and paste:

```text
接管项目。请先读取 @当前协作状态.md，再总结当前项目状态，然后继续处理：{your task}
```

Project Rule mode:

1. Create a Cursor Project Rule.
2. Use `project-clean-room/SKILL.md` as the rule reference.
3. Tell Cursor that when the user says `净化项目` or `接管项目`, it must first read the project state file.

### WorkBuddy / ChatGPT / Other AI Tools

If the tool does not support local skills, use the prompt pattern directly:

```text
接管项目。请先读取这份《项目当前协作状态》：{state document link or pasted content}。
读取后先用 5 条以内总结当前项目状态，再继续帮我处理：{current task}。
```

If the tool cannot open the link, paste the state file content into the chat.

## Create a Project State File

Start from:

```text
project-clean-room/references/project-state-template.md
```

The state file should include:

- project positioning;
- current scope;
- key rules;
- output preferences;
- existing deliverables and links;
- directory or document structure;
- next recommended work;
- open questions.

Keep it short and current. Do not paste long historical chat logs into the state file.

## Recommended Workflow

| Step | Action |
|---|---|
| 1 | Create or update the project state file. |
| 2 | Start a new AI chat or continue a long chat. |
| 3 | Say `接管项目` or `净化项目`. |
| 4 | Provide the state file path, file mention, link, or pasted content. |
| 5 | Ask the AI to summarize the current state. |
| 6 | Continue your actual task. |
| 7 | After major outputs, update the state file. |

## Example Prompts

### Take Over a Project

```text
接管项目。请先读取 @当前协作状态.md，再用 5 条以内总结项目当前状态，然后继续帮我处理：输出本周项目汇报材料。
```

### Clean Up a Long Conversation

```text
净化项目。请重新读取 @当前协作状态.md，以状态文件和我最新输入为准，旧聊天只作为参考。继续帮我处理：完善 PRD 的异常场景。
```

### Create a State File

```text
请基于当前项目资料创建《当前协作状态.md》，内容包括项目定位、当前范围、关键规则、输出偏好、已产出资料、资料目录、下一步建议和未决问题。不要复制长篇聊天记录。
```

## What Not To Do

- Do not copy one project's state file into another project without replacing the project facts.
- Do not put private project context into a public GitHub skill.
- Do not treat the skill as a replacement for project documentation.
- Do not rely on chat history when a project state file exists.

## Design References

This repository follows common Skill packaging practices:

- a named skill directory;
- a required `SKILL.md`;
- YAML frontmatter with `name` and `description`;
- optional reference files loaded only when needed.

Related references:

- GitHub Copilot Custom Skills documentation: https://docs.github.com/en/copilot/how-tos/copilot-sdk/features/skills
- GitHub Copilot Agent Skills documentation: https://docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/create-skills
- Anthropic public skills repository: https://github.com/anthropics/skills
- Awesome Claude Skills: https://github.com/ComposioHQ/awesome-claude-skills

## License

MIT

