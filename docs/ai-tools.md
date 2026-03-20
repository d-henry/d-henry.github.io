## AI Tools for QA Engineers

Notes on how I use AI tools in my day-to-day testing and automation work. This isn't about AI testing AI — it's about using LLMs as a force multiplier in a QA automation workflow.

At Optilogic I've led efforts to adopt and standardize AI tooling across the QA team, including Claude Code, GitHub Copilot, and local LLMs via Ollama.

---

## Tools I Use

| Tool | Type | Best For |
|------|------|----------|
| Claude Code | Agentic CLI (Anthropic) | Complex multi-file tasks, refactoring, code review |
| GitHub Copilot | IDE autocomplete + chat | In-editor suggestions, quick generation |
| Ollama + Local LLM | Self-hosted | Sensitive codebases, offline use, experimentation |

---

## Claude Code

[Claude Code](https://claude.ai/code) is an agentic CLI tool from Anthropic. Unlike Copilot, it can read the full codebase, run commands, edit multiple files, and reason across a whole task — not just complete the next line.

**What I use it for in QA:**

- **Scaffolding test suites** — Give it a feature spec or API schema, ask it to generate a full Pytest + Playwright test file with fixtures and page objects.
- **Refactoring legacy tests** — Point it at an old Selenium Java suite and have it convert patterns to modern equivalents.
- **Generating test data** — Ask it to write factory functions or seed scripts based on a data model.
- **Writing documentation** — First drafts of onboarding docs, README files, and inline comments.
- **Code review** — Ask it to review a PR diff for test coverage gaps, flaky test patterns, or missing assertions.

**Tips:**
- Give it context upfront — tell it your framework, language, and conventions before asking it to generate code.
- Review everything it writes. It's fast but not infallible, especially on domain-specific logic.
- Use it for the boring parts: boilerplate, repetitive patterns, and first drafts. You still own the quality.

---

## GitHub Copilot

Copilot lives in the IDE and shines at in-context autocomplete and quick generation. It's faster than Claude Code for single-file work where you already know what you want.

**What I use it for:**

- Completing repetitive test methods (it learns your patterns within the file)
- Generating docstrings and comments
- Quick one-off scripts
- Inline chat for "what does this do" questions

**Tips:**
- It works best when the file already has examples to learn from. Write one test well and it will pattern-match the rest.
- Use the chat panel for explanations; use autocomplete for generation.
- Be skeptical of its assertions and imports — verify they're correct for your specific version of a library.

---

## Ollama + Local LLMs

[Ollama](https://ollama.com/) lets you run LLMs locally. Useful when:
- You're working with sensitive internal code you can't send to a cloud API
- You want to experiment with models (Mistral, Llama, Qwen, etc.) without API costs
- You need offline access

```bash
# Install and run a model
ollama pull llama3.2
ollama run llama3.2
```

Local models are weaker than Claude or GPT-4 for complex reasoning, but for code explanation, simple generation, and documentation they're useful and free after the initial setup.

Models worth trying for code work:
- **Qwen2.5-Coder** — strong on code tasks
- **Mistral** — good general-purpose, fast
- **Llama 3.2** — solid reasoning, Meta's open model

---

## Practical AI Workflow for Test Automation

This is roughly how I integrate AI into a testing task:

1. **Understand the feature first** — read the ticket, talk to a dev, look at the code. AI is faster when you already know what to ask.
2. **Use Claude Code to scaffold** — generate the skeleton of the test file, page objects, and fixtures.
3. **Fill in with Copilot** — let it autocomplete the repetitive parts as you work through the file.
4. **Review and run** — always run the tests. AI-generated tests can be syntactically correct but logically wrong.
5. **Iterate** — if a test is flaky or the assertions are weak, ask Claude Code to review it.

---

## What AI Won't Do For You

- It won't understand your product. You still need domain knowledge to write meaningful tests.
- It won't catch logic bugs in the code under test — it can only test what you tell it to test.
- It won't keep tests maintainable. Over-generated test suites become a maintenance burden.
- It hallucinates APIs and method signatures — always verify against real docs.

The goal is to spend less time on boilerplate and more time on thinking about what actually needs to be tested.

---

## Links

- [Claude Code Docs](https://docs.anthropic.com/en/docs/claude-code)
- [GitHub Copilot](https://github.com/features/copilot)
- [Ollama](https://ollama.com/)
- [Anthropic API Docs](https://docs.anthropic.com/)
