# Optional extras: calendar, inbound, current CRM, team

Each of these is off unless the user's setup answers turn it on. Read the section you need.

## Calendar

**When:** R4.8 names a calendar or booking tool, or the user says "connect my calendar".

**What it does in the CRM**
- A Calendar page lists upcoming meetings that include a lead's email address. Other events stay private and are never shown or stored. The page only shows a count of hidden events.
- When a lead books, the lead moves to "Meeting booked" by itself.
- Before each meeting, Claude writes a short prep note from the conversation, deal notes and the lead's public website.
- If a lead doesn't show, a task and a drafted follow-up appear, and the user approves before anything sends.
- "Copy booking link" copies the link from the profile (`offer.calendar_link`).

**How to connect**
1. Most calendars (Google Calendar, Outlook, Calendly, Cal.com) can be added on claude.ai under **Settings > Connectors**. Ask the user to add theirs and sign in. For anything else, check whether it has a Claude connector or an API key. If it needs a key, the key goes in `.env`.
2. When they say "connect my calendar", list the connector's tools and make one safe read (for example, the next 7 days of events). Never create, change or delete events unless the user asks.
3. Republish the CRM with the calendar connector added to its `mcp` capability, naming only the read tools it uses. Save `integrations.calendar.tool` and `integrations.calendar.connected`.

No calendar is fine. The Calendar page then shows connect options, and deals move to Meeting booked by hand.

## Inbound

**When:** R4.9 is yes, or the user turns on Inbound in Settings. The Inbound page and its menu item only appear when it is on.

**Setup:** ask where inbound leads come from and how Claude can see them. Turning Inbound on in Settings asks the same two things before it switches on.
- `inbound.source`: plain words, for example "the demo request form on our website" or "chat on our pricing page".
- `inbound.how`: one of "A form tool connected to Claude", "Forwarded into one of my Smartlead mailboxes", "A CSV export I'll upload", or "Something else". For "Something else", ask follow-up questions until you know the exact path.

Then set it up case by case:
- **Form tool with a Claude connector or API:** connect it the same way as any connector (a read-only check first). New submissions become inbound items.
- **Forwarded into a Smartlead mailbox:** inbound emails then arrive with the normal Smartlead replies. Tag them as inbound by the sending address or subject the user describes.
- **CSV export:** the user uploads it on the Inbound page, which uses the same importer as Leads.

**In the CRM:** each inbound item shows who it is, their message, the source, and Claude's draft reply. Send needs a click and a confirm, the same as the Inbox. Buttons: Add to leads, Spam, Dismiss. Once a reply is sent, the conversation continues in the Inbox.

Inbound leads asked to hear from the business, so replies can be warmer and can include a booking link. The voice rules still apply.

## Current CRM

**When:** R4.3a is yes, or the user says "connect my CRM".

This is case by case. Every CRM is set up differently.
1. Ask which CRM, and what should sync. The defaults are: new positive replies become contacts and deals, deal stages sync both ways, and the do-not-contact list syncs.
2. Check how it connects: a Claude connector on claude.ai, or an API key in `.env`.
3. Make one safe read (a single contact or deal), show it to the user, and confirm how fields line up (stage names, owner, deal value).
4. Only then turn on syncing. Write a short `crm-sync.md` in the working folder that records the field mapping, so it can be repeated or fixed later.
5. Never delete or overwrite records in their CRM without asking. When in doubt, add a note instead of changing a field.

Save `integrations.crm.tool`, `integrations.crm.sync`, `integrations.crm.connected`.

## Team

**When:** R7.8 says they're on a Claude Team or Enterprise plan with teammates.

- The CRM is a claude.ai artifact. The owner shares it from the page's **Share** menu with people in their organization. Teammates see the same inbox, leads and pipeline, because the data lives in the artifact's shared database.
- Each send records who approved it, using the viewer's own account (the `user` capability). Store user ids only, never names or emails.
- Settings > Team lists the people who have opened it and has an "Assign replies to teammates" switch. Turning it on adds an "Assigned to" field to every conversation.
- On a personal plan, the CRM stays single-user. That is the default.
- Claude can't invite people. The owner does that from the Share menu.
