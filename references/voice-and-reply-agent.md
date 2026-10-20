# Voice and reply agent

Phase 4, and any time Claude writes an email, follow-up, or reply. The reply agent drafts. A human approves. That is the default, always.

## Part 1: collect voice material

Ask for any of these. More is better, but 10 to 20 good emails is plenty.

| Material | How to get it |
|---|---|
| Sent emails from Gmail | Go to takeout.google.com. Deselect all, select only Mail. Click "All Mail data included" and pick only the "Sent" label. Export, download, and unzip. You get an `.mbox` file |
| Sent emails from Outlook | File, Open and Export, Import/Export, Export to a file. Choose a .pst file (or .csv if offered) and pick the Sent Items folder. If the .pst is hard to work with, copy 10 to 20 good emails into a plain text file instead |
| Campaigns from a sending tool | Most tools can export campaigns, sequences, or sent messages as CSV. Export the ones that got the best replies |
| Sales calls | Transcripts from your call recorder or meeting tool (.txt, .vtt, .srt, .docx, or .pdf) |
| Website and sales copy | The URL, or a PDF of a deck or one-pager |
| Emails you liked from others | Paste them in chat |

Tell the user:
- Drop files into `voice-samples/` in the working folder, or drag them into the chat.
- These files stay on your computer. Only the finished rules go into the CRM.
- Large `.mbox` files are fine. Claude will sample from them.

If they have nothing, skip to Part 3 and use the default rules.

## Part 2: study the material with agents

Spawn agents in parallel (one message, several Agent calls). Each gets the file paths and one job:

1. **Structure agent.** Measure: typical word count of first emails and replies, paragraph count, greeting style, sign-off, how the ask is phrased, use of questions, use of links.
2. **Language agent.** Find: tone, formality, words and phrases the user repeats, words they never use, how they talk about the product, how they open a reply.
3. **Sales agent** (only if there are call transcripts or sales copy). Pull out: the problems customers describe in their own words, the objections that come up, and how the user answers each one. Only record answers the user actually gave.

Rules for the agents:
- Only report patterns that appear in the material. Quote short examples.
- Strip names, emails, and phone numbers of other people from quotes.
- Return short findings, not a report.

Then combine the findings into voice rules: short, imperative lines, each with a tiny example if helpful. Aim for 10 to 20 rules. Add any objections and answers found to `offer.objections` (mark them "from calls").

## Part 3: default writing rules

These are a common best-practice set for cold email. The user can keep them, edit them, or replace them with rules built from their own voice. When the user has their own rules, theirs win wherever they conflict.

### All emails
1. Short, plain, and specific. Write like a person, not a brochure.
2. No over-explaining. Cut any sentence the reader would not miss.
3. One idea and one call to action per email.
4. Never claim anything that is not in the profile: no made-up results, customers, numbers, or features.
5. Personalization must come from a real, checkable fact about the prospect (their website, a public post, a job listing, news). Never invent it. If there is no real fact, write a clean version without personalization.
6. No hype words, no stacks of exclamation marks, no fake familiarity ("Hey buddy", "Hope you're crushing it").
7. No "I hope this email finds you well" or similar filler.
8. Plain text. No images. No bold, colors, or fancy formatting.

### First email
9. About 50 to 120 words. Shorter is usually better.
10. Subject: 2 to 5 words, mostly lowercase, plain. No clickbait. Never "Re:" or "Fwd:" on a first email.
11. No links and no images in the first email. The calendar link comes later, once they show interest.
12. Shape: a relevant opener (the real fact), the problem in their words, what you do about it in one line, proof if you have it, one simple ask.
13. The ask is easy to answer, for example "Worth a look?" or "Open to a quick chat next week?"

### Follow-ups
14. Each follow-up stands alone. Someone who never read the first email should still understand it.
15. Each one adds something new: a different angle, a short result, a useful idea.
16. Never guilt-trip. No "just bumping this", "did you see my last email", "circling back", or "I haven't heard from you".
17. Shorter than the first email. The last one can be a polite close: "I'll leave it here. If timing changes, just reply."

