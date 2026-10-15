# Filing feedback

Each issue is one piece of feedback about Entire. People skim the top; agents act on the rest.

## Title

- State the problem or request directly, under 70 characters.
- No type prefixes like `[Bug]`. The board column carries the type.
- Good: `Deleted items stay in the list until reload`. Bad: `Problem with the list`.

## Description

1. `## Summary` first: one or two sentences saying what happens or what is wanted. No backstory, no greetings.
2. Every section after it is context for agents, and should be complete enough to act on without asking: steps to reproduce, expected vs actual, versions, logs, links, screenshots, suspected cause. Follow the sections of the matching template in `.github/ISSUE_TEMPLATE/`.

## Putting it on the board

The board is https://github.com/users/Meldiron/projects/2. Its columns are the `Status` field.

| Type       | Label        | Status option ID |
| ---------- | ------------ | ---------------- |
| Bug        | `bug`        | `99d71d95`       |
| Suggestion | `suggestion` | `7de1da4f`       |
| UX         | `ux`         | `7bb7cc52`       |
| Question   | `question`   | `c59fcc28`       |
| Praise     | `praise`     | `827a6f95`       |
| Other      | `other`      | `eb255ea4`       |

```sh
url=$(gh issue create -R Meldiron/entire-feedback --title "..." --body-file body.md --label bug)
item=$(gh project item-add 2 --owner Meldiron --url "$url" --format json --jq .id)
gh project item-edit --project-id PVT_kwHOASao7s4BnJ8l --id "$item" \
  --field-id PVTSSF_lAHOASao7s4BnJ8lzhlv23s --single-select-option-id 99d71d95
```
