# Claude Code Skills

Reusable coding-agent skills for production app work. Each skill focuses on transferable rules, guardrails, workflows, and validation gates instead of project-specific prompt boilerplate.

## Skills

| Skill | Use it for |
| --- | --- |
| `react-native-ui-from-figma` | Build mobile screens from Figma design data. |
| `backend-api-contract-builder` | Implement backend APIs from contracts and product rules. |
| `api-integration-wiring` | Wire completed backend APIs into existing frontends. |
| `appium-flow-qa` | Run read-only Appium QA against mobile screens and user journeys. |
| `product-system-design` | Generate product analysis and backend-ready contracts from requirements plus app code. |
| `supabase-app-backend` | Build Supabase schema, row-level security, Edge Functions, generated types, and app stitching. |
| `figma-page-builder` | Convert Figma pages into production web output for the target app or platform. |

## Install

Install all skills:

```bash
git clone https://github.com/asifdotdev/claude-code-skills.git
mkdir -p ~/.codex/skills
cp -R claude-code-skills/skills/* ~/.codex/skills/
```

Install one skill:

```bash
mkdir -p ~/.codex/skills
cp -R claude-code-skills/skills/react-native-ui-from-figma ~/.codex/skills/
```

Then invoke a skill explicitly, for example:

```text
Use $react-native-ui-from-figma to implement this screen from Figma.
```

## Repo Shape

```text
skills/
  react-native-ui-from-figma/
    SKILL.md
    agents/openai.yaml
  ...
```

Each skill is intentionally small enough to load directly. There are no long project-specific master prompts bundled in the skill folders.

## Validate

From this repo:

```bash
VALIDATOR="/path/to/skill-creator/scripts/quick_validate.py"
for skill in skills/*; do
  python3 "$VALIDATOR" "$skill"
done
```

## License

MIT
