# Setup interview

The full question list for Phase 1. About 55 questions in 9 short rounds. Nothing gets built until the user confirms the summary at the end.

## How to run it

- One round at a time. Say "Round X of 8" (Round 0 is the start).
- Questions with options go through AskUserQuestion: up to 4 questions per call, 2 to 4 options each, recommended option first and labeled "(recommended)". The user can always type "Other".
- Questions marked **free** are asked in plain chat. Give an example answer.
- Every question accepts "skip" and "decide for me". "Decide for me" uses the default in the table. If the default says "infer", work it out from earlier answers and say what you chose.
- Save to `coms-profile.json` after each round. The profile path for each answer is in the "Saves to" column (paths are inside `businesses[n]` unless they start with `app.`, `operator.`, or `setup.`).
- Quick setup mode: ask only the questions marked with a star (*). Use defaults for the rest and list them in the final summary.
- Do not lecture. One short sentence of context before a question is enough when the question is not obvious.
- Example answers in this file use fictional businesses: Acme Desk (a B2B scheduling app) and Northwind Cleaning (a commercial cleaning company).

## Round 0: start (2 questions)

| ID | Question | Options | Default | Saves to |
|---|---|---|---|---|
| R0.1* | How do you want to do this? | Full setup (recommended) / Quick setup | Full setup | `setup.mode` |
| R0.2* | Who is this for? | My own business (recommended) / My clients (agency) / Both | My own business | `operator.uses_for` |

If they pick clients or both, say: "We'll set up one business now. You can add more any time by saying 'add a business'."

## Round 1: business basics (8 questions)

| ID | Question | Options | Default | Saves to |
|---|---|---|---|---|
| R1.1* | What is the business called? **free** | | none, required | `name`, `id` (slug) |
| R1.2* | What is the website? **free** | | none | `website` |
| R1.3* | In one or two sentences, what do you sell and to whom? **free** Example: "Acme Desk is a scheduling app that lets clinics fill last-minute cancellations." | | read the website and propose one | `basics.what_you_sell` |
| R1.4 | How do you charge? | Subscription / One-time project / Retainer / Custom quote | infer from website | `basics.pricing_model` |
| R1.5 | What is a typical deal worth? | Under 1,000 / 1,000 to 10,000 / 10,000 to 50,000 / Over 50,000 (any currency) | ask again later, mark skipped | `basics.avg_deal_size` |
| R1.6 | How long from first reply to closed deal, usually? | Under 2 weeks / 2 to 8 weeks / 2 to 6 months / Longer | 2 to 8 weeks | `basics.sales_cycle` |
| R1.7 | How does a deal usually close? | Call, then proposal (recommended for services) / Demo, then trial / They sign up on their own / Quote, then site visit | Call, then proposal | `basics.how_deals_close` |
| R1.8* | What should a cold email ask for? | A reply (recommended) / Book a call on my calendar / Sign up / Visit a link | A reply. Low friction gets the most responses. A calendar link goes in the second email, after interest | `basics.call_to_action.type` |

Follow-up if R1.8 is "book a call": ask for the calendar link (**free**), save to `basics.call_to_action.calendar_link` and `tools.calendar_link`.

## Round 2: ideal customer (11 questions)

Say first: "This decides who the agents look for and who never gets contacted. It also builds your Map View, a live map of every lead in your territory."

After R2.3c, read `references/map-view.md` and save `territory` in its format (state and county FIPS codes, plus a short label).

