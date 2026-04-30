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
