# Contributing to OpsAI

Thank you for your interest in contributing! Here's how to get started.

## 🐛 Reporting Bugs

1. Check if the issue already exists in [Issues](../../issues)
2. Open a new issue with:
   - Clear title describing the bug
   - Steps to reproduce
   - Expected vs actual behavior
   - Browser and OS

## 💡 Suggesting Features

Open an issue with the label `enhancement` and describe:
- What problem it solves
- How it fits the LLM + DevOps theme
- Any implementation ideas

## 🔧 Submitting a Pull Request

1. Fork the repo
2. Create a branch: `git checkout -b feat/your-feature-name`
3. Make your changes to `index.html`
4. Test in Chrome, Firefox, and Safari
5. Commit: `git commit -m "feat: add your feature"`
6. Push and open a PR against `main`

## 📐 Code Style

- Keep everything in the single `index.html` file
- Follow the existing CSS variable system (`--accent`, `--surface`, etc.)
- All new views go inside `<div class="view" id="view-yourview">`
- New nav items go in the sidebar under the correct section
- Mock data fallbacks are required for every new API endpoint
- Test offline demo mode (no backend) before submitting

## 🧠 AI Features

When adding new AI capabilities:
- Use `gemini-2.5-flash` as the primary model
- Always include a live system prompt with current cluster state
- Use function calling for any action the AI should perform
- Stream responses using SSE (`alt=sse` query param)
- Add a confidence badge (High/Medium/Low) to the response

## Commit Message Format

```
feat: add terminal tab completion
fix: streaming cursor not removed on error
docs: update API endpoint table in README
style: fix alert badge color in midnight theme
refactor: consolidate sparkline drawing functions
```