| ID | Question | Options | Default | Saves to |
|---|---|---|---|---|
| R2.1* | Which industries do you sell to? **free**, list. Example: "dental clinics, physiotherapy practices, veterinary clinics" | | infer from website and best customers | `icp.industries` |
| R2.2* | What size of company? | 1 to 10 people / 11 to 50 / 51 to 200 / 201 or more (they can pick several by typing) | 11 to 50 | `icp.company_size` |
| R2.3* | Where are they? **free**. Example: "US and Canada", "UK only", "Texas and Oklahoma" | | sender's own country | `icp.geographies` |
| R2.3a* | For your Map View: which country is your territory in? | United States (recommended if US) / Another country / Several countries | from R2.3 | `territory.country`, `territory.countries` |
| R2.3b* | (US only) How much of the US do you cover? | Whole US / Certain states (I'll list them) / One metro or a few counties | from R2.3 | `territory.level`, `territory.states` |
| R2.3c | (Only if 1 to 3 states, or a metro) Which counties? **free**. Example: "Denver metro" or "Travis and Williamson counties". Claude suggests the county list and the user confirms | | the whole state | `territory.counties` |
| R2.4* | Who do you email? Titles or roles. **free**. Example: "practice owner, office manager" | | owner or founder for under 50 people, department head above that | `icp.titles` |
| R2.5 | What tells you a company needs you right now? (buying triggers) **free**. Examples: "hiring a front-desk person", "just opened a second location", "bad reviews about wait times" | | infer 3 from the offer, mark as suggested | `icp.buying_triggers` |
| R2.6 | What rules a company out? **free**. Examples: "franchises", "under 3 staff", "already uses a competitor" | | none | `icp.disqualifiers` |
| R2.7 | Who must never be contacted? | Current customers (recommended) / Competitors / Partners and investors / Specific domains I'll list | Current customers and competitors | `icp.do_not_contact` |
| R2.8 | Do you have a customer list or exclusion list to upload? | Yes, I'll upload it / Not now | Not now | `icp.do_not_contact.domains`, `.emails` |

## Round 3: offer and proof (6 questions)

| ID | Question | Options | Default | Saves to |
|---|---|---|---|---|
| R3.1* | What problem do you solve, in the customer's words? **free**. Example: "Empty appointment slots cost us money every week." | | infer from website | `offer.core_problem` |
| R3.2 | What results can you point to? Numbers, case studies, testimonials. **free**. Example: "One clinic filled 40 more slots a month." | | none. Emails will not claim results | `offer.proof` |
| R3.3 | Can emails name those customers? | No, keep them anonymous (recommended unless you have permission) / Yes, I have permission | No | `offer.proof[].can_name_customer` |
| R3.4 | What makes you different from the usual options? **free**, up to 3 points | | infer from website, mark as suggested | `offer.differentiators` |
| R3.5* | What objections do you hear most, and how do you answer them? **free**. Example: "We already have a system. Answer: it works alongside it, setup takes a day." | | 3 common ones for the industry, marked as suggested, user must confirm before use | `offer.objections` |
| R3.6 | Do you offer anything that lowers the risk? | Free trial / Guarantee / Free audit or sample / Nothing like that | Nothing like that | `offer.risk_reversal` |

## Round 4: tools you already use (11 questions)

Say first: "Tell me what you already have. Nothing gets replaced unless you want it to be."

| ID | Question | Options | Default | Saves to |
|---|---|---|---|---|
| R4.1* | What do you send cold email with today? | Nothing yet (recommended: Smartlead) / Smartlead / Another sending tool / Gmail or Outlook directly | Nothing yet | `tools.sending_tool` |
| R4.2* | Do you have separate domains and mailboxes for cold email? | No, starting fresh / Yes, some / Yes, enough for my volume | No | `tools.existing_domains_count`, `tools.existing_mailboxes_count` |
| R4.3 | Where do you keep leads and deals today? | Spreadsheet / A CRM tool / Nowhere yet / Several places | Nowhere yet | `tools.current_crm` |
| R4.3a | (If R4.3 names a CRM) Want COMS AI to keep it in sync? | No, COMS AI is enough (recommended) / Yes, set it up with Claude | No | `integrations.crm.tool`, `integrations.crm.sync` |
| R4.4 | Where do your leads come from today? | Lists I buy or export / I research by hand / Referrals and inbound only / Nowhere yet | Nowhere yet | `tools.lead_sources` |
| R4.5 | Do you have any scraping, enrichment, or data tools connected to Claude or with an API key? **free**, list names only | | none | `tools.scraping_and_enrichment` |
| R4.6 | Do you use an email verification service? | Not yet / Yes (name it) | Not yet | `tools.verifier.name` |
| R4.7 | Any LinkedIn tools? | None / Sales Navigator / An automation tool / Other | None | `tools.linkedin_tools` |
| R4.8 | Do you use a calendar or booking tool for sales calls? | Google Calendar / Outlook / Calendly or Cal.com / None | None | `integrations.calendar.tool` |
| R4.9 | Do you get inbound leads, people who contact you first? | No (recommended: keep it off) / Yes | No | `inbound.on` |
| R4.9a | (If yes) Where do they come from, and how could Claude see them? **free**. Example: "the demo form on our site, it emails me" | | ask | `inbound.source`, `inbound.how` |

After R4.3a, R4.8 or R4.9, read `references/extras.md` for what each one turns on. Connections happen after the CRM is built, not during the interview.

If R4.5 or R4.6 names a tool with an API key, say: "Put the key in the `.env` file in this folder. Don't paste it in chat." Create `.env` with an empty line for it (for example `VERIFIER_API_KEY=`) and add `.env` to `.gitignore`. Save only the variable name.

## Round 5: numbers and goals (6 questions)

| ID | Question | Options | Default | Saves to |
|---|---|---|---|---|
| R5.1 | How many cold emails do you send per day now? | None / Under 50 / 50 to 200 / Over 200 | None | `numbers.current_daily_sends` |
| R5.2 | Reply rate, if you know it? | Don't know / Under 1% / 1 to 3% / Over 3% | Don't know | `numbers.reply_rate_pct` |
| R5.3 | Meetings or sales calls per month from outbound now? | None / 1 to 5 / 6 to 20 / Over 20 | None | `numbers.meetings_per_month_now` |
| R5.4* | How many meetings per month do you want from outbound? | 5 / 10 (recommended for a first goal) / 20 / 40 or more | 10 | `goals.meetings_per_month` |
| R5.5* | How many new leads should get a first email each day? | 25 (recommended to start) / 50 / 100 / 200 | 25 | `goals.daily_new_leads` |
| R5.6 | When do you want the first cold emails to go out? | As soon as it is safe (recommended, after warmup) / In about a month / A specific date | As soon as it is safe | `goals.start_by` |

After R5.5, do a quick sanity check and say it in one line. A rough guide: if about 1 in 100 to 1 in 200 contacted leads books a meeting, then 25 new leads a day (about 500 a month) is roughly 3 to 5 meetings. If the meetings goal needs much more than the lead target allows, say so and offer to raise the lead target. Label this a rough guide, not a promise.

## Round 6: channels and voice (8 questions)

| ID | Question | Options | Default | Saves to |
|---|---|---|---|---|
| R6.1* | Do you also do outreach on LinkedIn? | No, email only (recommended to start) / Yes / Maybe later | No | `channels.linkedin` |
| R6.2 | What tone fits you? | Friendly and direct (recommended) / Consultative / Formal / Playful | Friendly and direct | `voice.tone` |
| R6.3 | How long should first emails be? | Very short, under 80 words (recommended) / Short, 80 to 120 words / Medium, up to 150 | Very short | `voice.email_length` |
| R6.4* | Who sends the emails? Name and title. **free**. Example: "Sam Rivera, Founder" | | none, required | `voice.sender` |
| R6.5 | How do you sign off? | Just my first name (recommended) / "Thanks," and name / "Best," and name / Other | First name | `voice.sign_off` |
| R6.6 | Any words or phrases you never want used? **free** | | the default banned list in `voice-and-reply-agent.md` | `voice.banned_words` |
| R6.7 | Paste any cold emails you received or sent that you liked. **free** | | none | `voice.liked_examples` |
| R6.8* | Do you want to share past emails, call notes, or website copy so the reply agent writes like you? | Yes, I'll upload now (recommended) / Later / No, use the best-practice rules | Use the best-practice rules | `voice.rules_source` |

If R6.8 is yes, explain the exports (full steps in `voice-and-reply-agent.md`):
- Gmail: Google Takeout, select only Mail, pick the "Sent" label, download the .mbox file.
- Outlook: File, Open and Export, Import/Export, Export to a file (.pst or .csv). If .pst is hard to read, copy 10 to 20 good emails into a text file instead.
- Sending tool: export a campaign or sent-messages CSV.
- Calls: transcripts from your call recorder (.txt, .vtt, .docx, or .pdf).
- Website copy: just give the URL.

Tell them to drop files into `voice-samples/` or drag them into the chat. Studying them happens in Phase 4.

## Round 7: compliance and team (8 questions)

Say first: "Cold email laws depend on where you and your prospects are. This is not legal advice. I'll keep things on the safe side."

| ID | Question | Options | Default | Saves to |
|---|---|---|---|---|
| R7.1* | Which country is the business in? **free** | | infer from website | `compliance.sender_country` |
| R7.2* | Where are your prospects? | Same country only / US / UK or EU / Canada or Australia (they can type several) | same as R2.3 | `compliance.target_regions` |
| R7.3* | What physical mailing address should go in the email footer? **free**. A business address or a mailbox service address works | | ask again before the first campaign; campaigns cannot start without it | `compliance.mailing_address` |
| R7.4 | How should the opt-out line read? | "If this isn't relevant, reply 'no thanks' and I won't reach out again." (recommended) / An unsubscribe link / My own wording | the recommended line | `compliance.opt_out_wording` |
| R7.5 | Who approves replies before they go out? | Me (recommended) / Someone on my team / Different people per business | Me | `team.approver` |
| R7.6 | Will more than one person send under their own name? | No, just me / Yes (list names and titles) | No | `team.senders` |
| R7.7 | Auto-send for one low-risk case (a polite thank-you to "not interested") can be unlocked later in Settings. Start with it off? | Yes, keep it off (recommended) / I want to turn it on after setup | Off | `team.auto_send.enabled` (always false at setup) |
| R7.8 | Are you on a Claude Team or Enterprise plan with teammates who'll use this CRM? | No, just me (recommended) / Yes | No | `team.shared` |

After R7.2, read `references/compliance.md` and add one line per region to `compliance.laws_considered`. If the targets include the EU or the UK, mention that some EU countries require consent even for business emails, and suggest checking before targeting them.

## Round 8: branding and finish (5 questions)

Say first: "This is your own master inbox. Let's make it look like yours."

| ID | Question | Options | Default | Saves to |
|---|---|---|---|---|
| R8.1 | What should the app be called? | "{Business name} Outbound" (recommended) / COMS AI / Something else | "{Business name} Outbound" | `app.display_name` |
| R8.2 | Upload a logo? | Yes, I'll drop it in the chat / No, use initials | Use initials | `app.logo_file` |
| R8.3 | Brand color? | Pull it from my website or logo (recommended) / I'll give a hex code / Use the default | Pull it from the website or logo | `app.brand_colors` |
| R8.4 | Will you add more businesses later? | Maybe / Yes, soon / No | Maybe. The business switcher is always there either way | (no field, just sets expectations) |
| R8.5 | Do you want a short morning brief each weekday? | Yes, at 8:00 (recommended) / Yes, another time / No | No. It can be added any time | `routine.morning_brief`, `routine.morning_brief_time` |

## After the last round

1. Write the complete profile. Fill `created_at`, `updated_at`, `operator.timezone` (ask if unclear), `active_business_id`.
2. Show the summary (about 15 lines):

```
Business:      Acme Desk (acmedesk.example), scheduling app for clinics
Offer:         Fill last-minute cancellations automatically
Ideal customer: Dental and physio clinics, 11-50 staff, US. Owners and office managers
Ask:           A reply ("Worth a quick look?")
Volume:        25 new leads a day, goal 10 meetings a month
Channels:      Email only
Voice:         Friendly and direct, under 80 words, signs off "Sam"
Sender:        Sam Rivera, Founder
Compliance:    US sender, US prospects (CAN-SPAM). Footer address: on file
Approvals:     You approve every reply. Auto-send off
App:           "Acme Desk Outbound", logo uploaded
Skipped:       Deal size, reply rate
Defaults used: Buying triggers (3 suggested), objections (3 suggested)
```

3. Ask "Does this look right?" with "Yes, build it" and "Change something".
4. On yes: set `setup.profile_confirmed` to true, add 1 to `setup.completed_phases`, move to Phase 2.

## Adding another business later

Run rounds 1 to 7 for the new business only. Skip Round 0 and the app-wide questions in Round 8 (app name). Ask R8.2 and R8.3 again as "logo and color for this business" and save them to the business (`logo_asset_url`, `accent_color`). Append to `businesses`. Then follow "Add a business" in `operations.md`.

## Profile schema

The same schema is in `assets/profile.schema.json`. Validate against it before saving. Never store secrets in the profile: only environment variable names.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "$id": "https://coms-ai.local/profile.schema.json",
  "title": "COMS AI profile (coms-profile.json)",
  "description": "Everything the setup interview collects. Lives in the user's working folder. Never store secrets here: API keys and connector URLs go in .env or in claude.ai connector settings, and this file only names them.",
  "type": "object",
  "required": ["schema_version", "app", "businesses"],
  "additionalProperties": false,
  "properties": {
    "schema_version": { "type": "integer", "const": 1 },
    "created_at": { "type": "string", "format": "date-time" },
    "updated_at": { "type": "string", "format": "date-time" },
    "setup": {
      "type": "object",
      "additionalProperties": false,
      "properties": {
        "mode": { "enum": ["full", "quick"], "default": "full" },
        "current_phase": { "type": "integer", "minimum": 0, "maximum": 7 },
        "completed_phases": { "type": "array", "items": { "type": "integer", "minimum": 0, "maximum": 7 } },
        "skipped_questions": { "type": "array", "items": { "type": "string" }, "description": "Question ids the user skipped, so Claude can come back to them." },
        "defaulted_questions": { "type": "array", "items": { "type": "string" }, "description": "Question ids where the user chose 'decide for me'." },
        "profile_confirmed": { "type": "boolean", "default": false }
      }
    },
    "operator": {
      "type": "object",
      "description": "The person running COMS AI.",
      "additionalProperties": false,
      "properties": {
        "role": { "enum": ["solo_founder", "marketing_lead", "sales_lead", "agency", "other"] },
        "uses_for": { "enum": ["own_business", "clients", "both"], "default": "own_business" },
        "country": { "type": "string" },
        "timezone": { "type": "string", "description": "IANA timezone, for example America/Chicago." },
        "working_folder": { "type": "string", "description": "Absolute path of the folder that holds this file, .env and voice samples." }
      }
    },
    "app": {
      "type": "object",
      "description": "How the CRM looks. This becomes the user's own master inbox.",
      "required": ["display_name"],
      "additionalProperties": false,
      "properties": {
        "display_name": { "type": "string", "default": "COMS AI" },
        "logo_file": { "type": "string", "description": "Local path of the logo the user uploaded, if any." },
        "logo_asset_url": { "type": "string", "description": "URL returned after uploading the logo to the CRM's asset store." },
        "brand_colors": {
          "type": "array",
          "items": { "type": "string", "pattern": "^#[0-9A-Fa-f]{6}$" },
          "maxItems": 3,
          "description": "First color is the main accent."
        }
      }
    },
    "crm": {
      "type": "object",
      "additionalProperties": false,
      "properties": {
        "artifact_url": { "type": "string", "description": "claude.ai artifact URL of the published CRM." },
        "connector_name": { "type": "string", "default": "Smartlead" },
        "template_version": { "type": "string" },
        "published_at": { "type": "string", "format": "date-time" }
      }
    },
    "secrets": {
      "type": "object",
      "description": "Names only. Values live in .env or in claude.ai connector settings.",
      "additionalProperties": false,
      "properties": {
        "env_file": { "type": "string", "default": ".env" },
        "keys": {
          "type": "array",
          "items": {
            "type": "object",
            "required": ["name", "purpose"],
            "additionalProperties": false,
            "properties": {
              "name": { "type": "string", "pattern": "^[A-Z][A-Z0-9_]*$", "description": "Environment variable name, for example VERIFIER_API_KEY." },
              "purpose": { "type": "string" }
            }
          }
        }
      }
    },
    "businesses": {
      "type": "array",
      "minItems": 1,
      "items": { "$ref": "#/$defs/business" }
    },
    "active_business_id": { "type": "string" }
  },
  "$defs": {
    "money": {
      "type": "object",
      "additionalProperties": false,
      "properties": {
        "amount": { "type": "number", "minimum": 0 },
        "currency": { "type": "string", "default": "USD" }
      }
    },
    "string_list": { "type": "array", "items": { "type": "string" } },
    "business": {
      "type": "object",
      "required": ["id", "name"],
      "additionalProperties": false,
      "properties": {
        "id": { "type": "string", "pattern": "^[a-z0-9-]+$", "description": "Short slug, for example acme-desk." },
        "name": { "type": "string" },
        "website": { "type": "string" },
        "logo_asset_url": { "type": "string" },
        "accent_color": { "type": "string", "pattern": "^#[0-9A-Fa-f]{6}$" },

        "basics": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "what_you_sell": { "type": "string", "description": "One or two plain sentences." },
            "pricing_model": { "enum": ["one_time", "subscription", "retainer", "usage_based", "custom_quote", "other"] },
            "price_range": { "type": "string" },
            "avg_deal_size": { "$ref": "#/$defs/money" },
            "sales_cycle": { "enum": ["same_day", "under_2_weeks", "2_to_8_weeks", "2_to_6_months", "over_6_months"] },
            "how_deals_close": { "enum": ["call_then_proposal", "demo_then_trial", "self_serve_signup", "quote_then_site_visit", "other"] },
            "how_deals_close_notes": { "type": "string" },
            "call_to_action": {
              "type": "object",
              "additionalProperties": false,
              "properties": {
                "type": { "enum": ["book_call", "reply", "sign_up", "link", "other"], "default": "reply" },
                "wording": { "type": "string", "description": "How the ask is phrased, for example 'Worth a quick chat?'" },
                "calendar_link": { "type": "string" },
                "link_url": { "type": "string" }
              }
            }
          }
        },

        "icp": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "industries": { "$ref": "#/$defs/string_list" },
            "company_size": {
              "type": "object",
              "additionalProperties": false,
              "properties": {
                "employees_min": { "type": "integer", "minimum": 0 },
                "employees_max": { "type": "integer", "minimum": 0 },
                "revenue_range": { "type": "string" }
              }
            },
            "geographies": { "$ref": "#/$defs/string_list" },
            "territory": {
              "type": "object",
              "additionalProperties": false,
              "properties": {
                "label": { "type": "string" },
                "country": { "type": "string" },
                "level": { "enum": ["us", "states", "counties", "countries"] },
                "states": { "type": "array", "items": { "type": "string", "pattern": "^[0-9]{2}$" } },
                "counties": { "type": "array", "items": { "type": "string", "pattern": "^[0-9]{5}$" } },
                "countries": { "$ref": "#/$defs/string_list" }
              }
            },
            "titles": { "$ref": "#/$defs/string_list" },
            "seniority": { "$ref": "#/$defs/string_list" },
            "buying_triggers": { "$ref": "#/$defs/string_list" },
            "disqualifiers": { "$ref": "#/$defs/string_list" },
            "do_not_contact": {
              "type": "object",
              "additionalProperties": false,
              "properties": {
                "domains": { "$ref": "#/$defs/string_list" },
                "companies": { "$ref": "#/$defs/string_list" },
                "emails": { "$ref": "#/$defs/string_list" },
                "exclude_current_customers": { "type": "boolean", "default": true },
                "exclude_competitors": { "type": "boolean", "default": true },
                "notes": { "type": "string" }
              }
            }
          }
        },

        "offer": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "core_problem": { "type": "string" },
            "core_offer": { "type": "string" },
            "proof": {
              "type": "array",
              "items": {
                "type": "object",
                "additionalProperties": false,
                "properties": {
                  "type": { "enum": ["case_study", "result", "testimonial", "customer_logo", "other"] },
                  "summary": { "type": "string" },
                  "can_name_customer": { "type": "boolean", "default": false },
                  "source": { "type": "string" }
                }
              }
            },
            "differentiators": { "$ref": "#/$defs/string_list" },
            "objections": {
              "type": "array",
              "items": {
                "type": "object",
                "additionalProperties": false,
                "properties": {
                  "objection": { "type": "string" },
                  "answer": { "type": "string" }
                }
              }
            },
            "risk_reversal": { "type": "string" },
            "free_resource": { "type": "string" }
          }
        },

        "integrations": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "calendar": { "type": "object", "additionalProperties": false, "properties": { "tool": { "type": "string" }, "connected": { "type": "boolean", "default": false } } },
            "crm": { "type": "object", "additionalProperties": false, "properties": { "tool": { "type": "string" }, "sync": { "$ref": "#/$defs/string_list" }, "connected": { "type": "boolean", "default": false } } }
          }
        },

        "inbound": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "on": { "type": "boolean", "default": false },
            "source": { "type": "string" },
            "how": { "type": "string" }
          }
        },

        "tools": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "sending_tool": { "type": "string", "default": "Smartlead" },
            "domains_and_mailboxes_provider": { "type": "string" },
            "existing_domains_count": { "type": "integer", "minimum": 0 },
            "existing_mailboxes_count": { "type": "integer", "minimum": 0 },
            "current_crm": { "type": "string" },
            "lead_sources": { "$ref": "#/$defs/string_list" },
            "scraping_and_enrichment": { "$ref": "#/$defs/string_list" },
            "verifier": {
              "type": "object",
              "additionalProperties": false,
              "properties": {
                "name": { "type": "string" },
                "env_var": { "type": "string", "pattern": "^[A-Z][A-Z0-9_]*$" }
              }
            },
            "calendar_link": { "type": "string" },
            "linkedin_tools": { "$ref": "#/$defs/string_list" }
          }
        },

        "numbers": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "current_daily_sends": { "type": "integer", "minimum": 0 },
            "reply_rate_pct": { "type": "number", "minimum": 0, "maximum": 100 },
            "positive_reply_rate_pct": { "type": "number", "minimum": 0, "maximum": 100 },
            "meetings_per_month_now": { "type": "integer", "minimum": 0 }
          }
        },

        "goals": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "meetings_per_month": { "type": "integer", "minimum": 0 },
            "daily_new_leads": { "type": "integer", "minimum": 0, "default": 25 },
            "start_by": { "type": "string", "format": "date" }
          }
        },

        "channels": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "email": { "type": "boolean", "default": true },
            "linkedin": { "type": "boolean", "default": false },
            "linkedin_tools": { "type": "array", "items": { "enum": ["heyreach", "gojiberry", "own_persona", "manual"] }, "default": [] }
          }
        },

        "voice": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "tone": { "enum": ["friendly", "direct", "consultative", "playful", "formal"] },
            "formality": { "enum": ["casual", "neutral", "formal"], "default": "neutral" },
            "email_length": { "enum": ["very_short", "short", "medium"], "default": "short" },
            "greeting": { "type": "string" },
            "sign_off": { "type": "string" },
            "sender": {
              "type": "object",
              "additionalProperties": false,
              "properties": {
                "name": { "type": "string" },
                "title": { "type": "string" },
                "signature": { "type": "string" }
              }
            },
            "banned_words": { "$ref": "#/$defs/string_list" },
            "liked_examples": { "$ref": "#/$defs/string_list" },
            "samples_folder": { "type": "string", "default": "voice-samples" },
            "rules_source": { "enum": ["default_best_practice", "own_voice", "mixed"], "default": "default_best_practice" },
            "rules": { "$ref": "#/$defs/string_list" }
          }
        },

        "compliance": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "sender_country": { "type": "string" },
            "target_regions": { "$ref": "#/$defs/string_list" },
            "mailing_address": { "type": "string", "description": "The business's physical postal address for email footers." },
            "opt_out_wording": { "type": "string", "default": "If this isn't relevant, reply \"no thanks\" and I won't reach out again." },
            "laws_considered": { "$ref": "#/$defs/string_list" }
          }
        },

        "team": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "shared": { "type": "boolean", "default": false },
            "approver": { "type": "string", "description": "Who approves replies before they go out." },
            "senders": {
              "type": "array",
              "items": {
                "type": "object",
                "additionalProperties": false,
                "properties": {
                  "name": { "type": "string" },
                  "title": { "type": "string" },
                  "mailboxes": { "type": "integer", "minimum": 0 }
                }
              }
            },
            "auto_send": {
              "type": "object",
              "additionalProperties": false,
              "properties": {
                "enabled": { "type": "boolean", "default": false },
                "allowed_categories": { "$ref": "#/$defs/string_list" }
              }
            }
          }
        },

        "infrastructure": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "sending_provider": { "type": "string", "default": "Smartlead" },
            "sending_via_recommended_link": { "type": "boolean" },
            "domains_provider": { "type": "string", "default": "InboxKit" },
            "domains_via_recommended_link": { "type": "boolean" },
            "planned_domains": { "type": "integer", "minimum": 0 },
            "planned_mailboxes": { "type": "integer", "minimum": 0 },
            "sends_per_mailbox_per_day": { "type": "integer", "minimum": 1, "default": 25 },
            "warmup_start_date": { "type": "string", "format": "date" },
            "cold_send_start_date": { "type": "string", "format": "date" },
            "open_tracking": { "type": "boolean", "default": false },
            "link_tracking": { "type": "boolean", "default": false },
            "plain_text": { "type": "boolean", "default": true },
            "sequence_steps": { "type": "integer", "minimum": 1, "default": 3 },
            "follow_up_gaps_days": { "type": "array", "items": { "type": "integer", "minimum": 1 }, "default": [3, 5] }
          }
        },

        "linkedin": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "heyreach_via_recommended_link": { "type": "boolean" },
            "heyreach_connected": { "type": "boolean" },
            "gojiberry_via_recommended_link": { "type": "boolean" },
            "gojiberry_connected": { "type": "boolean" },
            "persona": {
              "type": "object",
              "additionalProperties": false,
              "properties": {
                "name": { "type": "string" },
                "instructions": { "type": "string" }
              }
            },
            "daily_connection_limit": { "type": "integer", "minimum": 0 },
            "daily_message_limit": { "type": "integer", "minimum": 0 }
          }
        },

        "pipeline": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "stages": {
              "$ref": "#/$defs/string_list",
              "default": ["New reply", "Interested", "Meeting booked", "Proposal sent", "Won", "Lost"]
            },
            "tags": { "$ref": "#/$defs/string_list" }
          }
        },

        "routine": {
          "type": "object",
          "additionalProperties": false,
          "properties": {
            "morning_brief": { "type": "boolean", "default": false },
            "morning_brief_time": { "type": "string", "pattern": "^[0-2][0-9]:[0-5][0-9]$" },
            "scheduled_lead_searches": { "type": "boolean", "default": false }
          }
        }
      }
    }
  }
}
```
