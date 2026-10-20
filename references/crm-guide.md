# How your CRM works

This is your private cold outbound CRM and master inbox. It runs inside Claude and connects to your Smartlead account. Only you can open it unless you share it.

Two promises:
- **Nothing is sent without your click.** Every email and reply waits for you to approve it. The only exception is auto-send for low-risk replies, which is off unless you turn it on in Settings.
- **Nobody is contacted without your approval.** Leads only get emailed after you approve their first emails and add them to a campaign.

If you want anything changed, ask Claude in Claude Code. Whatever you want changed gets changed.

## The menu and top bar

- **Business switcher** (top of the menu). Shows the business you're working on. Click it to switch, see all businesses, or add one.
- **Smartlead status** (bottom of the menu). "Smartlead connected" when everything works. If it says "not reachable", check that your connector in claude.ai Settings > Connectors has the name this CRM expects (usually **Smartlead**) and that you allowed it for this page.
- **Search, Refresh, How it works** sit in the top bar. The menu button hides or shows the menu. Press **?** for keyboard shortcuts.

## Home: what needs you now

Your daily starting point:
1. Replies waiting for you, interested ones first.
2. Inbound leads waiting, if Inbound is on.
3. Nudges due (interested leads who went quiet).
4. First emails waiting for approval.
5. Meetings today, with prep notes.
6. Mailbox warnings: disconnected, or low warmup health.

Tick a row when it's handled. Below that: your numbers, setup progress, and lead searches.

## Inbox

Every reply from every Smartlead mailbox, in one list. Tabs: **Replies**, **Nudges**, **Sent**. Filters: To reply, Interested, Done.

- Claude sorts each reply (interested, question, not now, referral, not interested, out of office, unsubscribe) and drafts an answer in your voice, using only facts from your business profile.
- Read the draft, edit it if you want, click **Send**, then confirm. It goes from the same mailbox, in the same thread.
- **Redraft** asks for a fresh draft. **Snooze** hides it until tomorrow. **Mark handled** closes it without replying. **Do not contact** blocks the person everywhere, also in Smartlead.
- Set the pipeline stage and deal value from the panel on the right.
- Unsubscribe replies are handled for you (if that rule is on): no reply is sent, and the person goes on Do not contact.
- LinkedIn conversations show here too, tagged LinkedIn, if you connected a LinkedIn tool.

## Nudges

Interested leads you replied to who then went quiet. The first nudge is due after 3 days, the second after 7. Claude drafts a short nudge that adds something new. Approve and send, or skip. Never more than 2 per lead.

## Compose

A new email to anyone, from the Inbox (**Compose**). Write it or have Claude draft it from a line about what you want to say. It goes out through your business's "Manual sends" campaign in Smartlead within a few minutes, so replies come back to this Inbox.

## Leads

Everyone you might contact.

- **Add lead:** one at a time.
- **Import:** click it, then choose or drag in a CSV or Excel (.xlsx) file, or paste rows. You'll see a column-matching step so each column goes to the right field (name, email, company, website, title...). Duplicates and blocked contacts are skipped.
- **Find leads:** describe who you want (prefilled from your ideal customer) and how many. This queues a search. Then, in Claude Code, say **"run my lead searches"**. Research agents look through public websites, directories, registries, news, job posts, and any tools you connected, and bring leads back here for review.
- **Review:** new leads arrive as "New". Remove any that don't fit; pick the rest and press Write first emails.

About emails on found leads: agents only record emails that are actually published somewhere public, and they save the link where they found it. They never guess. If no published email was found, the lead says **"no published email"**.

## Calendar

Meetings with your leads from the calendar you connected. Leads move to Meeting booked as soon as they book, and each meeting has a prep note. Other events stay private. Not connected? The page shows how to connect one.

## Inbound

Only shows if you turned it on. People who reached out to you first, with Claude's draft reply for each. Send needs your click. Settings > Inbound turns it on or off and records where these leads come from.

## Map View

A live map of every lead in your territory. Click a state to zoom in and see its counties, click a county to zoom further, and click any dot to see that lead. Dots are colored by status. The side panel shows numbers for the area you're looking at, the best-fit leads there, and a "Find leads here" button. Scroll or pinch to zoom; Esc goes back up a level. Leads only appear if their business address (or at least their city) is published online.

## Email checks

An email is only labeled Valid, Risky or Invalid after a connected email checker has tested it. With no checker connected, the label stays blank; Smartlead can check emails when leads go into a campaign. Connect one any time in Settings > Connections.

