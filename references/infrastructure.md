# Infrastructure: sending, domains, mailboxes

Phase 2. Goal: the user knows exactly what to buy, how many, and when cold sending can safely start.

## What to tell the user first

"Cold email needs its own setup so it never hurts your main business email. We'll use separate domains and mailboxes, warm them up for a few weeks, then start slowly. Here is the plan."

## The default path and the alternatives

Show each item with its link, say it is an affiliate link, and always offer the alternative.

| Need | Recommended (affiliate link) | Alternative |
|---|---|---|
| Sending, warmup, and master inbox | **Smartlead**: https://smartlead.ai/?via=dilly | "I already have Smartlead" or "I use a different service" |
| Domains and mailboxes | **InboxKit**: https://www.inboxkit.com/?aff=dilly | "I already have my own domains and mailboxes" or "I use a different provider" |
| LinkedIn campaigns (only if they do LinkedIn) | **HeyReach**: https://heyreach.io/?via=dilly | See `linkedin.md` |
| LinkedIn AI SDR (only if they do LinkedIn) | **GojiBerry**: https://gojiberry.ai/?ref=dilly | See `linkedin.md` |

Use the AskUserQuestion tool for each: "Recommended: Smartlead (affiliate link)", "I already have Smartlead", "I use a different service".

Important: the CRM is built on the Smartlead connector. If the user picks a different sending tool, tell them plainly: the Inbox, Campaigns, and Mailboxes screens need Smartlead to work. Leads, Pipeline, Contacts, Tasks, and the writers still work, and they can export approved leads as CSV for their own tool. Record the choice in `infrastructure.sending_provider`.

Links in chat are plain links so they open in the user's default browser. Do not open them in a browser tool unless the user asks.

## Sizing calculator

Work this out from `goals.daily_new_leads` and show the result as a table.

**Step 1. Emails per day.** Each new lead gets a first email plus follow-ups. Once the sequence is running, the daily volume is about:

`emails per day = new leads per day x steps in the sequence`

Default sequence: 3 steps (first email plus 2 follow-ups). So 25 new leads a day becomes about 75 emails a day after the first week or two.

**Step 2. Mailboxes.** A common industry guideline is about 20 to 30 cold emails per mailbox per day. COMS AI uses 20 to stay safe. The user can adjust it.

`mailboxes = emails per day / 20`, rounded up

**Step 3. Domains.** Put 2 to 3 mailboxes on each domain. COMS AI uses 3, with a minimum of 2 domains so one problem domain never stops everything.

`domains = mailboxes / 3`, rounded up, minimum 2

**Step 4. Warmup.** New mailboxes need about 14 to 21 days of warmup before any cold email. Then ramp up slowly.

### Quick table (3-step sequence, 20 per mailbox, 3 mailboxes per domain)

| New leads per day | Emails per day | Mailboxes | Domains |
|---|---|---|---|
| 25 | 75 | 4 | 2 |
| 50 | 150 | 8 | 3 |
| 100 | 300 | 15 | 5 |
| 200 | 600 | 30 | 10 |
| 500 | 1,500 | 75 | 25 |

Tell the user these are starting points, not rules. Adjust if replies or bounces suggest otherwise. Save the results to `infrastructure.planned_mailboxes`, `infrastructure.planned_domains`, and `infrastructure.sends_per_mailbox_per_day`.

### Timeline

Work out real dates from today and show them.

| When | What happens |
|---|---|
| Day 0 | Buy domains, create mailboxes, set DNS records |
| Day 1 | Connect mailboxes to Smartlead, turn on warmup for every mailbox |
| Day 1 to 14 | Warmup only. Meanwhile: finish voice rules, build the CRM, find and approve leads, write the first sequence |
| Day 14 to 21 | First cold emails at about 5 to 10 per mailbox per day |
| Every 3 to 4 days after | Add about 5 per mailbox per day, up to the target (20) |
| Always | Leave warmup running in the background, even during campaigns |

Save `warmup_start_date` and `cold_send_start_date`. Phase 6 must not start a campaign before `cold_send_start_date` unless the user insists after hearing the risk.

## Domains: the rules

