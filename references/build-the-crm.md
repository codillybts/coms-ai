# Build the CRM

Phase 5. The CRM is a private claude.ai artifact (a web app only the user can open unless they share it). It is published from the template `assets/app.html`, then filled with the user's settings through the ArtifactData tool. It starts empty: no demo leads, no sample replies.

Follow these steps in order. Tell the user what is happening at each step in one short line.

## Step 0: check the preconditions

- [ ] `coms-profile.json` exists and `setup.profile_confirmed` is true.
- [ ] The Smartlead connector check in `connect-smartlead.md` passed, and you know the connector's exact name (usually **Smartlead**). Save it to `crm.connector_name`.
- [ ] These files exist in the skill folder (usually `~/.claude/skills/coms-ai/`): `assets/app.html`, `assets/vendor/d3.min.js`, `assets/vendor/topojson.min.js`, `assets/vendor/xlsx.full.min.js`, `assets/maps/us-counties-albers-10m.json`.
- [ ] Voice rules are saved (`voice.rules` is not empty, or the user chose the defaults).

If anything is missing, finish it first.

## Step 1: load the profile

1. Read `coms-profile.json` and validate it against `assets/profile.schema.json`.
2. Confirm there are no secrets in it. If you find an API key, token or connector URL, remove it, move it to `.env` if it is needed, and tell the user.

## Step 2: make the working copy

1. Create a folder `crm/` in the working folder with `crm/vendor/` and `crm/maps/`.
2. Copy `assets/app.html` to `crm/coms-crm.html`, the three files in `assets/vendor/` to `crm/vendor/`, and `assets/maps/us-counties-albers-10m.json` to `crm/maps/`. Never edit the files in the skill folder.
3. In `crm/coms-crm.html`, change exactly two things (see "Template config" below):
   - The `<title>` on the first line: `<title>COMS AI</title>` becomes `<title>{app.display_name}</title>`.
   - The config block on the second line.
4. Never put any of these in the HTML: API keys, connector URLs, `.env` values, lead data, voice samples. Everything else lives in the database (Step 5).

## Step 3: load the artifact skills

Load `artifact-design` and `artifact-capabilities` before publishing, as the Artifact tool requires. Use the capability formats from `artifact-capabilities` exactly.

## Step 4: publish

Call the Artifact tool (publish) with:

| Parameter | Value |
|---|---|
| `file_path` | absolute path of `crm/coms-crm.html` |
| `icon` | `inbox` |
| `description` | "Private cold outbound CRM and master inbox for {business name}." |
| `files` | `{"vendor/d3.min.js": "crm/vendor/d3.min.js", "vendor/topojson.min.js": "crm/vendor/topojson.min.js", "vendor/xlsx.full.min.js": "crm/vendor/xlsx.full.min.js", "maps/us-counties-albers-10m.json": "crm/maps/us-counties-albers-10m.json"}` (absolute paths) |
| `capabilities` | below |

```json
{
  "db": {},
  "user": {},
  "sample": {},
  "downloads": true,
  "assets": {},
  "mcp": { "servers": [ { "server": "<crm.connector_name>", "tools": [
    "get_email_accounts", "fetch_master_inbox_replies", "fetch_master_inbox_sent_emails", "get_campaigns",
    "get_campaign_analytics", "get_overall_stats_v2", "get_day_wise_overall_stats", "get_campaign_lead_message_history",
    "send_campaign_email_thread", "change_master_inbox_read_status", "add_leads_to_campaign", "create_campaign",
    "save_campaign_sequences", "add_email_accounts_to_campaign", "update_campaign_schedule", "update_campaign_settings",
    "update_campaign_status", "get_campaign_email_accounts", "get_campaign_sequences", "get_lead_by_email",
    "unsubscribe_lead_globally", "add_domain_block_list" ] } ] }
}
```

These 22 tool names are the ones the template calls. Do not add others; the page never calls calendar, LinkedIn, CRM or inbound connectors directly (Claude syncs those, see `sync-jobs.md`), so they never go in this list.

If the publish is refused because a name doesn't match, list the connector's tools (they appear in your own tool list as `mcp__<id>__<tool>`) and correct the name or the connector name. Never guess.

After publishing:
1. Save the URL to `crm.artifact_url`, plus `crm.published_at` and `crm.template_version` ("1.0.0").
2. The result says whether this session watches the artifact. Never claim a watch it didn't confirm.

## Step 5: seed the database

Load the ArtifactData tool (ToolSearch "select:ArtifactData" if needed). Write the first records in **one batch**, using the CRM URL. Seed only what the user gave. No demo data.

### `settings/app` (one document)

```json
{ "appName": "<app.display_name>", "schemaVersion": 1, "openPage": "home",
  "autoSend": { "on": false }, "verifier": { "connected": false, "name": "" },
  "team": { "assign": false }, "dataTools": ["<names from tools.scraping_and_enrichment>"] }
```

### `businesses/<business id>` (one document per business)

