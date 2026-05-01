# test-code-agents

This repository contains a custom Binder environment based on `rocker/binder` with pre-installed VS Code extensions and global CLI tools (Node.js, uv, Claude CLI, etc.).

## Open in Binder

Click the button below to launch this environment in JupyterLab:

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/e-kotov/test-code-agents/main?urlpath=lab)

To access VS Code once the environment is loaded, look for the VS Code icon in the JupyterLab Launcher or append `?urlpath=vscode` to the Binder URL.


To monitor total RAM:

```bash
watch -n 1 'ps -U jovyan -o rss= | awk "{sum+=\$1} END {print sum/1024 \" MB\"}"'
```

```bash
watch -n 1 'smem --userfilter=jovyan -t -k'
```

## Preinstalled Coding Agents

This environment comes preconfigured with several AI coding agents and assistants:

- **[Aider](https://aider.chat/)**: AI pair programming in your terminal.
- **[Amp](https://ampcode.com/)**: Command-line AI coding assistant.
- **[ARF Console](https://github.com/eitsupi/arf)**: Rust-based R console.
- **[Claude Code](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/overview)**: Anthropic's official CLI tool for Claude.
- **[Gemini CLI](https://www.npmjs.com/package/@google/gemini-cli)**: Google's official command-line interface for Gemini.
- **[NCA (NCA CLI)](https://nca-cli.com/)**: OpenAI-compatible CLI assistant.
- **[OpenCode AI](https://www.npmjs.com/package/opencode-ai)**: CLI tool for OpenCode.
- **[Pi Coding Agent](https://www.npmjs.com/package/@mariozechner/pi-coding-agent)**: AI coding agent by Mario Zechner.
