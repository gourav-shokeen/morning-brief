# Contributing

Thanks for contributing to MorningBrief.

This repository is centered around an n8n workflow export, so the main goal is to keep the workflow importable, understandable, and easy to set up for someone cloning the repo for the first time.

## Local workflow for changes

1. Import `workflow.json` into n8n
2. Make your changes in the n8n editor
3. Test the workflow manually
4. Export the updated workflow back to `workflow.json`
5. Update `README.md` or docs if setup behavior changed

## Contribution guidelines

- Keep node names clear and stable
- Keep the visual layout readable on the n8n canvas
- Prefer simple setup over clever setup
- Avoid adding sources that require paid APIs unless clearly optional
- Do not commit real credentials, chat IDs, or private feed URLs

## Before opening a pull request

- Confirm `workflow.json` imports successfully
- Confirm the main path still works: schedule -> fetch -> merge -> assemble -> Telegram
- Confirm docs match the actual node configuration
- Confirm `.env`, private links, and credentials are not included

## Good pull requests

Useful contributions include:

- new optional news or productivity sources
- better Telegram formatting
- improved parsing robustness
- setup documentation improvements
- bug fixes in node configuration or branching

## Large changes

If you want to restructure the workflow heavily, open an issue or draft PR first so the approach can be discussed before the export changes too much.
