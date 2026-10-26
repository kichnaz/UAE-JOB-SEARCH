# UAE Job Search — a Claude skill

A Claude skill that finds **active, genuine** job openings across UAE job portals and lists them newest first.

## What it does
1. Asks for your CV (optional) so it can match jobs to your experience.
2. Asks four filters:
   - **Position**: engineer, graphic designer, accountant, etc.
   - **Job type**: full-time, part-time, remote/hybrid or contract.
   - **Posted within**: 1 week, 2 weeks or last month.
   - **Genuineness filter**: verified only, or show all with risks flagged.
3. Searches LinkedIn, Bayt, Naukrigulf, GulfTalent, Indeed UAE, Dubizzle, foundit, Glassdoor, Dubai Careers and company career pages.
4. Confirms each job is still open and merges duplicates posted on several portals.
5. Rates each job ✅ Verified / ⚠️ Caution / ❌ Likely scam, using UAE-specific red flags. For example, the candidate being asked to pay visa fees, or recruiters reachable only on WhatsApp.
6. Lists the results newest first, with apply links.

## Files
- `SKILL.md` — the skill instructions
- `references/scam-checks.md` — the genuineness checklist

## Install in Claude
In claude.ai, open the `uae-job-search.skill` file Claude gives you and click **Save skill**. Alternatively, zip this folder and upload it under Settings → Capabilities → Skills.
