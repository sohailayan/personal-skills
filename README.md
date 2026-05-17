# Personal AI Skills Repository

Welcome to my personal repository of AI skills, custom instructions, and behavioral guidelines. This collection is dedicated to enhancing the capabilities of various AI assistants and coding companions. 

I will continuously update this repository with new tools, workflows, and prompts as I **discover**, **create**, and **use** them in my daily engineering tasks.

## Overview

This repository serves as a centralized knowledge base for configuring and extending the behavior of modern AI tools. The skills contained herein range from specific context instructions to more complex operational guidelines tailored for professional software engineering, architecture, and debugging.

## Setting Up Skills Across AI Assistants

The implementation of a "skill" or custom instruction varies depending on the platform or IDE. Below is a brief setup guide for integrating these skills into popular AI tools.

### 1. Cursor
Cursor utilizes `.cursorrules` files to provide project-specific instructions to its AI engine.
- **Project Setup:** Create a file named `.cursorrules` in the root directory of your workspace. Copy the relevant skill content into this file. Cursor will automatically apply these rules when generating code or answering queries.
- **Global Setup:** You can configure global rules across all projects by navigating to `Settings > General > Rules for AI` and pasting your instructions there.

### 2. GitHub Copilot (VS Code)
Copilot in VS Code can be guided using workspace-specific custom instructions.
- **Setup:** You can define custom instructions for Copilot Chat to guide its code generation.
- **Usage:** Open your VS Code Settings and search for `github.copilot.chat.codeGeneration.instructions`. Alternatively, you can define these instructions within your workspace's `.vscode/settings.json` file to keep them scoped to a specific project.

### 3. Claude Code
Claude can be extended using custom project instructions or via the Model Context Protocol (MCP) for deeper integrations.
- **Web/Desktop Setup:** Create a new "Project" in the Claude interface. Paste the skill definitions into the project's "Custom Instructions" box, or upload them as project knowledge files.
- **CLI/API Setup:** When using Claude Code or the API, you can provide the skill markdown as part of the initial `system` prompt or feed it directly into the context window.

### 4. ChatGPT
ChatGPT utilizes "Custom GPTs" to encapsulate specific skills, instructions, and knowledge bases.
- **Setup:** Navigate to "Explore GPTs" -> "Create" in the sidebar.
- **Usage:** Paste the skill instructions into the "Instructions" section of the "Configure" tab. You can also upload related skill files (like JSON schemas or documentation) into the "Knowledge" section.

### 5. Google Gemini
Gemini provides customized, focused experiences through "Gems" (available to Gemini Advanced users).
- **Setup:** Go to the Gemini web interface, navigate to the "Gem Manager", and select "New Gem".
- **Usage:** Provide an appropriate name and paste the skill criteria into the "Instructions" field. This creates a dedicated assistant tuned specifically to those behavioral guidelines.

## Future Updates

This is an active, living repository. Expect frequent updates as I refine these techniques and build out new functional skills for different development environments.

## Contributing

Feel free to clone this repository and add your own skills as well!

---
*Maintained by Ayan Sohail abbasi.*
