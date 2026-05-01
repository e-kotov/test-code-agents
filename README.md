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

This environment comes preconfigured with several AI coding agents:

- **[OpenCode](https://opencode.ai/)**: OpenCode coding agent that has some basic [free models without any authentication](https://opencode.ai/docs/zen/#pricing) and connects easily to OpenRouter (also has [free models](https://openrouter.ai/models?order=pricing-low-to-high&q=free), needs API key - free after registration), Nvidia NIM (also has [free models](https://build.nvidia.com/models?filters=nimType%3Anim_type_preview) with API key after registration). Best choice to just launch it and try right away without registration.


- **[Google Gemini CLI](https://geminicli.com/)**: Google's official command-line interface for Gemini. Even free accounts get rather generous quota. Like many other free services, it requires [opt out](https://github.com/google-gemini/gemini-cli/blob/main/docs/reference/configuration.md#usage-statistics) to prevent using your requests and data for model training.

- **[Anthropic Claude Code](https://code.claude.com/docs/en/overview)**: Anthropic's official CLI tool for Claude.

- **[OpenAI Codex](https://developers.openai.com/codex/cli)**: OpenAI's official Codex CLI.

- **[Pi Coding Agent](https://pi.dev/)**: AI coding agent by Mario Zechner. Can connect to [OpenCode Zen](https://opencode.ai/docs/zen/), [OpenRouter](https://openrouter.ai/) and other services.

- **[Aider](https://aider.chat/)**: Lightweight coding agent written in Python. Can connect to [OpenCode Zen](https://opencode.ai/docs/zen/), [OpenRouter](https://openrouter.ai/) and other services.

- **[NCA (NCA CLI)](https://nca-cli.com/)**: Lightweight coding written in Rust. Can connect to [OpenCode Zen](https://opencode.ai/docs/zen/), [OpenRouter](https://
openrouter.ai/) and other services.

Other preinstalled software:

- **[ARF Console](https://github.com/eitsupi/arf)**: Rust-based R console that tries to bring RStudio-like console to terminal (in VScode that we use in the container).

- `R` language extensions to improve integration with VScode.


For full list of software see [Dockerbuild/Dockerfile](Dockerbuild/Dockerfile) and [.devcontainer/devcontainer.json](.devcontainer/devcontainer.json)