### Replies
18. Answer the question they asked first. Then move to the next step.
19. Match their length and tone. A two-line reply gets a short answer.
20. If they want a call, offer the calendar link or two specific times.
21. If they raise an objection, use the answer from the profile. If there is no answer on file, flag it for the human instead of making one up.
22. If they ask whether they are talking to an AI, answer honestly. For example: "Good question. I use an AI assistant to help draft replies, and I read and approve every one."
23. If they ask to stop, stop. No reply is needed. Add them to the do-not-contact list right away.

### Default banned words and phrases

The user can add or remove any of these.

> I hope this email finds you well, just checking in, just bumping this, circling back, touching base, following up on my last email, game-changer, revolutionary, cutting-edge, best-in-class, world-class, synergy, leverage (as a verb), unlock, skyrocket, 10x, supercharge, guaranteed results, no-brainer, act now, limited time, don't miss out, Dear Sir or Madam, To whom it may concern

Save the final rules to `voice.rules` in the profile and to `voice-rules.md` in the working folder. The CRM gets a copy in its settings (see `build-the-crm.md`).

## Part 4: show the rules and let the user decide

Show the rules grouped as above, with what came from their material marked "(from your emails)". Ask:
- "Keep these (recommended)"
- "Edit some"
- "Use only the default best-practice rules"
- "Use only rules from my own emails"

Save the choice in `voice.rules_source`.

Then write one sample first email and one sample reply for a made-up prospect that fits the ICP, so they can see the rules in action. Ask if anything feels off. Fix the rules, not just the sample.

## How the reply agent works

The CRM shows every reply from every Smartlead mailbox. For each reply, it drafts a response using the voice rules, the profile, and the full thread.

### Reply categories

Each reply gets one category. The category decides what happens next.

| Category | What the agent does |
|---|---|
| Interested | Drafts a reply that moves to the call to action. Suggests moving the lead to "Interested" in the pipeline |
| Wants a meeting | Drafts a reply with the calendar link or two specific times. Suggests "Meeting booked" once confirmed |
| Question | Answers the question from the profile. If the answer is not in the profile, says so in a note and leaves a gap for the human |
| Objection | Drafts using the matching answer in `offer.objections`. If none matches, flags for the human |
| Not now / later | Drafts a short, polite reply and creates a follow-up task for the date they mentioned |
| Referral (wrong person, points to someone else) | Drafts a thank-you. Creates a new lead for the person named only if their email was actually given in the reply |
| Not interested | Drafts a short, polite close, or no reply if the user prefers. Stops the sequence for that lead |
| Opt-out / unsubscribe | No reply. Adds to the do-not-contact list at once and syncs to Smartlead's global unsubscribe |
| Out of office | No reply. Pauses or delays the sequence until their return date if one is given |
| Bounce or auto-reply | No reply. Marks the lead and, for hard bounces, the email as invalid |
| Upset or complaint | No draft. Flags for the human with high priority. Adds to do-not-contact if they ask to stop |
| Asks if it is AI | Drafts an honest answer (rule 22). Never auto-sent |

### Approval

- Every draft waits in the Inbox for a human to read, edit, and click Send. This is the default.
- Edits the human makes are saved. Over time, Claude can review them ("look at my edits and update the voice rules") and suggest rule changes. The user approves rule changes too.

### Auto-send (off by default)

The user can unlock auto-send in Settings for low-risk replies only. When they ask to turn it on, explain the risk in one line and confirm.

- Allowed category in this version: a polite one-line thank-you to a "not interested" reply (40 words or fewer).
- Never auto-sent: interested replies, questions, objections, pricing or contract questions, complaints, legal or privacy questions, "is this AI", and any thread where a human already replied by hand.
- Auto-send only runs while the CRM page is open. Scheduled runs in Claude Code never send. Auto-sent replies show as Sent in the Inbox.
- Turning it off takes effect at once.

## First-email writer and nudges

- **First-email writer:** for each approved lead, Claude writes a first email using the rules and one real fact about the prospect (with its source). Drafts wait in the approval queue. Approved emails go to a Smartlead campaign as personalized first steps, or the user approves a campaign-wide template with merge tags.
- **Nudges:** leads who replied with interest and then went quiet get a short, standalone nudge: the first after 3 days of quiet, the second after 7. Same approval rules. Never more than 2 nudges per lead.
