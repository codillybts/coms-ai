# COMS AI

**AI Cold Outbound Management System.** A Claude Code skill that sets up and runs cold outbound for your business: sending infrastructure, a reply agent that writes in your voice, lead-finding agents, and your own private CRM and master inbox, all inside Claude.

Built for solo founders and people running marketing or outbound at small businesses. Agencies can use it too: it supports more than one business.

![COMS AI preview](docs/preview.png)

## What it does

1. **Asks before it builds.** A setup interview (about 55 questions in short rounds, with clickable options) covers your business, ideal customer, offer, proof, tools, goals, voice, compliance, team, and branding. You can skip any question or say "decide for me". Nothing gets built until you confirm the summary.
2. **Plans your infrastructure.** Works out how many mailboxes and domains you need from your daily lead target, explains SPF, DKIM, and DMARC in plain words, and sets a safe warmup timeline.
3. **Connects Smartlead to Claude** through Smartlead's official MCP server.
4. **Learns your voice.** Upload past emails, call notes, or website copy. Agents study them and write voice rules. Or use the built-in best-practice rules.
5. **Builds your CRM.** A private claude.ai artifact with your name, logo, and colors. It starts empty.
6. **Finds leads.** Click "Find leads" in the CRM, then tell Claude to run your searches. Agents research public sources and any tools you connected. They only record emails that are actually published, with the source link. They never guess.
7. **Keeps you in control.** Every email needs your click to send. Nobody is contacted without your approval.

## The CRM

- **Business switcher** at the top. Add more businesses any time.
- **Home:** what needs you now.
- **Inbox:** every reply from every Smartlead mailbox, each with an AI draft. You click to send. Auto-send for low-risk replies can be unlocked in Settings. It is off by default.
- **Nudges:** follow-ups for interested leads who went quiet.
- **Compose:** one-off emails.
- **Leads:** add by hand, paste, upload, or drag and drop CSV or XLSX with column mapping. Plus "Find leads" agent mode.
- **First-email writer** with an approval queue.
- **Campaigns:** create and edit Smartlead campaigns, sequences, and schedules.
- **Mailboxes:** health and deliverability checks.
- **Pipeline:** deals by stage, with values.
- **Contacts and companies**, **Tasks and automations**, **Analytics and A/B insights**.
- **Do-not-contact list**, synced to Smartlead's global unsubscribe.
- **Reply agent settings:** your voice rules.
- **Calendar:** connect the calendar you use; booked meetings move leads forward on their own.
- **Inbound (optional):** a section for people who contact you first, only if you turn it on.
- **Your current CRM (optional):** keep HubSpot, Pipedrive or another CRM in sync, set up with Claude.
- **Teams:** on a Claude Team or Enterprise plan, share the CRM with teammates.
- **Map View:** a live map of your territory. Click a state, then a county, then any lead.
- **LinkedIn:** only if you do LinkedIn outreach. Connect HeyReach (campaigns and replies show in the CRM) or GojiBerry, or build your own AI SDR.
- **Settings:** branding (logo and brand color), stages, tags, auto-send, and an Instructions tab.

## Requirements

- **Claude Code.** Built mainly for the Code tab in the Claude desktop app. The command-line version works too.
- **A claude.ai plan that includes artifacts** and custom connectors.
- **A Smartlead account** for sending and the master inbox. (Other sending tools can be used for parts of the system, but the inbox, campaign, and mailbox screens need Smartlead.)
- Separate domains and mailboxes for cold email. The skill helps you plan and set these up.

## Install

Pick one option. Both put the skill in `~/.claude/skills/coms-ai`.

### Option A: download the zip

1. Download `coms-ai.zip` from the Releases page of `https://github.com/codillybts/coms-ai`, or use the green "Code" button, then "Download ZIP".
2. Unzip it.
3. Rename the unzipped folder to `coms-ai` if it has a different name.
4. Move it to `~/.claude/skills/coms-ai`. (On a Mac, in Finder press Cmd+Shift+G and type `~/.claude/skills`. Create the `skills` folder if it does not exist.)
5. Check that `~/.claude/skills/coms-ai/SKILL.md` exists.

### Option B: git clone

```bash
git clone https://github.com/codillybts/coms-ai ~/.claude/skills/coms-ai
```

COMS AI never connects to GitHub and never checks for updates. Once it's in your skills folder, it's just files, and your copy keeps working as is.

## Start

1. Make a folder for your outbound work, for example `~/Documents/COMS AI`.
2. Open Claude Code in that folder.
3. Say: **set up COMS AI**

Claude walks you through everything from there. Your answers are saved in `coms-profile.json` in that folder. API keys go in a local `.env` file, never in the CRM or the profile.

## After setup

In Claude Code, you can say things like:
- "run my lead searches"
- "verify my approved leads"
- "give me my morning brief"
- "add a business"
- "change my voice rules"

If you want anything changed, just ask. Whatever you want changed gets changed.

## What is in this repo

```
SKILL.md                 The skill: intro, phases, hard rules
references/              Detailed steps Claude reads when needed
  intake-interview.md    Every setup question and the profile schema
  infrastructure.md      Domains, mailboxes, warmup, sizing calculator
  connect-smartlead.md   Adding the Smartlead MCP connector
  voice-and-reply-agent.md  Voice rules, default writing rules, reply agent
  build-the-crm.md       How the CRM is published and seeded
  crm-guide.md           How the CRM works (also the in-app Instructions)
  lead-sourcing.md       Lead imports and the lead-finding agents
  map-view.md            Territory questions and how Map View works
  extras.md              Calendar, inbound, current CRM, team
  linkedin.md            LinkedIn: HeyReach, GojiBerry, own AI SDR
  compliance.md          Plain-language overview of cold email laws
  operations.md          Daily routine, scheduled tasks, troubleshooting
assets/
  app.html               The CRM template
  profile.schema.json    Schema for coms-profile.json
```

## Affiliate disclosure

COMS AI may recommend tools during setup using affiliate links: Smartlead, InboxKit, HeyReach, and GojiBerry (HeyReach and GojiBerry only if you do LinkedIn outreach). If you sign up through these links, the project may earn a commission at no extra cost to you. Every recommendation comes with an "I already have my own / I use a different service" option. By downloading and using this skill, you agree to these terms.

## No liability

The AI behind this skill can make mistakes. Check every email and reply before you send it. By downloading and using this skill, you accept that its authors and contributors claim no liability for anything that results from using it with Claude or any other AI tool. You are responsible for following email, privacy, and anti-spam laws where you and your prospects are, and the terms of every tool you use. Nothing in this skill is legal advice. See [DISCLAIMER.md](DISCLAIMER.md).

## License

MIT. See [LICENSE](LICENSE).

## Credits

Map View uses map data from [US Atlas](https://github.com/topojson/us-atlas) (ISC license, based on US Census Bureau boundaries) and [World Atlas](https://github.com/topojson/world-atlas) (ISC license, based on Natural Earth), drawn with [d3](https://d3js.org) 7.9.0 (ISC) and [topojson](https://github.com/topojson/topojson) 3.0.2 (ISC). Excel imports use [SheetJS](https://sheetjs.com) 0.18.5 (Apache 2.0). All three are bundled in `assets/vendor/` so nothing is downloaded at run time.
