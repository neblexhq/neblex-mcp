# Clean and dedupe incoming leads

## Prompt

> Build a webhook flow that receives a list of leads, trims names and companies, lowercases emails, drops leads that repeat an email, and sends leads with no email to a separate "needs review" branch.

## What gets built

| Step | Kind | What it does |
|---|---|---|
| Leads in | Webhook trigger | Receives `{"leads": [...]}` |
| One record per lead | Unwind | Splits the list into records |
| Trim and lowercase | Transform | Trims name and company, lowercases and trims email, turns null into "" |
| Drop repeat emails | Deduplicate (exact on email) | Keeps the first lead per email |
| Ready to load | Placeholder | Where the clean leads go next (CRM, database, warehouse) |
| Needs review: no email | Placeholder | Leads without an email, kept apart instead of lost |

Leads without an email branch off before the duplicate check, so they are not merged with each other.

## Test input

```json
{
  "leads": [
    { "name": " Dana Reyes ", "email": "Dana.Reyes@Example.com ", "company": "Northwind" },
    { "name": "Dana Reyes", "email": "dana.reyes@example.com", "company": "Northwind Traders" },
    { "name": "Sam Okafor", "email": "sam@contoso.com", "company": "Contoso" },
    { "name": "Lee Park", "email": "", "company": "Fabrikam" },
    { "name": "Ana Silva", "email": null, "company": "Tailspin" }
  ]
}
```

## Result

| Branch | Leads |
|---|---|
| Ready to load | Dana Reyes (dana.reyes@example.com), Sam Okafor (sam@contoso.com) |
| Needs review | Lee Park, Ana Silva |

The two Dana Reyes leads only match after cleaning: the first had a capitalized email with a trailing space.

## Take it further

- Replace "Ready to load" with a connector step that creates or updates the lead in your CRM.
- Replace "Needs review" with a form so someone can add the missing email.
- Switch the duplicate check to Jaro-Winkler on name and company to also catch leads that use different emails.
