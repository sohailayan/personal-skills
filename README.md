# Personal AI Skills Repository

Welcome to my personal repository of AI skills, custom instructions, and behavioral guidelines. This collection is dedicated to enhancing the capabilities of various AI assistants and coding companions. 

I will continuously update this repository with new tools, workflows, and prompts as I **discover**, **create**, and **use** them in my daily engineering tasks.

## The `.agents/skills` Framework (Native Format)

This repository natively uses the Advanced Agentic Coding environment structure. Skills are stored in the `.agents/skills/` directory. Each skill has its own folder containing a `SKILL.md` file.

**Anatomy of a Skill:**
```yaml
---
name: skill-name
description: A brief summary of what the skill does and when to invoke it.
---

[Detailed markdown instructions, rules, and decision trees go here...]
```

When an AI agent encounters a situation relevant to a skill's description, it will automatically read the `SKILL.md` file and execute the specialized instructions.

## Porting Skills to Other AI Assistants

While the primary format is `.agents/skills/`, you can easily port these markdown instructions to other tools to establish workspace-level or global rules.

### 1. GitHub Copilot (VS Code)
Copilot can read repository-level custom instructions directly from `.github` files.
- **Setup:** Create a `.github/copilot-instructions.md` file in the root of your project.
- **Usage:** Copy the markdown content of a skill into this file. Copilot will automatically index it and use it as context for code generation and chat within that repository.

### 2. Claude (Desktop & Code)
Claude can automatically pick up project instructions using local `.claude` configuration files.
- **Setup:** Create a `.claude.json` or a `.claude/` directory with specific project context files in your workspace root.
- **Usage:** Paste the skill instructions here so Claude Code or Claude Desktop automatically respects them. Alternatively, you can use the Claude web interface to create a "Project" and paste the skill into the "Custom Instructions" box.

### 3. Cursor
Cursor utilizes `.cursorrules` files to provide project-specific instructions to its AI engine.
- **Setup:** Create a file named `.cursorrules` in the root directory of your workspace. 
- **Usage:** Copy the relevant skill content into this file. Cursor will automatically apply these rules when generating code or answering queries.

### 4. ChatGPT
ChatGPT utilizes "Custom GPTs" to encapsulate specific skills.
- **Setup:** Navigate to "Explore GPTs" -> "Create" in the sidebar.
- **Usage:** Paste the skill instructions into the "Instructions" section of the "Configure" tab. You can also upload the `SKILL.md` files directly into the "Knowledge" section for retrieval.

### 5. Google Gemini
Gemini provides customized experiences through "Gems" (available to Advanced users).
- **Setup:** Go to the Gemini web interface, navigate to the "Gem Manager", and select "New Gem".
- **Usage:** Provide an appropriate name and paste the skill criteria into the "Instructions" field to create a dedicated assistant.

## Contributing

Feel free to clone this repository and add your own skills as well! If submitting a Pull Request, please follow the native `.agents/skills/<name>/SKILL.md` folder structure.

---
*Maintained by Ayan Sohail abbasi.*
