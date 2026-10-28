# Find likely duplicate contacts in HubSpot

## Prompt

> Build a flow that reads my HubSpot contacts and lists the ones that look like duplicates by name, using fuzzy matching so small typos still match. Read-only: don't change anything in HubSpot.

## What gets built

| Step | Kind | What it does |
|---|---|---|
| Run on demand | Webhook trigger | Starts the flow |
| List HubSpot contacts | HubSpot connector | Reads contacts with first name, last name, email and company, following every page |
| Clean fields | Transform | Trims names and company, lowercases email, turns empty values into "" |
| Full name | Template | Joins first and last name |
| Trim name | Text | Removes stray spaces |
| Skip contacts without a name | Filter | Keeps blank names out of the comparison |
| Find similar names | Deduplicate (Jaro-Winkler, threshold 90) | Tags each contact with `_duplicate` and `_dup_of` instead of dropping it |
| Keep likely duplicates | Filter | Keeps contacts where `_duplicate` is true |
| Duplicate report | Map | Contact id, name, email, company and the name it looks like |

All native steps; no code.

## Result

In a HubSpot test portal seeded with duplicates, the flow read 838 contacts across 9 pages and flagged 233 as likely duplicates in one run.

## Take it further

- Add the email or company to the match (`by: "name,company"`) to cut false positives on common names.
- Lower the threshold to catch more variation, or raise it to flag only near-identical names.
- Send the report to a Slack channel or a review form instead of reading it in the run log.