## First emails

Pick leads on the Leads page and press **Write first emails**. Claude writes each lead's first email and two follow-ups from your voice rules and one real fact about them, with its source. Checks run on each one (length, subject, no links, banned words, a real fact). Approve, edit or reject; **Approve all clean** approves every one that passed. Then **Add approved to a campaign** puts them in a campaign made with the "Approved first emails" sequence.

## Campaigns

Your Smartlead campaigns, with leads, sent, reply rate, interested and bounce rate.
- **New campaign** creates a draft in Smartlead: the email steps ("Approved first emails", or "Manual sends" for Compose and inbound replies), the mailboxes, days, hours, time zone and daily new-lead cap. Plain text, no open tracking, stops when someone replies.
- **Launch** checks the campaign has leads, mailboxes and email steps first, then starts it after you confirm.
- **Pause** and **Resume** any time. A bounce rate over 3% is flagged.

## Mailboxes

Every sending mailbox: status (ready, warming, check, disconnected), warmup progress, warmup health, sends today and daily limit, plus your domains. Ask Claude "check my domain records" to test SPF, DKIM and DMARC.

## Pipeline

Deals by stage, with totals and a weighted forecast. Anyone who replied with interest starts in Interested. Drag cards between stages, set values in the conversation, and edit stages and their chance of closing in Settings > Pipeline.

## Contacts

Everyone who replied, as people or grouped by company, with their stage and deal value. Click a person to open the conversation.

## Tasks and rules

- **Tasks:** to-dos with due dates. Rules create most of them (for example, a 1-hour task when someone is interested, or a reminder on the date a "not now" reply gave).
- **Rules:** interested → Interested stage + task; not now → reminder; unsubscribe → Do not contact everywhere; out of office → handled; referral → ask-first task. Turn each on or off.

## Analytics

Sent, replies, positive replies and bounce rate for the last 30 days, a daily chart, which campaigns win by reply rate, and how replies break down by type. Open rates are left out on purpose.

## Do not contact

People and whole domains that must never be contacted. Unsubscribes are added automatically. Each entry is also blocked in Smartlead (emails are unsubscribed from every campaign; domains go on the block list). "Here only" means Smartlead had no matching lead yet; the CRM still skips it.

## Reply agent

How the AI writes for you:
- Tone, voice rules and words it must never use. Edit any of them; the next draft follows.
- The facts it may use (offer, pricing, proof, objections) live in your business profile. Change them by telling Claude in Claude Code.
- "Rebuild from my emails": share past emails or call notes with Claude and it rewrites the rules from how you really write.

## LinkedIn

Only shows if you said you do LinkedIn outreach. Connect HeyReach and its campaigns, stats and replies show here, with LinkedIn replies in your Inbox tagged LinkedIn; Claude drafts each answer and nothing sends until you click Send. Connect GojiBerry and its warm leads come into Leads, with its results on the LinkedIn page. You can also build your own AI SDR: name it, write how it acts, and it drafts LinkedIn messages for you to approve.

## Team

Settings > Team. On a Claude Team or Enterprise plan, share this CRM from its Share menu and teammates see the same inbox. You can turn on "Assign replies to teammates".

## Settings

- **Business & brand:** name, sender, call to action, booking link, logo, brand color.
- **General:** which page opens first.
- **Inbound:** turn the Inbound page on or off, and describe where those leads come from.
- **Pipeline:** stages, colors and chance of closing.
- **Connections:** Smartlead, InboxKit, email checker, calendar, your current CRM, data tools, HeyReach and GojiBerry.
- **Auto-send:** off by default. You can unlock it for one low-risk case: a polite one-line thank-you to a "not interested" reply, sent only while the CRM is open. Interested replies, questions, objections and complaints are never auto-sent. Auto-sent replies show as Sent in the Inbox.
- **Businesses:** edit a business, or add a new one.
- **Instructions:** this guide.

## Things you do in Claude Code, not here

Some jobs run in Claude Code, because they need research agents or private keys:
- "run my lead searches"
- "check my leads" (uses your email checker's key from your `.env` file)
- "sync my calendar", "sync LinkedIn", "sync inbound", "run my jobs"
- "set up manual sends"
- "give me my morning brief"
- "set up my new business"
- "change my voice rules" or anything else you want changed

## Links

Every link in this CRM opens in your default browser.

## If something looks wrong

Ask Claude in Claude Code: describe what you see. The AI behind this can make mistakes, so check drafts before you send them.
