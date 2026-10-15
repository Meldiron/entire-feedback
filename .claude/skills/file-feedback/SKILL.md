---
name: file-feedback
description: File Matej's feedback on Entire as an issue in Meldiron/entire-feedback and place it in the right board column, or update an existing one. Use for every bug, suggestion, UX complaint, question or praise about Entire.
---

# File feedback

## 1. Classify

| Type       | When                                               | Label        | Status option ID |
| ---------- | -------------------------------------------------- | ------------ | ---------------- |
| Bug        | Broken, or behaves unexpectedly                    | `bug`        | `99d71d95`       |
| Suggestion | Missing feature or improvement idea                | `suggestion` | `7de1da4f`       |
| UX         | Works, but the flow, design or copy is confusing   | `ux`         | `7bb7cc52`       |
| Question   | Something unclear that needs an answer             | `question`   | `c59fcc28`       |
| Praise     | Something Matej likes                              | `praise`     | `827a6f95`       |
| Other      | Doesn't fit the above                              | `other`      | `eb255ea4`       |

## 2. Check for duplicates

```sh
gh issue list -R Meldiron/entire-feedback --state all --search "<2-3 keywords>"
```

If an issue already covers the point, add the new detail as a comment, and reopen it if it was closed and the problem is back. File a new issue only for new points.

## 3. Write

**Title:** state the problem or request directly, under 70 characters, with no `[Bug]`-style prefixes. Good: `Deleted items stay in the list until reload`. Bad: `Problem with the list`.

**Body:** start with `## Summary`, one or two sentences saying what happens or what is wanted, with no backstory. Every section after it is context for agents. Make it complete enough that an agent can act without asking.

| Type       | Sections after Summary                                     |
| ---------- | ---------------------------------------------------------- |
| Bug        | Steps to reproduce, Expected vs actual, Environment, Context |
| Suggestion | Problem, Proposed solution, Context                        |
| UX         | Where, What you expected, Context                          |
| Question   | What you checked, Context                                  |
| Praise     | Context                                                    |
| Other      | Context                                                    |

Leave out a section you have nothing for, except Summary and Context. Context holds:

- what Matej was doing when it came up
- exact error text, logs and commands pasted in the session
- versions and OS when known
- links
- screenshots described in words, because `gh` can't upload images
- open questions

Never invent any of it.

## 4. File and place on the board

Write the body to a temp file to avoid shell quoting, then:

```sh
url=$(gh issue create -R Meldiron/entire-feedback --title "$title" --body-file "$body" --label bug)
item=$(gh project item-add 2 --owner Meldiron --url "$url" --format json --jq .id)
gh project item-edit --project-id PVT_kwHOASao7s4BnJ8l --id "$item" \
  --field-id PVTSSF_lAHOASao7s4BnJ8lzhlv23s --single-select-option-id 99d71d95
```

Use the label and option ID from step 1.

## 5. Update an existing issue

- Add detail: `gh issue comment N -R Meldiron/entire-feedback --body-file "$body"`
- Rewrite: `gh issue edit N -R Meldiron/entire-feedback --title "$title" --body-file "$body"`
- Change type: run `gh issue edit N -R Meldiron/entire-feedback --remove-label bug --add-label ux`, then move the card. Get its item ID with `gh project item-list 2 --owner Meldiron --limit 1000 --format json --jq '.items[] | select(.content.number==N) | .id'`, and run the `item-edit` from step 4 with the new option ID.
- Resolved or withdrawn: `gh issue close N -R Meldiron/entire-feedback --comment "<why>"`

## 6. Confirm

Reply with one line per issue and nothing else:

```
Bug · Deleted items stay in the list until reload · https://github.com/Meldiron/entire-feedback/issues/1
```
