# rpi-hardware-troubleshooting

An [Agent Skill](https://agentskills.io) for interactive diagnosis and debugging of Raspberry Pi hardware issues
across all models.

## Philosophy

This skill uses a **top-down elimination** approach. Rather than diving into edge cases, it guides users through
questions that eliminate the largest remaining variables first:

1. **Power** — Is the device receiving adequate power?
2. **Boot** — Does the device show signs of life?
3. **Storage** — Is the boot media functional?
4. **Peripherals** — Are connected devices causing issues?
5. **Software boundary** — Only after hardware is confirmed working

## Structure

```
rpi-hardware-troubleshooting/
├── SKILL.md              # Skill definition (frontmatter + instructions)
├── references/
│   ├── power-issues.md   # Power supply reference
│   ├── boot-sequence.md  # Boot diagnostics by model
│   ├── led-codes.md      # LED status codes
│   ├── peripheral-issues.md # Common peripheral problems
│   ├── model-specs.md    # Key specs and known issues per model
│   ├── pi5_pmic_breakdown.md # Pi 5 PMIC area component map
│   └── raspberrypi5-pmic_area_indexed.png # Indexed photo of PMIC area
├── CONTRIBUTING.md
└── README.md
```

Conforms to the [Agent Skills specification](https://agentskills.io/specification).

## Usage

Add this skill directory to your agent's skill path. The agent will read the `SKILL.md` frontmatter to decide when
to activate it, then load the full instructions when a user describes a Raspberry Pi hardware problem.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

MIT
