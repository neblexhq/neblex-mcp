# Example prompts

Prompts you can give an AI client connected to the Neblex MCP server, with the flow each one produced and what happened when it ran. Every example here was built through the MCP server and run in a Neblex workspace before it was added.

| Example | Systems | What it shows |
|---|---|---|
| [Find likely duplicate contacts in HubSpot](hubspot-duplicate-contacts.md) | HubSpot | Fuzzy matching (Jaro-Winkler) on names, read-only |
| [Clean and dedupe incoming leads](clean-incoming-leads.md) | Webhook | Normalizing emails, removing duplicates and routing incomplete records before they load anywhere |

To try one, connect your client (see the [main README](../README.md)), make sure the connection it needs exists in your workspace, and paste the prompt. The client will validate and test the flow before anything is published.
