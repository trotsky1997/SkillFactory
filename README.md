# SkillFactory

A small Agent Skills repository for Pi focused on creating, capturing, and improving skills.

## Included skills

- `skillify` - turn a successful conversation or workflow into a reusable `SKILL.md`
- `skill-creator` - draft, test, evaluate, and iterate on skills with a structured review loop

## Install with `npx skills add`

```bash
# List skills available in this repo
npx skills add trotsky1997/SkillFactory --list

# Install skillify into the current project's Pi skills directory
npx skills add trotsky1997/SkillFactory --skill skillify -a pi -y

# Install skill-creator into the current project's Pi skills directory
npx skills add trotsky1997/SkillFactory --skill skill-creator -a pi -y

# Install both globally for Pi
npx skills add trotsky1997/SkillFactory --skill skillify --skill skill-creator -a pi -g -y
```

Pi project installs go to `.pi/skills/`. Global installs go to `~/.pi/agent/skills/`.

## Layout

```text
skills/
  skillify/
    SKILL.md
  skill-creator/
    SKILL.md
    agents/
    assets/
    eval-viewer/
    references/
    scripts/
```