- **Never send cold email from the main domain.** If something goes wrong, the main business email is safe.
- Use secondary domains that look related. Example for a fictional business at `acmedesk.example`: `getacmedesk.example`, `tryacmedesk.example`, `acmedeskhq.example`.
- Prefer `.com` or the business's usual ending. Avoid cheap, unusual endings that spam filters distrust.
- Point each secondary domain to the main website (a simple redirect), so anyone who checks it finds the real business.
- Mailbox names should be real people, like `sam@getacmedesk.example`. Avoid `info@` or `sales@` for cold email.

## SPF, DKIM, and DMARC in plain words

These are DNS records. They prove the email really comes from you. Without them, most email lands in spam or gets rejected. InboxKit and most mailbox providers can set them up automatically.

| Record | What it does, simply | What to check |
|---|---|---|
| SPF | A list of the servers allowed to send email for your domain | One SPF record per domain that includes your mailbox provider |
| DKIM | A signature on each email that proves it was not changed and came from your domain | DKIM turned on in the mailbox provider, with the record added to DNS |
| DMARC | Tells other mail servers what to do if SPF or DKIM fail, and where to send reports | Start with `p=none`. Move to `p=quarantine` once everything passes |
| MX | Where replies are delivered | Points to the mailbox provider so replies arrive |

Smartlead can check these for each mailbox once connected. COMS AI can also check them through the connector and show the result on the Mailboxes screen.

## Smartlead setup checklist

1. Create the account: https://smartlead.ai/?via=dilly (affiliate link). Pick a plan that covers the number of mailboxes and leads from the calculator.
2. Add mailboxes (Email Accounts, then add). If InboxKit offers a direct connection to Smartlead, use it. Otherwise add them one by one.
3. Turn on warmup for every mailbox.
4. Set each mailbox's daily limit to the ramp start (5 to 10). Raise it later.
5. In campaign settings: **open tracking off**, **link tracking off**, **plain text on**. Open tracking adds a hidden image, which hurts deliverability and is unreliable anyway. Plain text looks like a normal person's email.
6. Find the API key in Settings. Do not paste it in chat. It is used in Phase 3.

## InboxKit setup checklist

1. Create the account: https://www.inboxkit.com/?aff=dilly (affiliate link).
2. Buy the number of domains from the calculator, using names like the examples above.
3. Create the mailboxes (2 to 3 per domain) with real first names.
4. Let it set SPF, DKIM, DMARC, and MX automatically. Set up the redirect to the main website.
5. Connect the mailboxes to Smartlead.

Their onboarding may change. Follow their screens if they differ from these steps.

## If the user already has their own setup

Ask: how many domains, how many mailboxes, which provider, how old they are, and whether warmup is running. Then:
- Check that none of them is the main domain. If one is, recommend moving cold email off it.
- Run the calculator and say whether they have enough.
- Check DNS records through Smartlead once connected.
- If mailboxes are older than 3 weeks with warmup running, they can skip the warmup wait.

## Email verification

Verifying emails before sending keeps bounces low. Bounces hurt deliverability.

- Any email verification service works. COMS AI does not recommend a specific one.
- To connect one: put its API key in the `.env` file in the working folder, for example `VERIFIER_API_KEY=...`. The Claude Code session reads it from there when running verification.
- The key never goes into the CRM page, the profile, or chat. The profile only stores the variable name (`tools.verifier.env_var`).
- Make sure `.env` is listed in `.gitignore`.
- Verification runs from Claude Code ("verify my approved leads"), not from the CRM page. Results are written back to each lead as valid, risky, invalid, or unknown. Only valid leads go into campaigns by default.

## Health targets to keep an eye on

These are common industry guidelines. The Mailboxes screen shows them.

| Metric | Healthy | Act if |
|---|---|---|
| Bounce rate | Under 2% | Over 3%: pause, verify the list, check the lead source |
| Spam complaints | As close to 0 as possible, under 0.1% | Over 0.3%: pause and review targeting and copy |
| Warmup health | Steady and high in Smartlead | Dropping: lower daily volume for that mailbox |
| Reply rate | Over 1% | Under 0.5% for 500+ sends: change the list or the message |

Large mailbox providers publish sender rules. In short: authenticate with SPF, DKIM, and DMARC, keep complaints very low, and make opting out easy. COMS AI's defaults follow these.