| Field | From the profile |
|---|---|
| `name`, `sub` | `name`; one line from `basics.what_you_sell` |
| `brand` | `accent_color` or `app.brand_colors[0]` (hex) |
| `logoUrl` | `logo_asset_url` (Step 6), or leave out |
| `sender`, `senderTitle` | `voice.sender` name and title |
| `cta`, `calendarLink` | `basics.call_to_action`; the booking link if they gave one |
| `profile` | `{ offer, pricing, proof, differentiators, objections, faq, icpShort, dailyTarget }` written as plain sentences from `offer.*`, `basics.*`, `icp.*` and `goals.daily_new_leads`. `icpShort` is one sentence ("Owners of dental clinics with 5 to 50 staff in Texas"). These are the only facts Claude's drafts may use, so make them complete and true |
| `voice` | `{ tone, rules: [...], banned: [...], learnedFrom }` from `voice.*` |
| `firstTouch` | `{ wordsMin, wordsMax, subjectMaxWords, followUpDays: [3, 7] }` (defaults 50, 120, 5) |
| `stages` | `pipeline.stages` as `[{ id, color, odds }]`, or leave out for the defaults |
| `territory` | `icp.territory` as `{ label, states, counties }` (see `map-view.md`) |
| `match` | Only with 2+ businesses: `{ domains: [mailbox domains], campaignPrefixes: [name prefixes] }` so Smartlead replies and campaigns file under the right business. Campaigns for a business should start with its name |
| `linkedin` | `{ on: channels.linkedin, heyreach: { connected }, gojiberry: { connected }, sdr: { name, signAs, instructions } }` |
| `inbound` | `{ on, source, how }` from `inbound.*` |
| `calendar`, `crm` | `{ tool, connected }` from `integrations.*` |
| `createdAt`, `order` | timestamp; 0, 1, 2… for the menu order |

### `dnc/<id>`

One document per do-not-contact entry from the profile: `{ who, why, scope: "Everywhere" | "Whole domain", at, biz, synced: false }`. `who` is a lowercase email or domain.

Rules: use "set" so a rerun replaces cleanly; never write secrets; the mailing address and sender name are the user's own business details and are fine to store.

## Step 6: logo

If `app.logo_file` is set, upload it with the Artifact tool (`url` = CRM URL, `file_path` = logo, `asset: true`). Use the returned URL exactly as given: save it to `logo_asset_url` and write it to the business document's `logoUrl` ("update"). The page also has an Upload logo button for the owner.

## Step 7: manual sends campaign (one per business)

Compose and inbound replies send through a Smartlead campaign whose sequence is one step: subject `{{manual_subject}}`, body `{{manual_body}}`. Ask before creating it: "I'll create a campaign called '{business} · Manual sends' in Smartlead. It only sends emails you write in Compose or approve in Inbound. OK?" On yes:
1. `create_campaign` with that name. Read the id from the result.
2. `save_campaign_sequences` with one step: `{ seq_number: 1, seq_delay_details: { delay_in_days: 0 }, subject: "{{manual_subject}}", email_body: "{{manual_body}}" }`.
3. `add_email_accounts_to_campaign` with the ready mailboxes for that business.
4. `update_campaign_schedule`: the user's time zone, every day, 00:00 to 23:59, `min_time_btw_emails` 3, `max_new_leads_per_day` 100.
5. `update_campaign_settings`: plain text, open and click tracking off, stop on reply.
6. Ask before starting it, then `update_campaign_status` START.
7. Write `manualCampaignId` (the id, as a string) to the business document.

The user can also do this from the CRM (Campaigns > New campaign > "Manual sends").

## Step 8: read-back check

1. ArtifactData "get" `settings/app` and each business document. Compare with the profile: names, auto-send off, stages, territory, profile facts present.
2. Fix anything wrong with "update" and check again.
3. Open the CRM (Artifact tool, action open). claude.ai asks the user once to allow the Smartlead connector and Claude drafting for this page. Tell them to allow both.
4. Ask the user to check: the business name at the top, "Smartlead connected" at the bottom of the menu, and their mailboxes under Mailboxes.

Then give them the link and say: "This is your CRM. Bookmark it. It starts empty. Next we'll get your first leads in."

## Updating the CRM later

- **Settings** (colors, stages, voice rules, profile facts, territory, auto-send, a new business): write them with ArtifactData. No republish.
- **Template changes**: copy the new template, refill the title and config, and republish to the same URL (same `file_path`, or `url` = `crm.artifact_url` from a new session after reading it). Omit `capabilities` to keep them; never pass `contract: "latest"`. The database is kept.
- **Schema changes**: only add fields with defaults. Never delete or rename collections or fields. Bump `settings/app.schemaVersion`.
- Never delete the artifact unless the user asks. Deleting it removes all CRM data.

## Template config

Line 2 of `assets/app.html`:

```html
<script id="coms-config" type="application/json">{"appName":"COMS AI","brand":"#3346d3","smartleadServer":"Smartlead","templateVersion":"1.0.0"}</script>
```

| Key | Type | Fill from | Default |
|---|---|---|---|
| `appName` | string | `app.display_name` | "COMS AI" |
| `brand` | hex color | `app.brand_colors[0]` | "#3346d3" |
| `smartleadServer` | string | `crm.connector_name` (exact connector name) | "Smartlead" |
| `templateVersion` | string | leave as is | "1.0.0" |

Example: `{"appName":"Acme Desk Outbound","brand":"#0f7b74","smartleadServer":"Smartlead","templateVersion":"1.0.0"}`

Everything else the page needs is in the database. The collections it reads and writes, and what Claude writes into them, are in `sync-jobs.md`.
