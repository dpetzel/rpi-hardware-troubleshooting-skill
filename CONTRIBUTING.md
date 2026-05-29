# Contributing

This skill is designed to evolve. Hardware changes, new Pi models ship, and real-world troubleshooting reveals patterns not covered here.

## What to Contribute

- **New failure patterns** you've encountered and confirmed
- **Model-specific quirks** discovered through experience
- **Corrections** to outdated specs or diagnostic steps
- **New resource files** for uncovered subsystems (e.g., PCIe on Pi 5, HAT compatibility)

## Guidelines

### Reference Files

References should capture information that:
1. Is essential for diagnosis (not just "nice to know")
2. Could disappear from the web or be hard to find quickly
3. Is factual and verified (not speculation)

Keep references concise — tables and bullet points over prose. Include the model/revision the info applies to.

### SKILL.md Changes

The body of `SKILL.md` defines the troubleshooting methodology. Changes here should:
- Preserve the top-down elimination approach
- Not add model-specific details (those belong in references)
- Keep total body under 500 lines (per Agent Skills spec progressive disclosure)
- Be tested by running through a few scenarios mentally

### Adding a New Reference

1. Create a `.md` file in `references/`
2. Add a relative link in the "Reference Files" section of `SKILL.md`
3. Follow the format of existing files (title, tables, diagnostic steps)

### Versioning

- Bump patch version for resource corrections/additions
- Bump minor version for new resource files or methodology changes
- Bump major version for structural changes to the skill

## Process

1. Fork the repo
2. Make changes on a branch
3. Test by reviewing against real troubleshooting scenarios
4. Open a PR with a description of what was added/changed and why
