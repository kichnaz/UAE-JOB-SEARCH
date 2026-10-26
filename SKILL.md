---
name: uae-job-search
description: Search active job openings across all major UAE job portals (LinkedIn, Bayt, Naukrigulf, GulfTalent, Indeed UAE, Dubizzle, foundit/Monster Gulf, Glassdoor, Dubai Careers and company career pages) for a given position, filter by job type and posting date, verify each listing is still open and genuine, and list results newest first. Use this skill whenever the user wants to find jobs, vacancies, openings or hiring posts in the UAE, Dubai, Abu Dhabi, Sharjah or the other emirates — even if they just say "find me designer jobs in Dubai", "any engineer vacancies?", "job search", "who is hiring", or upload a CV and ask what fits.
---

# UAE Job Search

Find **currently active, genuine** job openings in the UAE for the position the user wants, filtered by job type and posting date, and present them **most recent first**.

The value of this skill is trust: a short list of real, still-open jobs beats a long list of expired or scam posts. Every listing you present should have been checked for being active and for genuineness.

---

## Step 1 — Ask for the CV (optional upload)

Start by inviting the user to upload their CV/resume (PDF, DOCX or image). Explain in one line why: it lets you match jobs to their real experience, pick the right seniority and add a "fit" note to each result. If they skip it, carry on without it.

If a CV is uploaded, read it and note: current title, years of experience, key skills/software, industries, languages, and location. Use these to sharpen search keywords and to score fit later. Do not ask the user to retype anything the CV already tells you.

## Step 2 — Ask the search filters

Use the `AskUserQuestion` tool (one call, up to 4 questions) so the user can tap answers. If the tool isn't available, ask in plain text in one short message.

1. **Position** — e.g. Graphic Designer, Engineer, Accountant, Sales Executive, Nurse… (offer 3 sensible options; if a CV was uploaded, base the options on it; the user can always type their own). If the user names a broad title like "engineer", ask which kind (civil, mechanical, software, electrical…) only if the answer would change the search a lot.
2. **Job type** (multi-select) — Full-time · Part-time · Remote/Hybrid · Contract/Freelance
3. **Posted within** — Last 1 week · Last 2 weeks · Last month
4. **Genuineness filter** — "Verified only" (hide anything with a red flag) · "Show all, but flag risks"

Also accept an emirate preference if the user volunteers one (Dubai, Abu Dhabi, Sharjah, etc.). Default: all UAE.

If the user's first message already answers some of these, don't ask them again — only ask what is missing.

## Step 3 — Search every portal

Get today's date first (use a time tool if available) so the date filter is exact.

Search each source **separately** — a combined query returns shallow results. Run the searches in parallel where possible. Use the position + "UAE"/"Dubai" + job-type words, and the date window.

| Source | How to search |
|---|---|
| LinkedIn Jobs | `site:linkedin.com/jobs "<position>" UAE` (or Dubai) |
| Bayt | `site:bayt.com "<position>" UAE` |
| Naukrigulf | `site:naukrigulf.com "<position>" UAE` |
| GulfTalent | `site:gulftalent.com "<position>" UAE` |
| Indeed UAE | `site:ae.indeed.com "<position>"` |
| Dubizzle Jobs | `site:dubizzle.com jobs "<position>"` |
| foundit (Monster Gulf) | `site:foundit.ae "<position>"` |
| Glassdoor | `site:glassdoor.com "<position>" UAE` |
| Government | Dubai Careers (dubaicareers.ae), Abu Dhabi gov jobs, TAMM — for public-sector roles |
| Company career pages | `"<position>" careers Dubai apply` — big UAE employers often post here first |

Use `WebSearch` with `mode: "standard"`; switch to `"extended"` when results are thin or look stale. Aim for roughly 20–40 raw hits before filtering.

Many portals block plain page fetching. When `WebFetch` fails on a portal, and a browser tool is available (built-in browser or Claude in Chrome — load its skill first), open the listing there to read the date and status. If neither works, rely on the search snippet and mark the listing **"status unconfirmed"** rather than guessing.

## Step 4 — Confirm each job is ACTIVE

A job counts as active only if all of these hold:

- The posting date (or "x days ago") is inside the user's window. Convert relative dates ("3 days ago") to an actual date using today's date.
- The page does not say "No longer accepting applications", "Expired", "Closed", "Position filled", or redirect to a generic search page.
- An apply button / apply email / application link is still present.

Drop expired posts entirely. If the date can't be found at all, keep it only in a separate "date unknown" group at the bottom, never mixed in with dated results.

**De-duplicate**: the same job is often on 3–4 portals. Merge them into one entry, keep the earliest posting date, and list the extra links. Prefer the company's own career page as the main link, since that is the most trustworthy place to apply.

## Step 5 — Check GENUINENESS

Rate each job: **✅ Verified**, **⚠️ Caution**, or **❌ Likely scam**. Read `references/scam-checks.md` for the full checklist. In short:

**Strong red flags (→ ❌):**
- Asks the candidate to pay anything — visa fee, registration, training, medical, "processing" (UAE law: the employer pays recruitment and visa costs)
- Contact only by WhatsApp/Telegram, or a personal Gmail/Yahoo/Hotmail address claiming to be a big company
- Salary far above market for the role with no experience required, "immediate joining, no interview"
- Company name not findable online, or website domain doesn't match the company

**Caution signs (→ ⚠️):**
- Recruitment agency not naming the client, and no MOHRE licence mentioned
- Very vague description, no company name ("confidential")
- Posted only on classifieds (e.g. Dubizzle) and nowhere else
- Same text re-posted repeatedly for months

**Verified signals (→ ✅):**
- Listed on the company's official careers page, or posted by the company's verified LinkedIn page
- Company has a real website, LinkedIn presence and UAE address
- Corporate email domain, clear job description and requirements

When the user chose "Verified only", hide ❌ and ⚠️ jobs but say how many were hidden and why, in one line.

## Step 6 — Present the results

Sort **newest posting date first**. Format:

```
## <Position> jobs in UAE — <job type(s)>, posted in last <window>
Searched <date> · <N> active jobs found · <M> hidden (expired/duplicate/flagged)

### 1. <Job title> — <Company>
📍 <Emirate/City> · 🕒 <Full-time/Part-time/Remote> · 📅 Posted <date> (<x days ago>)
💰 <Salary if listed, else "Not stated">
🛡️ <✅ Verified / ⚠️ Caution: reason / ❌ reason>
🎯 Fit: <one line, only if a CV was given — matching skills/gaps>
🔗 Apply: <main link> · also on: <other portal links>
```

Then, after the list:
- A **"date unknown / status unconfirmed"** group if any (clearly separated).
- A one-line tip if relevant, e.g. which 2–3 jobs look like the strongest match.
- An offer of a next step: tailor the CV or write a cover letter for a chosen job, or re-run the search with different filters.

If more than ~15 jobs survive filtering, offer to also save them as a spreadsheet (title, company, emirate, type, posted date, salary, genuineness, link) so the user can track applications.

If nothing active is found, say so plainly, show what was searched, and suggest one or two adjustments (broader title, longer date window, related titles).

---

## Things to get right

- **Never invent jobs, dates, salaries or links.** Only list what you actually retrieved. A real link the user can click is the whole point.
- Never apply, message a recruiter, or enter personal data on the user's behalf without asking first.
- Keep the CV private: don't paste its contents into searches; use only job-title and skill keywords.
- Dates matter more than volume — a job posted 2 days ago is worth more to the user than ten posted 5 weeks ago.
