# LinkedIn

Read this only if the user said they do LinkedIn outreach (`channels.linkedin` is true). If they said no, the CRM hides the LinkedIn section and none of this applies. They can turn it on later by saying "set up LinkedIn".

The CRM section is called **LinkedIn**. It can hold any mix of three things: HeyReach, GojiBerry, and the user's own AI SDR.

## The choices

Ask with AskUserQuestion (multiSelect true):

> "For LinkedIn, what do you want to use? Pick any."
> 1. **HeyReach (recommended, affiliate link).** Runs LinkedIn campaigns across one or many LinkedIn accounts, with one inbox for replies. Connects to the CRM.
> 2. **GojiBerry (affiliate link).** An AI SDR that watches LinkedIn for buying signals and writes the first message. Connects to the CRM.
> 3. **My own AI SDR inside the CRM.** You name it and write how it acts. It drafts LinkedIn messages and you send them yourself.
> 4. **No LinkedIn help for now.**

Save the picks to `channels.linkedin_tools` (any of `heyreach`, `gojiberry`, `own_persona`). If they already use another LinkedIn tool, add `manual` and record the tool name in `tools.linkedin_tools`.

## A word on LinkedIn's rules

Say this once, plainly: "LinkedIn limits automation in its terms. Any tool you use on LinkedIn is your choice and your responsibility. Keep volumes modest and messages personal." COMS AI itself never logs in to LinkedIn and never scrapes it. Messages go out only through a tool the user connected (HeyReach, GojiBerry) or by the user's own hand.

## HeyReach

HeyReach runs LinkedIn outreach across one or many LinkedIn sender accounts and collects every reply in one inbox. Link: https://heyreach.io/?via=dilly (affiliate link). Give it as a plain link so it opens in the default browser.

### Set it up

HeyReach's own onboarding is the source of truth. If a screen looks different, follow theirs.

1. **Create an account** through the link above, and connect the user's LinkedIn account (or several) inside HeyReach.
2. **Describe who to target.** Claude writes this from the ICP in the profile: titles, industries, company size, location, buying triggers, and disqualifiers, as plain sentences or a lead list they can load into HeyReach.
3. **Write the messages.** Claude drafts the connection note and the follow-up messages from the voice rules. LinkedIn messages are shorter than email, about 30 to 60 words, with one easy question and no hard pitch in the connection note.
4. **Set daily limits.** Start low per sender and raise slowly.

### Connect it to the CRM

HeyReach has an official MCP server, so it connects to Claude the same way Smartlead does.

1. In HeyReach, open **Integrations**, pick **HeyReach MCP Server**, generate an MCP key, and copy the connection URL.
2. On claude.ai, open **Settings > Connectors > Add custom connector**. Name it exactly **HeyReach** (the CRM looks it up by that name) and paste the URL. Treat that URL as a secret.
3. If Claude is asked for a workspace API key, the user copies it from their HeyReach workspace settings. It never goes into the CRM page or the profile.
4. Tell Claude "connect HeyReach to my CRM". Claude lists the connector's tools, makes one safe read (for example, listing campaigns) to confirm it works, and then republishes the CRM with HeyReach added to its `mcp` capability, naming only the tools the page uses.

Source: https://help.heyreach.io/en/articles/12123398-how-to-integrate-heyreach-mcp-with-claude

What the CRM shows once connected:
- On the LinkedIn page: campaigns, connection requests sent, acceptance rate, and replies.
- In the Inbox: LinkedIn replies next to email replies, tagged **LinkedIn**. Claude drafts a reply in the LinkedIn length and voice. The user clicks Send and confirms, and the message goes out through HeyReach. The same approval rule as email applies.
- Pipeline, tasks, and do-not-contact work the same as for email.

Read the tool names and input shapes from the connector itself (describe the tools, or make one safe read). Never guess them, and never run a write just to learn what it returns.

Save `linkedin.heyreach_via_recommended_link` and `linkedin.heyreach_connected`.

## GojiBerry

GojiBerry is an AI SDR for LinkedIn. It watches for buying signals (job changes, posts, engagement), finds and scores warm leads, and writes the first message. Link: https://gojiberry.ai/?ref=dilly (affiliate link). Claude access through its MCP is part of GojiBerry's Pro plan; check their pricing page for the current plan.

### Set it up

1. **Create an account** through the link and connect LinkedIn the way GojiBerry asks.
2. **Describe who to target** from the ICP. Claude writes it for them.
3. **Write the messaging instructions** from the voice rules: tone, 30 to 60 words, the call to action, banned words, and "never claim anything not true".
4. **Set daily limits.** Start low.

### Connect it to the CRM

GojiBerry has its own Claude MCP connection ("one URL, one setting").

1. In GojiBerry, open its Claude MCP setup and copy the connection URL. Their guide: https://gojiberry.ai/how-to-do-outreach-claude-linkedin-mcp
2. On claude.ai, open **Settings > Connectors > Add custom connector**. Name it exactly **GojiBerry** (the CRM looks it up by that name) and paste the URL. Treat that URL as a secret.
3. Tell Claude "connect GojiBerry to my CRM". Claude lists the connector's tools, makes one safe read to confirm it works, and republishes the CRM with GojiBerry added to its `mcp` capability, naming only the tools the page uses.

What the CRM shows once connected:
- Warm leads GojiBerry finds come into Leads, tagged `linkedin`, with the signal that made them warm. The same review rules apply before anyone is contacted by email.
- The LinkedIn page shows GojiBerry's results (leads found, messages sent, reply rate).
- The user can ask Claude about GojiBerry's pipeline at any time ("how are my LinkedIn replies this week?").

Read tool names and input shapes from the connector itself. Never guess them, and never run a write just to learn what it returns.

Save `linkedin.gojiberry_via_recommended_link` and `linkedin.gojiberry_connected`.

## Own AI SDR in the CRM

The user creates a persona that lives in the CRM's LinkedIn section.

Ask:
1. **Name** for the persona (for example "Riley"). This is an internal name for the assistant, not a fake person shown to prospects. Messages go out from the user's own LinkedIn account, or through HeyReach if connected, under the user's own name.
2. **Instructions:** how it should act. Claude offers a starting draft from the voice rules and ICP, for example:
   ```
   You draft LinkedIn messages for Sam Rivera, founder of Acme Desk.
   Keep messages under 60 words. Friendly and direct. No pitch in the
   connection note. Mention one real thing from their profile or company.
   Ask one easy question. Never claim results we don't have.
   If they ask whether this is AI, say an assistant helps draft and Sam
   reads and sends every message.
   ```
3. **Daily targets:** connection requests and messages per day.

Save to `linkedin.persona` and `linkedin.daily_*_limit`.

How it works in the CRM:
- If HeyReach is connected, the persona's drafts can be sent through HeyReach after the user approves each one.
- Without HeyReach, the user clicks "Copy", then "Open profile" (a normal link that opens LinkedIn in their default browser), and sends it themselves. They mark what happened (sent, accepted, replied) and paste replies in for a drafted answer.
- Nothing is sent without the user's click.

## No LinkedIn for now

Leave `channels.linkedin_tools` empty. The LinkedIn section stays hidden until they say "set up LinkedIn".
