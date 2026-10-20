# Lead sourcing: getting leads into the CRM

Phase 6, and every time the user says "run my lead searches".

## Four ways leads come in

| Way | Where | Notes |
|---|---|---|
| Add one by hand | CRM, Leads, "Add lead" | For referrals and one-offs |
| Paste a list | CRM, Leads, "Import", then paste rows | Paste rows from a spreadsheet. The CRM maps columns |
| Upload a file | CRM, Leads, "Import" (click or drag and drop) | CSV or XLSX. The CRM shows a column-mapping step (first name, last name, email, company, website, title, and so on) before importing |
| Find leads (agent mode) | CRM, Leads, "Find leads", then Claude Code | Agents research the web and write leads back for review |

All leads from every source land with the status "New". Nothing is contacted until the user writes and approves first emails for them and adds them to a campaign.

## Find leads: how the flow works

1. **In the CRM**, the user clicks "Find leads" and fills a short form: which business, which segment (industries, titles, size, location, prefilled from the ICP), how many leads, and any notes ("only clinics with 2+ locations"). The CRM saves this as a search request with the status "queued" in its database.
2. **In Claude Code**, the user says "run my lead searches". (Or a scheduled task does it. See `operations.md`.)
3. Claude reads the queued searches, runs research agents, and writes the leads back to the CRM.
4. **In the CRM**, the user reviews the new leads, approves or rejects them, and approved leads move on to the first-email writer.

## Running queued searches

When the user says "run my lead searches":

1. Read `crm.artifact_url` from `coms-profile.json`. If it is missing, ask for the CRM link.
2. Use the ArtifactData tool to query `lead_searches` for `status` == "queued" (formats in `sync-jobs.md`). If there are none, say so and offer to create one from the ICP.
3. Tell the user what you found: "2 searches queued: 50 dental clinics in Texas, 30 physio practices in Ohio. Starting now."
4. Set each search to "running".
5. Load context: the business profile (ICP, disqualifiers, do-not-contact list), the existing leads (for duplicates), and the list of tools the user has connected (`tools.scraping_and_enrichment`, any MCP connectors in the session, any keys named in `.env`).
6. Split the work and spawn research agents in parallel (one message, several Agent calls). Split by geography, sub-industry, or source type. About 10 to 25 target leads per agent works well.
7. Collect results. Check every lead against the rules below. Remove duplicates and anyone on the do-not-contact list.
8. Write the leads to the CRM with ArtifactData batches (up to 50 per batch), in the `leads` format from `sync-jobs.md`, with `status: "New"` (or "No published email").
9. Update each search: status "done", counts (found, with published email, without email, skipped as duplicates or do-not-contact), and a 2 to 3 line summary of what worked.
10. Tell the user the totals and say: "They're in the CRM under Leads, filter New. Remove any that don't fit, then pick the rest and press Write first emails."

If something fails halfway, write what was found so far, set the search to "done" with a `summary` saying what is missing, and tell the user.

## What agents may use

Only these:
- **Public web research:** company websites, public directories, public registries (business registries, licensing boards, professional associations), news articles, press releases, job postings, conference speaker pages, podcasts, and public posts by the person.
- **Publicly available APIs** that do not need an account the user does not have.
- **Tools the user has connected:** MCP connectors in the session, or APIs whose keys are in `.env`. Use them as their own terms allow.

Never:
- Log in to anything, use someone else's account, or get behind a paywall.
- Bypass a CAPTCHA, a rate limit, or a block.
- Scrape sites whose terms forbid it, including LinkedIn. Reading a public page that a normal search returns is fine. Automated scraping is not.
- Buy or download lists the user did not ask for.

COMS AI does not recommend specific lead databases, scrapers, or data vendors. If the user asks, say any tool they connect will be used, and that its terms are theirs to check.

## The email policy (strict)

This rule has no exceptions.

- **Only record an email that is actually published and that the agent actually found.** Valid places: the company website (contact page, team page, footer), the person's own posts or profiles, a public filing, a public directory or association listing, a press release, or a tool the user connected that returns a sourced email.
- **Record the source URL for every email.** No URL, no email.
- **Never guess.** Never build an email from a name pattern (like first.last@company). Never "fill in" a likely address. Never hallucinate one.
- A generic published address (like `office@` or `hello@`) is allowed if it is the business's published contact address. Mark it as "generic".
- If no published email is found, save the lead anyway with `email` empty and the status **"no published email"**. The user can decide what to do: find it themselves, use another channel, or skip.

Before writing results, check every lead: if an email has no source URL, or the source page does not show that email, remove the email and mark the lead "no published email".

## Lead record

Write each lead in the `leads` format in `sync-jobs.md`. Map the agent's results like this: company name, website and domain to `company`, `website`, `domain`; person name and title to `name`, `title`; the published email and the page it was on to `email`, `emailSource` (a short label like "Company site · Team page") and `emailSourceUrl`; fit reasons to `signal` (one line) and `fit` (0 to 100); the fact and its source to `facts`; the published address to `location`. Add `via: "Lead search"` and `searchId`.

## Brief for each research agent

Give each agent this brief, filled in:

```
You are a lead research agent for {business name}.
Find up to {N} companies, and the right person at each, that match:
- Industries: {industries}
- Size: {size}
- Location: {area for this agent}
- Titles to find: {titles}
- Good signs (buying triggers): {triggers}
- Rule out: {disqualifiers}
- Never include these domains or companies: {do-not-contact list}
- Already have these domains (skip them): {existing domains}

Use only: public websites, public directories and registries, news, job posts,
public posts by the person, public APIs, and these connected tools: {tools}.
Do not log in anywhere, bypass blocks, or scrape LinkedIn.

Emails: only record an email you actually saw published, with the exact URL
where you saw it. Never guess or build an email from a name pattern.
If you find none, leave email empty.

For each lead return: company name, website, domain, industry, size estimate,
location, person name, title, email or empty, email source URL, email kind
(personal or generic), 1-3 fit reasons, one real fact with its source URL,
and any trigger with its source URL.
Return results as a JSON array. No commentary.
```

## After review: verification

**Never call an email Valid, Risky or Invalid unless a connected email checker actually tested it.** Finding an email published online means it exists on a page. It does not mean it delivers.

- If a checker is connected (its key in `.env`), offer: "Check the approved emails before they go into a campaign?" Run it from Claude Code and write the checker's result (valid, risky, invalid, unknown) back to each lead, with the checker's name and the date. Only valid leads go into campaigns by default. The key is never sent to the CRM page.
- If no checker is connected, leave the check field empty. The CRM shows no Valid, Risky or Invalid label and no "Needs check" status. Say once: "Want to connect an email checker? Otherwise Smartlead can check emails when leads go into a campaign." Never guess a result, and never mark an email valid because it looks right.

## Privacy

- Collect only business contact details needed for outreach. No personal addresses, phone numbers, family details, or anything sensitive.
- Keep the source URL so the user can always show where data came from (this matters under GDPR and similar laws, see `compliance.md`).
- If someone asks to be removed, delete their lead record and keep only their email (or domain) on the do-not-contact list, so they are never contacted again.
