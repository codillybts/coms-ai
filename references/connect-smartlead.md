# Connect Smartlead to Claude

Phase 3. Smartlead has an official MCP server. Once it is added as a connector, Claude and the CRM can read replies, send approved emails, manage campaigns, and check mailboxes.

Source: https://helpcenter.smartlead.ai/en/articles/300-smartlead-mcp-server

## What to tell the user

"Now we connect Smartlead to Claude. This is what lets your CRM read replies and send the emails you approve. It takes about 3 minutes. You'll do it in your claude.ai settings, not here in the chat, because it uses your API key."

## Steps for the user

1. **Get your Smartlead API key.** In Smartlead, open Settings and copy your API key. Do not paste it into this chat.
2. **Build the connector URL** by putting your key at the end of this address:
   ```
   https://mcp.smartlead.ai/sse?user_api_key=<YOUR_API_KEY>
   ```
   Replace `<YOUR_API_KEY>` with the key. No spaces, no brackets.
3. **Open claude.ai in your browser.** Go to Settings, then Connectors.
4. **Add a custom connector.**
   - Name: **Smartlead** (exactly this, with a capital S. The CRM looks for a connector with this name.)
   - URL: the address from step 2.
   - Save.
5. **Come back to Claude Code** and say "done". If the connector does not show up, start a new session or check the connectors menu in the Claude desktop app. Connectors from your claude.ai account are available to Claude Code when you are signed in with the same account.

## Keep the URL secret

The connector URL contains your API key. Anyone with it can control your Smartlead account.
- Never paste it into chat, a document, the CRM, the profile, or a file in the working folder.
- If it was shared by mistake: create a new API key in Smartlead, delete the old one, and update the connector URL.
- If the user pastes the key or URL into chat anyway: do not repeat it, do not save it, and tell them to rotate the key.

## Check that it works

After the user says it is done:

1. Look for Smartlead tools in the session (tool names usually include the connector's name or id). If the session cannot see them yet, ask the user to restart the session and run the check again.
2. Make one read-only call, for example list the email accounts or list campaigns.
3. Tell the user what you found in plain words: "Connected. I can see 4 mailboxes and 0 campaigns."
4. If the call fails:
   - "Unauthorized" or similar: the API key is wrong or was rotated. Rebuild the URL with the current key.
   - No tools at all: the connector name or URL may be wrong, or the session started before it was added. Check the name is exactly **Smartlead**, then start a new session.
   - Anything else: show the error message and check the Smartlead help article above.

Do not move to Phase 4 until this check passes, unless the user chose a different sending tool.

## How the CRM uses the connector

- The CRM page declares the `mcp` capability for the connector named **Smartlead**. When the user opens the page, it asks Claude to call Smartlead tools on their behalf.
- A page can use at most **128 tools from one connector**. Smartlead's server offers more than that, so the CRM template lists only the tools it needs (inbox, sending replies, campaigns, sequences, leads, mailboxes, analytics, unsubscribes). If a feature ever needs a tool outside that list, update the template's tool list and republish (see `build-the-crm.md`).
- If the user renames the connector, the CRM will show "Smartlead not connected". Rename it back to **Smartlead**, or update `crm.connector_name` in the profile and the CRM settings.

## Smartlead behavior worth knowing

These come from how Smartlead's tools work. Claude follows them when creating campaigns from Claude Code or the CRM.

- New campaigns start as drafts and send nothing until started.
- Order for a new campaign: create the campaign, save the sequence steps, attach mailboxes, add leads, set the schedule (timezone, days, hours), then start.
- Saving sequence steps replaces all steps. When editing, always send every step, not just the changed one.
- Starting a campaign only checks that a schedule exists. Before starting, confirm the campaign has steps, mailboxes, and leads, or nothing will send.
- The first email's subject must not start with "Re:", "Fw:", or "Fwd:".
- Personalization uses merge tags in double curly braces, such as `{{first_name}}` and `{{company_name}}`.
- Status values you read (drafted, active, paused, stopped, completed) differ from the ones you set (start, pause, stop).
- Lead uploads go in batches of up to 1,000. Always check how many were actually uploaded.
- A tool result marked as an error means it failed. Fix and retry. Never report it as done.

Always ask the user before starting, pausing, or deleting a campaign, and before adding leads to a live campaign.
