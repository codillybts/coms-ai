# Running COMS AI day to day

Phase 7 (handoff) and every session after setup.

## The daily routine (about 15 minutes)

Give the user this at handoff:

1. **Open the CRM** and start on Home.
2. **Inbox:** read each reply and its draft. Edit if needed, then click Send. Opt-outs are already handled.
3. **Nudges:** approve or snooze the follow-ups due today.
4. **Leads:** review new leads. Approve the good ones.
5. **First emails:** approve the drafts in the queue so campaigns keep moving.

Once a week (about 30 minutes):
- Check Mailboxes for warnings and Analytics for reply rates.
- Queue new lead searches so next week has leads.
- Ask Claude: "review my week" for a short summary and one or two things to change.

## Scheduled tasks (optional)

Claude Code can run tasks on a schedule. Offer these at handoff and create only the ones the user says yes to. Ask for the time and days. Mention that local scheduled tasks run while the computer is on and the Claude app is running.

| Task | What it does | Suggested schedule |
|---|---|---|
| Morning brief | Summarizes what needs the user today | Weekdays at 8:00 |
| Lead search run | Runs any queued "Find leads" searches | Daily, early morning |
| Verification run | Verifies newly approved leads with the user's verifier (key from `.env`) | Daily, after the lead search run |
| Weekly review | Sends a short summary of the week with suggestions | Fridays |

Each scheduled task must follow the hard rules: it may draft, research, verify, and summarize, but it never sends an email or LinkedIn message (only the CRM page sends, after the user's click), never adds leads to a live campaign, and never starts a campaign. Those need the user.

When creating a task, write a prompt that names the working folder (so it finds `coms-profile.json`) and the exact job, for example: "In ~/Documents/COMS AI, run my queued COMS AI lead searches following the coms-ai skill. Do not contact anyone."

### Morning brief

When the user asks for the morning brief (or the scheduled task runs it):
1. Read the profile and the CRM data through ArtifactData. Check Smartlead through the connector for new replies and mailbox health.
2. Report in under 15 lines:
   - Replies waiting, and how many look interested.
   - Nudges due today.
   - New leads waiting for review.
   - First emails waiting for approval.
   - Tasks due today.
   - Yesterday's numbers: sent, replies, positive replies, meetings booked.
   - Any warning: bounces, a mailbox problem, a stopped campaign.
   - One suggestion, only if there is a clear one.
3. End with the CRM link.

## Adding a business

When the user says "add a business" or "add a client":

1. Run interview rounds 1 to 7 for the new business only (see `intake-interview.md`, "Adding another business later"). Ask for its logo and color.
2. Show the summary and get a yes.
3. Append it to `businesses` in the profile.
4. Infrastructure: run the sizing calculator for it. Each business should have its own sending domains and mailboxes, so one business's results never affect another.
5. Smartlead: if the user is an agency, suggest using Smartlead's client feature to keep each business's mailboxes and campaigns separate. Tag mailboxes with the business id either way.
6. Voice: run Phase 4 for the new business (its own rules).
7. Seed the new business into the CRM with ArtifactData (a `businesses` document in the format in `build-the-crm.md`, plus `dnc` entries). Upload its logo as an asset. No republish needed. Offer to set up its manual sends campaign (`build-the-crm.md`, Step 7).
8. Tell the user to pick it from the business switcher.

## Changing anything

The rule: **if the user wants something changed, change it.** Then make it stick:

| Change | Where to make it |
|---|---|
| Voice, tone, banned words, sign-off | Profile, `voice-rules.md`, and the `voice` field of the business document in the CRM |
| ICP, offer, objections | Profile and the CRM's `businesses` record |
| Colors, logo, name | Profile and the CRM's `settings` (logo: upload as an asset first) |
| Pipeline stages, tags | Profile and CRM `settings` or `businesses` |
| Auto-send | CRM Settings (the user can do it there), then mirror it in the profile |
| A CRM screen or feature | Template change, then republish (see `build-the-crm.md`) |
| Sending volume | Smartlead mailbox limits through the connector (ask first), and the profile |

Confirm the change in one line when done.

## Updating the skill

There are no automatic updates and COMS AI never contacts GitHub. The user's copy keeps working as is. If they ever replace the folder with a newer copy, rebuild the CRM with "rebuild my CRM"; stored data is kept (see "Keep it from breaking" in SKILL.md).

## Backups

- The CRM can export leads, contacts, deals, and the do-not-contact list as CSV (Settings or each screen's export button). Suggest a monthly export.
- `coms-profile.json` and `voice-rules.md` are plain files in the working folder. Back them up like any other file.
- Never back up `.env` to a shared place.

## Troubleshooting

| Problem | What to check |
|---|---|
| CRM says "Smartlead not connected" | Connector in claude.ai Settings, Connectors is named exactly **Smartlead**. The API key in its URL is current. Reload the CRM |
| Claude Code cannot see Smartlead tools | Start a new session. Check the connector is enabled for the session |
| A feature says a tool is missing | The CRM can use at most 128 tools from one connector. Add the tool to the template's allowlist and republish |
| Drafts are not appearing | The page needs the `sample` capability. Check it is still declared. Reload |
| Lead search stuck on "running" | The run was interrupted. Say "run my lead searches" again. Searches marked running for over an hour are retried |
| Bounces are high | Pause the campaign. Verify the list. Check where the leads came from |
| Replies are going to spam | Check SPF, DKIM, DMARC on the Mailboxes screen. Lower daily volume. Keep open tracking off |
| The profile and the CRM disagree | The profile is the source for setup answers. Ask the user which is right, then update both |
| Anything else | Describe it to Claude. Check the CRM's Instructions tab |
