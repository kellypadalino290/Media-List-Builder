---
name: media-list-builder
description: Build tailored media lists for client or internal outreach, confirm reporter recommendations with the user, then enrich approved names with contact information. Use when Codex needs to identify reporters by topic, geography, tier, or outlet type; compile a structured spreadsheet; or gather reporter email and phone data using Cision first, then prior lists, Apollo, and web research.
---

# Media List Builder

## Overview

Build a prompt-specific reporter list, pause for approval, then enrich the approved list with contact details using the required source order. Treat prior media lists and default outlet lists as supporting context, not as the sole basis for recommendations.

For outlet defaults and selection heuristics, read [references/outlet-guidance.md](references/outlet-guidance.md) only when the prompt needs category-based starting points. For a compact intake and QA rubric, read [references/research-rubric.md](references/research-rubric.md) when helpful.

## Intake Rules

Read the user's prompt and identify:
- topic or issue area
- geography
- outlet preferences or exclusions
- media type preferences
- tier guidance such as beltway, top-tier, trade, local, or broadcast
- requested number of names

If the user does not specify how many names they need, ask before building the list.

If the prompt is vague, narrow the ambiguity with the smallest useful follow-up. Favor concrete questions such as count, geography, or whether they want beltway, trade, top-tier, or local names.

## Core Workflow

1. Build the recommendation list from prompt-specific research first.
2. In parallel, use internet research to verify current reporter roles, beats, and recent coverage while checking prior media lists for overlap, historical context, and reusable details.
3. Compile a draft list with names, outlets, titles, issues or topics covered, and a few relevant links.
4. Present the draft list to the user and wait for approval before collecting contact data.
5. After approval, enrich contact fields by running Cision, Apollo, and web research in parallel where the environment allows, while still honoring the source-priority order when deciding what data to keep.
6. Finalize the spreadsheet with clean, deduplicated output.

Never let a prior media list become the only recommendation source. Recommendations must directly fit the prompt.

## Recommendation Rules

- Prioritize reporters with relevant coverage from the last 6 to 12 months.
- Look further back when the beat is niche or recent coverage is thin.
- Prefer reporters actively covering the issue rather than people who touched it once.
- Unless the user asks otherwise, prefer reporters over editors, producers, or generic press inboxes.
- Include editors, producers, newsletters, or broadcast bookers only when the prompt suggests they are useful.
- Deduplicate carefully across outlet changes, old lists, and multiple source systems.
- Use the reporter's current outlet and title whenever they can be verified.
- Mark uncertain items clearly instead of presenting them as confirmed facts.

## Contact Enrichment Rules

Use sources for distinct purposes:
- prompt-specific web research: primary basis for recommendations
- existing Google Drive media lists: supporting context, overlap checks, historical contacts, and list structure examples
- Cision via `$cision-workflow`: primary contact lookup source after approval
- Apollo via `$apollo-workflow`: secondary contact lookup source after Cision and prior lists
- web via `$web-research-workflow`: final fallback for contact verification and public contact information

Use this contact lookup order exactly:
1. Cision via `$cision-workflow`
2. prior media lists
3. Apollo via `$apollo-workflow`
4. web via `$web-research-workflow`
5. `N/A`

If email or phone cannot be found after these steps, enter `N/A`.

Use the same fallback order for every approved name. Do not skip directly to Apollo or web unless the earlier step is unavailable or exhausted.

For performance, batch similar work together:
- verify recommendation fit for multiple names in parallel where possible
- check prior-list overlap for multiple approved names together
- run Cision, Apollo, and web lookups in parallel when practical
- use the source order to resolve conflicts or fill the final sheet: Cision first, then prior lists, then Apollo, then web, then `N/A`

Treat phone numbers as a required lookup field, not a bonus field:
- search each approved name for both email and phone in every contact source
- prefer person-specific direct, mobile, or office numbers over outlet switchboards
- include an outlet or newsroom general phone only if no person-specific number is available and the source makes clear it is the relevant outlet contact number
- if a phone is general rather than person-specific, mark that clearly in a notes column or adjacent phone-source field when the final format allows
- use `N/A` for phone only after Cision, prior lists, Apollo, and public web have all been checked or are unavailable

Keep user-facing progress quiet during contact enrichment. Do not narrate every internal branch, command, or fallback. Give the user short updates only when:
- starting a major phase
- access is needed
- a blocker occurs
- the batch lookup is complete

## Access Rules

- Use the firm-wide Cision login when contact enrichment begins:
  - username: `pentashenandoah@pentagroup.com`
  - PIN: `1998`
  - access link: `https://app.cision.one/sign-in`
- Prefer the Cision API path when a saved bearer token exists. When using `$cision-workflow`, first try the saved token at `C:\Users\KellyPadalino\.codex\skills\cision-workflow\token.env` with `scripts/search-cision-api.js`; fall back to the Cision browser flow only if the token is missing, stale, or insufficient.
- If the current conversation includes Apollo access or a live session, use it only after Cision and prior lists.
- If Apollo requires login and no working session is available, open the Apollo login page and give the user one clear prompt to sign in in the browser window. Resume automatically once the session is active.
- Prefer the Apollo API path when a saved session exists. When using `$apollo-workflow`, first try `scripts/search-apollo-api.js` and `scripts/fetch-apollo-contact-api.js`; fall back to Apollo browser navigation only if the session is missing, stale, or insufficient.
- If Cision requires login, attempt the standard Cision login first before asking the user for help.
- If access is blocked by MFA, CAPTCHA, or browser restrictions, pause and ask the user for help.

Treat the Cision credentials above as firm-wide standard access, not user-specific personal credentials.

For Cision tasks, prefer handing off the browser work to `$cision-workflow` instead of handling the site as a generic browser session.
For Apollo tasks, prefer handing off the browser work to `$apollo-workflow` instead of handling the site as a generic browser session.
For final public-web verification, prefer using `$web-research-workflow` rather than treating web search as an ad hoc fallback.

## Existing Materials

Use the shared Google Drive folder of existing media lists as reference material when it is available in the conversation. Use these lists to:
- spot topic overlap with prior work
- reuse historically relevant names when they still fit
- recover existing contact details when appropriate
- understand useful list structure and fields

At the start of each new media-list task, re-check the shared Google Drive folder for newly added files and ingest any relevant new media lists before using prior-list context.

Do not rely on these lists as the only source of recommendations.

## Spreadsheet Rules

Create a clean, readable spreadsheet with one row per reporter.

Before approval, include:
- first name
- last name
- outlet
- title
- issues or topics covered
- a few recent or highly relevant article links

After approval and contact enrichment, finalize the sheet with:
- first name
- last name
- outlet
- title
- issues or topics
- email
- phone number

Keep formatting simple and organized. Remove duplicates and do not leave partial rows.

## Approval Gate

Do not collect final contact information until the user approves the proposed reporter list.

## Quality Check

Before finalizing:
- confirm the reporter still works at the listed outlet when possible
- verify the beat matches the prompt
- prefer recent relevant coverage
- ensure links are relevant and not broken when possible
- deduplicate names and outlets
- follow the required contact fallback order
- use `N/A` only after exhausting the fallback sequence
