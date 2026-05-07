
# Which agent and model to choose

They are quite close in experience and quality for basic tasks. Yet to try out and really understand the power of the frontier models and the value they can provide to you, it is worth trying the best even if it costs a bit. Then over time you may try other options and see what works best for you and provides best value for money.

Easiest options as of right now (subjectively ranked from my experience by how model power and cheapness and amount of usage you can get from it):

- Google `Gemini CLI` command line application with Google Gemini Flash and Flash Lite models. Completely free on any personal Google Account. Usage is limited, but might be enough for one 2h session of experimentation. No credit card required (as far as I know).

- OpenCode command line application with some [free models](https://opencode.ai/docs/zen/#pricing) without any authentication. Usage is limited, not sure by how much, especially when used without authentication and while being used form a public IP address that you might be sharing with several other users of these free quotas. No credit card required.

- OpenCode command line application with OpenRouter's [free models](https://openrouter.ai/models?order=pricing-low-to-high&q=free), needs [API key](https://openrouter.ai/workspaces/default/keys) which you get for free after registration. Usage of these free models is quite limited. No credit card required.

- OpenCode command line application with NVIDIA NIM [free models](https://build.nvidia.com/models?filters=nimType%3Anim_type_preview), also requires an API key after registration. Account needs to be activated with a mobile phone number via SMS. No credit card required.


## Pricing

https://sites.diy/blog/2026-05-01-coding-plan-comparisons/

# Sandboxing (agent isolation)

- [Pipelock](https://github.com/luckyPipewrench/pipelock). *Open-source AI agent firewall with mediator-signed action receipts from outside the agent trust boundary.*

- Native isolation support, e.g. [https://geminicli.com/docs/cli/sandbox/#sandboxing-methods](https://geminicli.com/docs/cli/sandbox/#sandboxing-methods)

# Other safety features (future work)

- Blog post for an idea if 'Statistical Guardrails for Non-Deterministic Agents' [https://machinelearningmastery.com/implementing-statistical-guardrails-for-non-deterministic-agents/](https://machinelearningmastery.com/implementing-statistical-guardrails-for-non-deterministic-agents/)

# Tools

This list is not exhaustive and should not be considered as endorsement of any particular approach of software. This is just FYI to see what's available.

## Tools to reduce token usage

- [context-mode](https://context-mode.com/). Reduce token usage by (supposedly) **a lot** via reducing the output of tool calls.

- [caveman](https://github.com/juliusbrussee/caveman). *A Claude Code skill/plugin and Codex plugin that makes agent talk like caveman — cutting ~75% of output tokens while keeping full technical accuracy.*, though [there is evidence](https://www.maxtaylor.me/articles/i-benchmarked-caveman-against-two-words) that simply adding 'be brief' to your prompt is just as effective!

## Tools to manage context and guide agents and models

- [Google Conductor](https://github.com/gemini-cli-extensions/conductor). *a Gemini CLI extension that enables Context-Driven Development. It turns the Gemini CLI into a proactive project manager that follows a strict protocol to specify, plan, and implement software features and bug fixes.* [Conductor-for-all](https://github.com/hlhr202/Conductor-for-all) is a community built tool that is reworked to function with other agentic harnesses (OpenCode, Claude, Codex, etc.)

- [Acai.sh](https://acai.sh/). *A toolkit for spec-driven software development. Stop writing prompts, start writing specs. Ship better quality software and minimize slop.*
