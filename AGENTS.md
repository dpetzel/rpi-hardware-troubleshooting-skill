# Project Rules

## Markdown Line Width

All markdown files in this repository must follow a **120-character line width** limit.

- Wrap prose lines at or before 120 characters.
- Markdown table rows are exempt (they cannot be wrapped without breaking rendering).
- URLs that exceed 120 characters on their own line are exempt (do not break URLs mid-string).
- Indented continuation lines (for wrapped list items, blockquotes, etc.) count from column 0.
- YAML frontmatter string values should use `>` folded block scalar if they would exceed 120 characters on one line.
