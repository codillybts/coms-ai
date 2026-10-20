# Compliance: a plain-language overview

**This is not legal advice.** It is a short overview of common cold email rules so the user knows what to ask about. Laws change, and they differ by country and situation. For anything specific, the user should talk to a lawyer where they operate. The user is responsible for following the laws that apply to them and the terms of every tool they use.

Read this in Round 7 of the interview, when writing footers, when handling opt-outs, and whenever the user asks a legal question.

## What COMS AI does by default, everywhere

These defaults are on the safe side in most places:
- Every cold email identifies the real sender and business.
- Subject lines are honest. No fake "Re:" or "Fwd:".
- The footer includes the business's physical mailing address and a clear way to opt out.
- Opt-outs are honored at once, not days later.
- The do-not-contact list syncs to Smartlead's global unsubscribe, so an opted-out person is blocked in every campaign and every mailbox.
- Only business contact details are collected, each with the source URL it came from.
- Emails go to people in their work role, about something relevant to that role.
- Campaigns cannot start without a mailing address in the profile.

### Footer example

```
Sam Rivera
Founder, Acme Desk
123 Example Street, Suite 4, Springfield, ST 00000

If this isn't relevant, reply "no thanks" and I won't reach out again.
```

The user picks the opt-out wording in Round 7. A reply-based opt-out is common for one-to-one cold email. An unsubscribe link also works, but links in the first email can hurt deliverability.

## United States: CAN-SPAM

- Applies to commercial email, including business-to-business.
- Does not require consent before the first email. It is an opt-out law.
- Header and "from" information must be accurate.
- Subject lines must not be deceptive.
- The email must be identifiable as a commercial message.
- Include a valid physical postal address (a street address, a post office box, or a registered private mailbox).
- Give a clear way to opt out. Honor opt-outs within 10 business days, and keep the opt-out working for at least 30 days after sending. Do not ask for anything more than the opt-out request itself.
- Do not sell or share the addresses of people who opted out.
- The business is responsible even if someone else sends for it.
- Penalties are set per email and can be large (over 50,000 US dollars per email, adjusted each year).

## European Union: GDPR and ePrivacy rules

- A work email of a named person (like jane@company.example) is personal data under GDPR.
- You need a lawful basis to use it. For business outreach, many senders rely on **legitimate interest**. That means you should write down a short assessment: why you are contacting them, why it is necessary, and why it does not unfairly affect them.
- Be transparent: tell people who you are, where you got their details, and how to object. This should happen no later than the first email. A short line plus a link to a privacy notice is common.
- The right to object to direct marketing is absolute. If someone objects, stop and suppress them.
- Collect only what you need. Keep the source of every contact.
- **Email marketing rules differ by country.** The EU's ePrivacy rules are applied by each country. Some countries (Germany and Austria, for example) generally require prior consent even for business email. Others allow business email without consent if it is relevant to the person's role and they can opt out easily. Check each country before targeting it.

## United Kingdom: UK GDPR and PECR

- UK GDPR works like the EU GDPR above: legitimate interest is commonly used, with transparency and an easy way to object.
- PECR covers electronic marketing. In short:
  - **Corporate subscribers** (limited companies, LLPs, some partnerships, government bodies): you can email them without prior consent, but you must say who you are and give a valid way to opt out.
  - **Individual subscribers** (sole traders and some partnerships): you generally need consent, or a narrow exception for existing customers.
- Know which kind of business you are emailing. When in doubt, treat a small business as a sole trader.

## Canada: CASL

- Applies to commercial messages sent to people in Canada. It is stricter than CAN-SPAM.
- You need **consent**, either express or implied.
- One common form of implied consent: the person published their email address conspicuously (for example on their company website), did not say they do not want unsolicited messages, and your message is relevant to their business role.
- Every message must identify the sender, include a mailing address and one other contact method (phone, email, or website), and include an unsubscribe option.
- Honor unsubscribes within 10 business days.
- Keep records of how you got consent. COMS AI's `email_source_url` helps here.
- Penalties can be very large for businesses.

## Australia: Spam Act 2003

- You need **consent**, express or inferred.
- Inferred consent can come from conspicuous publication: the address is published, there is no statement refusing unsolicited messages, and your message relates to the person's role or business.
- Every message must identify the sender and include a working unsubscribe option.
- Honor unsubscribes within 5 business days.

## Other countries

Rules vary widely. If the user targets a country not listed here, tell them COMS AI's defaults are a reasonable starting point, and suggest they check local rules before sending.

## Opt-outs and the do-not-contact list

- An opt-out can come as a reply ("remove me", "stop", "not interested, don't email again"), an unsubscribe click, or a complaint.
- COMS AI treats all of them the same way: no more emails, added to the do-not-contact list, synced to Smartlead's global unsubscribe, and the lead's sequence stopped in every campaign.
- If someone asks to be removed for one business, ask the user whether to block them for all businesses. Default: all businesses.
- Never re-import someone who is on the do-not-contact list. Imports and agent results are checked against it.
- If someone asks you to delete their data, delete the lead record and keep only their email or domain on the do-not-contact list so they stay blocked.

## Platform rules

Large mailbox providers publish rules for senders: authenticate with SPF, DKIM, and DMARC, keep spam complaints very low, and make opting out easy. LinkedIn limits automation in its terms. Every tool (Smartlead, InboxKit, HeyReach, GojiBerry, any data tool) has its own terms. The user is responsible for following them.
