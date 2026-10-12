# Mini-Project #2plus: Extend Your Web CMS
**Course:** IPHS 400: Frontiers in AI, Kenyon College
**Instructor:** Jon A. Chun · **Date:** 2026-10-12 · **Weeks 8–9** · **Extra credit**
**Format:** Self-guided lab manual. Work at your own pace between class meetings.

---

## Front matter
### What you are building
Two new features for the CMS you built in MP2, chosen from MP2's stretch menu. You also practice four habits MP2 introduced but did not score: a branch and pull request per feature, a `/prototype` side session, an ADR, and a budget re-forecast. And you harden your login against two attacks from the security checklist. MP2plus is **not a repair pass**. Fixes to MP2 belong to the MP2 resubmission, due **Thu Oct 15**. MP2plus starts where your MP2 ends.

### Time and deadlines
| Milestone | What | When (Eastern, EDT) |
|---|---|---|
| **Assigned** | This manual and the rubric | **Tue Oct 13, 2026** |
| **Due** | Tag `mp2plus-final` pushed, report emailed | **Thu Oct 22, 2026, 2:40 pm** |
| **Grace** | No penalty | until **Fri Oct 23, 2026, 2:40 pm** |

| Part | Topic | Time |
|---|---|---|
| 0 | Baseline and plan | 15 min |
| 1 | Two feature modules from the stretch menu | 3–4 hrs (1.5–2 hrs each) |
| 2 | Process upgrades: branch + PR, `/prototype`, ADR, re-forecast | 60 min |
| 3 | Hardening from the security checklist | 45 min |
| 4 | Report addendum, screenshots, submission | 30 min |
| | **Total** | **5.5–6.5 hours over nine days** |

That is about half of MP2's 11–14 hour estimate. Many of you reported that MP2 took about 6 hours, so you may finish faster than the table says. A complete submission with **one** module takes about 4 hours and earns up to 35 of the 50 points.

### Prerequisites
- **A submitted MP2 repo**, `iphs400-mp2-cms`, with `mp2-final` pushed. If you are resubmitting MP2, finish that first (Thu Oct 15).
- Your MP2 setup still working: `uv`, `gh` logged in, Claude Code with the mattpocock skills, the hooks, and the usage ledger. MP2plus reuses MP2 Part 6's per-ticket runbook with a branch added.

### Learning objectives
By the end you will be able to:

1. Turn a feature idea into acceptance steps that someone else can check one by one.
2. Ship a feature through a branch, a pull request, a review, and a merge that closes its issue.
3. Answer a design question with throwaway code, then record the decision where it lasts: `CONTEXT.md` and an ADR.
4. Prove a login resists account enumeration and password guessing, with a test that freezes the clock.
5. Re-forecast your usage budget from your own ledger and explain the difference.

### Deliverables checklist
Tick every line before you tag `mp2plus-final`.

- [ ] Tag `mp2plus-base` pushed before your first MP2plus commit
- [ ] `notes/mp2plus-plan.md`: two modules, hour estimates, base check results, the "before" forecast
- [ ] Two issues labeled `stretch`, each closed by a merged pull request with a `/code-review` findings comment
- [ ] A named, passing test for every acceptance step marked *test*
- [ ] `docs/handoff/{NN}_prototype-{topic}_{YYYYMMDD}.md`, one new `CONTEXT.md` entry, and `docs/adr/ADR-003-{slug}.md`
- [ ] Part 3 tests: login message, access matrix, rate limiter
- [ ] `docs/screenshots/mp2plus-{module}-{1280|390}.png`, 4 files
- [ ] `docs/iphs400_mp2plus-web-cms_report_{first}-{last}_{YYYYMMDD}.md`
- [ ] MP2 checker at 0 failures and `uv run pytest -q` green, then tag `mp2plus-final` pushed and email sent
- [ ] Module 9 only: live admin URL and host in the README

Grading: the rubric is `docs/mp2plus-grading-rubric_20261012.md` in the starter repo, `jon-chun/iphs400-mp2-cms-starter`. Your repo was copied before these files existed, so read both on GitHub.

> **Policy (set by the instructor).** MP2plus is **extra credit, scored out of 50**. It is a separate course item worth **up to 2.5% of the course grade**, half of MP2's 5%: your credit is MP2plus/50 × 2.5%.
> **It can rescue an MP2 No-Pass:** an MP2 base score of 60 or more plus an MP2plus score of 35 or more counts as an MP2 Pass.
> One resubmission within one week of feedback, up to full credit.
> Solo work. Every commit after `mp2plus-base` must be authored by you. No `mp2plus-base` tag, no grade.

---

## Part 0 — Baseline and plan
**Time: 15 min. A gate: 0 points, but Part 1 depends on it.**

### 0.1 Tag your baseline
The tag `mp2plus-base` marks the last commit of MP2. Everything after it is MP2plus, so it must exist before your first MP2plus commit. Without it the judge cannot tell the two projects apart, and there is no grade.

```bash
cd ~/code/iphs400-mp2-cms
git switch main && git pull
uv run python scripts/check_submission.py --stage 2
uv run pytest -q
```

**Expected:** the checker prints no `FAIL` lines and ends with `N/N checks passed.`; pytest ends with `passed` and no failures. If either fails, fix it as MP2 work before you tag. Then tag the commit your MP2 ended on:

```bash
git tag mp2plus-base mp2-final      # if you did not resubmit MP2
git tag mp2plus-base <sha>          # if you resubmitted: <sha> is your resubmission commit (git log --oneline -3)
git push --tags
```

**Expected:** `* [new tag] mp2plus-base -> mp2plus-base`.

### 0.2 Choose, forecast, and plan
Skim the nine modules in 1.3–1.11 and pick two. Then forecast the work ahead:

```bash
uv run python scripts/usage_report.py --remaining 7
```

**Expected:** the phase table, then `Forecast: 7 tickets need ~N% of the weekly cap; M% remains → FITS` (or `TIGHT`, or `OVER BUDGET`). Seven counts the ticket-sized sessions ahead: about three per module and one for hardening. If the verdict is not `FITS`, apply MP2's fixes in order (MP2 Part 5.6), or pick a smaller module.

Create `notes/mp2plus-plan.md` with these four lines, then commit it on `main` with the message `mp2plus: plan` and push:

```text
# MP2plus plan
Modules: 1) <name>, about <N> h   2) <name>, about <N> h
At mp2plus-base: <last line of check_submission>; <last line of pytest>
Before forecast: <paste the "Tickets done" and "Forecast" lines>
```

> **The gate.** At `mp2plus-final` the judge runs the same two checks. The MP2 checker must still report 0 failures, and every test that existed at `mp2plus-base` must still pass. If either fails, Part 1 scores 0 until a resubmission fixes it. MP2plus must not break MP2.

---

## Part 1 — Two feature modules from the stretch menu
**Time: 3–4 hrs, 1.5–2 hrs per module. 30 points, 15 per module.**

### 1.1 Rules
- **Exactly two modules count.** A third is not scored. With one module, Part 1 is scored out of 15 and the rest as normal, so the maximum is 35.
- **New work only.** A stretch goal you already built for MP2 cannot be one of your two.
- **The acceptance table is the contract.** Copy it into the issue word for word. The judge checks those steps and no others.
- **The Checked by column** uses five words. *walk*: the judge clicks through your app from a clean clone, at 1280 and 390 px. *publish*: the judge runs `uv run cms publish` and inspects `site/`. *test*: a named test for this step must exist and pass. *read*: the judge reads the file or code. *live*: the judge probes your live URL.

> **How a module scores (15).** **Function, 9:** the judge walks your acceptance table; 9 if every step passes, 5 if at least half pass, 0 otherwise. **Tests, 3:** 3 if every step marked *test* has a named, passing test; 2 if at least half do; 1 if some do. **Slice and trail, 3:** 1 for an issue created before your first commit reached GitHub; 1 for a `/code-review` findings comment on the PR before the merge; 1 for commits that touch at least three of model, route, template, and test.

### 1.2 The module runbook
Read Part 2 first: its branch, PR, and `/prototype` work happens inside this runbook. Run it once per module, in order.

**1. The issue, before any code.** Title: `MP2plus: <module name>`. Body: the intent, the acceptance table as a checklist, and `Blocked by: none` (or `Blocked by: #N` if your second module depends on your first).
```bash
gh label create stretch --description "MP2plus module"    # once; "already exists" is fine
gh issue create --label stretch
```

**2. A branch from an up-to-date `main`, then build it test first** with the module's prompt from 1.3–1.11 and your issue number. Afterwards `cat .claude/state/phase` should print `T14`, so the ledger labels the work (the template's `CLAUDE.md` tells Claude to write it; if it is missing, run `echo T14 > .claude/state/phase` yourself).
```bash
git switch main && git pull
git switch -c mp2plus-<slug>
```

```text
/clear
/implement #14 <the rest of the module's prompt>
```

**3. Publish the branch and open the PR** after the first commit. The judge dates your work by when GitHub received it.
```bash
git push -u origin mp2plus-<slug>
gh pr create --title "MP2plus: <module name>" --body "Closes #14"
```

**4. Review on the PR, before the merge.** Two messages: run the review, then post the findings on the PR, not the issue (the template's `CLAUDE.md` says issue).
```text
/code-review
Post the code-review findings and how each was resolved as a comment on pull request #15, then commit and push the fixes with a message that starts "<slug>:".
```

**5. Merge and confirm.**
```bash
gh pr merge 15 --merge --delete-branch
git switch main && git pull
gh issue view 14
```

**Expected:** state CLOSED, closed by pull request #15, and your review comment on the PR dated before the merge.

| Symptom | Cause | Fix |
|---|---|---|
| `gh issue create` says the label is not found | your copy has no `stretch` label | `gh label create stretch`, then retry |
| Issue still open after the merge | the PR body lacked `Closes #N` | `gh issue close 14 --comment "closed by PR #15"`; use the phrase next time |
| Your second PR shows your first module's commits | you branched from the first module, not `main` | merge the first module first; ask Claude to explain a rebase onto `main` before running it |
| MP2 checker fails on a transcript name | a new file in `docs/transcripts/` breaks MP2's pattern | use `iphs400_mp2-cms_chat-session_{NN}_{first}-{last}_{YYYYMMDD}.md` and continue your numbering |
| Scheduled test passes today, fails tomorrow | the code reads the real clock | pass `now` into the publish filter; tests pass a fixed time |
| Scheduled item goes public hours early or late | naive times compared with UTC | store times with their timezone; compare in the site's timezone |
| Images work locally, broken on Pages | a root-absolute `src="/media/x.png"`, or a post page missing `../` | use paths relative to the page; run MP2's `git grep` path check (MP2 Part 7) |
| Images missing from `site/` | `cms publish` does not copy `media/` | copy files into `site/media/` on publish; `media/` itself stays git-ignored |

### 1.3 Module 1: Media uploads with alt text (`media`, about 2 hrs)
Editors upload images to an admin media library and place them in posts. Every image carries alt text, so a screen reader can describe it. Files live in `media/`, which is git-ignored like the database. `cms publish` copies them into `site/media/` with relative paths.

| # | Step | Checked by |
|---|---|---|
| 1 | Upload a PNG or JPEG of 2 MB or less with alt text; it appears in the media library | walk, test |
| 2 | An upload with empty alt text is rejected with a message | walk, test |
| 3 | A `.html` file, or any file over 2 MB, is rejected; the type is checked by extension and by content type | walk, test |
| 4 | Uploaded files are stored under `media/`, and `media/` is git-ignored | read |
| 5 | The editor can insert a Markdown image reference to a library item | walk |
| 6 | After `uv run cms publish`, the file is in `site/media/` and the post's `<img>` carries the alt text and a relative `src` | publish, test |

**The judge runs:** Upload an image, try an empty alt text, try a `.html` file, publish, then grep `site/` for `<img` and check each has `alt=`. **Out of scope:** Resizing, thumbnails, video, drag-and-drop, cloud storage.
```text
/implement #N Media uploads with alt text. Follow the issue's acceptance criteria exactly, with one named test per step marked test. Store uploads under media/ and add it to .gitignore. cms publish copies them to site/media/ and every src stays relative. Check the type by extension and by the file's first bytes. Ask before adding any dependency.
```

### 1.4 Module 2: Categories and tags (`categories`, about 1.5 hrs)
Readers browse by topic. Each post has one category and any number of tags. The admin content list filters by both, and `cms publish` writes one index page per category and per tag, listing published posts only.

| # | Step | Checked by |
|---|---|---|
| 1 | A post gets one category and several tags; both persist after save | walk, test |
| 2 | The admin content list filters by category and by tag | walk, test |
| 3 | `uv run cms publish` writes `category/<slug>.html` and `tag/<slug>.html`, each listing its published posts | publish, test |
| 4 | Each post page links to its category and tag pages with relative paths | publish |
| 5 | A draft with the same category and tag is absent from both index pages | publish, test |

**The judge runs:** Assign a category and tags, filter the admin list, publish, open both index pages, and confirm a draft is absent. **Out of scope:** Nested categories, tag clouds, categories on pages, renaming a tag across posts.
```text
/implement #N Categories and tags. Follow the issue's acceptance criteria exactly, with one named test per step marked test. One category per post, many tags. cms publish writes category/<slug>.html and tag/<slug>.html with relative links and published posts only. Add the new terms to CONTEXT.md.
```

### 1.5 Module 3: Revision history with rollback (`revisions`, about 2 hrs)
Nobody loses work to a bad edit. Every save of a post or page stores a revision. Rolling back copies an old revision forward as a new one, so the history only grows and nothing is deleted.

| # | Step | Checked by |
|---|---|---|
| 1 | Every save of a post or page stores a revision with who, when, title and body | test |
| 2 | The admin lists a post's revisions and shows any one of them | walk |
| 3 | Saving a post three times lists three revisions | walk, test |
| 4 | Rolling back to revision 1 adds revision 4, whose body equals revision 1; nothing is deleted | walk, test |
| 5 | Revisions survive a server restart | walk |

**The judge runs:** Save a post three times and check the list shows 3. Roll back to revision 1: the list shows 4, with revision 1's body. Restart the server and check again. **Out of scope:** Side-by-side diffs, pruning old revisions, autosave, revisions of users or settings.
```text
/implement #N Revision history with rollback. Follow the issue's acceptance criteria exactly, with one named test per step marked test. Store revisions in their own table. Rollback creates a new revision and never deletes or rewrites an old one.
```

### 1.6 Module 4: Scheduled publishing (`scheduled`, about 1.5 hrs)
An author finishes a post on Sunday and wants it public on Tuesday. A post or page gets a `publish_at` time and a `scheduled` status. `cms publish` exports it only once that time has passed in the site's timezone. Nothing runs in the background: the item goes public the next time you publish after its time.

| # | Step | Checked by |
|---|---|---|
| 1 | A post or page can be saved with a `publish_at` time and the status `scheduled` | walk, test |
| 2 | The admin content list shows scheduled items with their `publish_at` time | walk |
| 3 | `uv run cms publish` exports a scheduled item whose `publish_at` is now or earlier, in the site's timezone | publish, test |
| 4 | A scheduled item with a future `publish_at` is not exported | publish, test |
| 5 | The tests use a frozen clock: the current time is passed in, so they pass on any day | read, test |

**The judge runs:** Create one scheduled item in the past and one in the future, then publish. Only the past one is exported, and the admin list shows both with their states. **Out of scope:** A background scheduler, recurring posts, an unpublish-at time, email reminders.
```text
/implement #N Scheduled publishing. Follow the issue's acceptance criteria exactly, with one named test per step marked test. Pass the current time into the publish filter instead of reading the clock inside it, so tests can freeze it. Store times with their timezone and compare in the site's timezone. No new dependencies.
```

### 1.7 Module 5: Site search (`search`, about 1.5 hrs)
Readers find a post without scrolling. `cms publish` writes a small search index of published items and a `search.html` page that queries it with one short JavaScript file. The admin content list gets a keyword search too. ADR-002 rejected a JavaScript application stack, not one script on the public site; record that exception in your ADR (Part 2.3).

| # | Step | Checked by |
|---|---|---|
| 1 | `uv run cms publish` writes `site/search-index.json` with the title, slug and excerpt of each published item | publish, test |
| 2 | Drafts are absent from the index | publish, test |
| 3 | `site/search.html` uses one vanilla JavaScript file of 60 lines or fewer, with no external dependency and relative paths | read |
| 4 | A query typed into `search.html` returns the matching slug | walk |
| 5 | The admin content list has a keyword search that returns the matching row | walk, test |

**The judge runs:** Confirm drafts are absent from the index, type a query on the published search page, and search the admin list. Preview with `python3 -m http.server -d site 8001`: opening the file directly blocks the index fetch. **Out of scope:** Fuzzy matching, ranking, a search server, searching drafts from the public site.
```text
/implement #N Site search. Follow the issue's acceptance criteria exactly, with one named test per step marked test. cms publish writes search-index.json from published items only. search.html loads one plain JavaScript file of at most 60 lines, with relative paths and no library.
```

### 1.8 Module 6: Comments with moderation (`comments`, about 2 hrs)
Readers send comments, and only the ones you approve are published. Comments enter through an admin "Moderation queue" form, because a static site cannot accept posts. How readers reach you is out of scope: say so in your ADR (Part 2.3). Approved comments are exported under their post and sanitized like Markdown.

| # | Step | Checked by |
|---|---|---|
| 1 | A comments table stores reader name, body, status (pending, approved or rejected), post and time | read, test |
| 2 | The admin Moderation queue form creates a pending comment, and approve and reject both work | walk, test |
| 3 | A pending comment is absent from the export; after approval and a new publish it appears under its post | publish, test |
| 4 | A rejected comment is absent from the export | publish, test |
| 5 | A comment carrying MP2's D4 payload (`<script>` and an `onerror=` attribute) does not execute in the queue or the export | walk, test |

**The judge runs:** Create a pending comment and publish (absent), approve it and publish (present), reject another (absent), then run the D4 payload through a comment. **Out of scope:** A public comment form, reader accounts, spam filtering, threaded replies, email notifications.
```text
/implement #N Comments with moderation. Follow the issue's acceptance criteria exactly, with one named test per step marked test. Comments enter only through the admin Moderation queue form. Sanitize comment bodies with the same sanitizer as post Markdown, and export approved comments only.
```

### 1.9 Module 7: Plugin or hook system (`plugins`, about 2 hrs)
New behavior arrives without editing the publisher. A small registry offers two hook points, `before_publish(item)` and `render_body(html, item)`. Plugins are Python files in `app/plugins/`, switched on and off in admin settings. You ship one real plugin: reading time or a table of contents.

| # | Step | Checked by |
|---|---|---|
| 1 | The registry offers two hooks, `before_publish(item)` and `render_body(html, item)` | read, test |
| 2 | Plugins are discovered from `app/plugins/` without editing the publisher | test |
| 3 | With the shipped plugin enabled, its output appears in the exported post | publish, test |
| 4 | With the plugin disabled in admin settings, its output is absent | walk, publish |
| 5 | Hook order is deterministic: a second test plugin runs after the first | test |

**The judge runs:** Enable the plugin and publish (output present), disable it and publish (absent), and run your ordering test. **Out of scope:** Installing plugins from the internet, plugin dependencies, editing plugin code in the browser.
```text
/implement #N Plugin or hook system. Follow the issue's acceptance criteria exactly, with one named test per step marked test. Two hooks only: before_publish(item) and render_body(html, item). Discover plugins from app/plugins/ in a fixed order, and ship one plugin: reading time or table of contents.
```

### 1.10 Module 8: Switchable theme (`theme`, about 1.5 hrs)
Your client can change the site's look without a developer. Two themes, each a CSS file with optional template overrides, are selectable on an admin Settings page. `cms publish` exports whichever theme is selected.

| # | Step | Checked by |
|---|---|---|
| 1 | Two themes exist, each a CSS file with optional template overrides | read |
| 2 | An admin Settings page selects the theme, and the choice persists | walk, test |
| 3 | `uv run cms publish` exports the selected theme's CSS with relative paths | publish, test |
| 4 | Switching back and publishing again restores the first theme's CSS | publish |
| 5 | Both themes show no horizontal overflow at 390 px on the home page and one post | walk |

**The judge runs:** Switch the theme, publish, confirm the exported CSS file, and check both themes at 390 px. **Out of scope:** Theme upload, per-post themes, a visual theme editor.
```text
/implement #N Switchable theme. Follow the issue's acceptance criteria exactly, with one named test per step marked test. Keep each theme in its own folder. cms publish copies only the selected theme's CSS into site/ with relative paths. Check both themes at 390 px wide.
```

### 1.11 Module 9: Live admin deploy behind the security gate (`deploy`, about 2 hrs)
**For students comfortable with hosting; the rate limiter alone also earns Part 3 credit (3.3).** Your admin console goes on a free HTTPS host (Render, Fly, Railway or similar). ADR-001 kept it off the internet, so record the change in your ADR (Part 2.3). Email the instructor when you choose this module: the reply holds a test login. Create that user on your live admin, and never commit it.

| # | Step | Checked by |
|---|---|---|
| 1 | The admin console answers 200 over HTTPS on a free host | live |
| 2 | The admin password and secret key are read from environment variables, and neither appears in git | read, test |
| 3 | No seeded default accounts in production: the demo logins from `.env.example` and `tests/conftest.py` are refused | live, test |
| 4 | The judge logs in with the test login the instructor emailed you | live |
| 5 | After five wrong passwords within 15 minutes, a sixth attempt gets 429 or a lockout message | live |
| 6 | README names the live URL and the host, and a `docs/handoff/` note says how to redeploy | read |

**The judge runs:** Load the URL over HTTPS, log in with the test login, try the demo passwords, then send six bad logins for an unknown email. **Out of scope:** A custom domain, uptime, keeping content across redeploys, email.
```text
/implement #N Live admin deploy behind the security gate. Follow the issue's acceptance criteria exactly, with one named test per step marked test. Read the admin password and secret key from environment variables. Production never runs the demo seed. Reuse the Part 3 rate limiter.
```

---

## Part 2 — Process upgrades
**Time: 60 min, most of it inside the Part 1 runbook. 10 points.**

### 2.1 Branch and pull request (3 pts)
The runbook in 1.2 does this. Every scored module is merged by a pull request whose body closes its issue, with a `/code-review` findings comment on the PR before the merge. It is MP2's stretch goal "last two tickets done as branch + pull request", now for every module.

- ✅ *"Findings: (1) upload accepted a renamed `.html` file; fixed by checking the first bytes, test added. (2) suggested Pillow for resizing; rejected, resizing is out of scope."* Posted before the merge.
- ❌ *"LGTM"*, or any comment posted after the merge.

### 2.2 A `/prototype` side session (3 pts)
Pick one design question in one of your modules that you cannot settle on paper. Examples: does a search index of 50 posts stay small enough for one file? Which theme folder layout keeps both themes' paths relative? Ask it in a separate session:

```text
/clear
/prototype <your question, in one sentence>
```

The skill builds throwaway code on its own branch, out of `main`. When you have an answer, run `/handoff` and save the result as `docs/handoff/{NN}_prototype-{topic}_{YYYYMMDD}.md`. It needs four parts: the question, what was tried, the finding, and what was carried back. Then add one `CONTEXT.md` entry naming the decision, and make sure the decision shows in the merged module.

> **Folder name.** MP2 uses `docs/handoff/` (singular), and the MP2 checker counts handoffs there. The template's `CLAUDE.md` says `docs/handoffs/`. Either is accepted for MP2plus; the singular one also keeps the MP2 checker happy.

### 2.3 One ADR (2 pts)
Write `docs/adr/ADR-003-{slug}.md` in the format of `docs/adr/ADR-002-stack.md`: status, date, context, decision, consequences. Pick a module decision that is hard to reverse; modules 5, 6, and 9 already hand you one. Full credit needs all three sections, and the decision must match the code.

```text
Write docs/adr/ADR-003-<slug>.md in the format of docs/adr/ADR-002-stack.md. Decision: <what I decided>. Context: <what forced it>. Name the options I rejected and why. Keep it under 30 lines.
```

### 2.4 Re-forecast (2 pts)
Your "before" forecast is in `notes/mp2plus-plan.md` (0.2). After your second module merges, run the forecast again for the one session left, hardening:

```bash
uv run python scripts/usage_report.py --remaining 1
```

**Expected:** the phase table now includes your two module phases, then a `Forecast:` line. Put both forecasts in your report and compare them: the average weekly cost per ticket before and after, and why it moved.

---

## Part 3 — Hardening from the security checklist
**Time: 45 min. 5 points.** Two lines of `SECURITY-CHECKLIST.md` that MP2 did not score (#2 and #10), plus the rate-limited login from MP2's live-deploy stretch goal.

### 3.1 Non-revealing login error (1 pt)
Checklist #2: a wrong password and an unknown email must produce the same message. Otherwise an attacker learns which emails have accounts.

```text
/clear
/tdd Make a wrong password and an unknown email produce the same login error message and status code. Add a test that asserts both responses match.
```

### 3.2 Direct-URL access matrix (2 pts)
Checklist #10: access control lives in code, not in hidden links. The test lists every admin-only route from the app's router, including your new modules' routes. A list generated from the router earns 2; a hand-written list earns 1.

```text
/clear
/tdd Write tests/test_access_matrix.py. Build the app with create_app() and collect every admin-only route from its router, marked in code so the test can find them. For each, assert client_as("editor") gets 403 or a redirect and an anonymous client is redirected to login. Do not hand-list routes.
```

**Expected:** `uv run pytest -q tests/test_access_matrix.py` passes, and a new admin route is tested without editing the test.

### 3.3 Rate-limited login (2 pts)
Five failed logins within 15 minutes, then the next attempt is refused with 429 or a lockout message. Enforced and tested with a frozen clock earns 2; enforced only earns 1.

```text
/clear
/tdd Rate-limit login: after 5 failed attempts within 15 minutes, refuse further attempts with 429 or a lockout message. Pass the clock in so the test can freeze it. Test that attempt 6 is refused and that logins work again after 15 minutes. No new dependencies.
```

| Symptom | Cause | Fix |
|---|---|---|
| Other tests start failing with 429 | limiter state is shared across tests | reset it in a fixture in `tests/conftest.py` |
| The limiter test passes once, fails on rerun | the test uses the real clock or `sleep` | freeze the clock; never sleep in a test |
| Live host: everyone locked out at once | behind a proxy, every request shares one address | key the limiter on the account as well (module 9) |

---

## Part 4 — Report addendum, screenshots, submission
**Time: 30 min. 5 points.**

### 4.1 Report (3 pts)
Write `docs/iphs400_mp2plus-web-cms_report_{first}-{last}_{YYYYMMDD}.md`, one page, with five parts. The judge checks every claim against the repo, not your style.

1. The modules you chose and why, in your own words.
2. A **decisions table** with at least three rows: the question, your choice, and why.
3. One drift instance and the step that caught it (MP2 Part 6.5).
4. The two forecasts from 2.4, and what changed between them.
5. What you left broken.

- ✅ `| Where do uploads live? | media/ at the repo root, git-ignored | Photos are 1–3 MB each; I did not want them in git history, same reason as cms.db |`
- ❌ `| Storage | Local filesystem | Simple, efficient, and follows best practices |` (it restates the model)

### 4.2 Screenshots (1 pt)
Each scored module at 1280 px and 390 px, showing the module working: `docs/screenshots/mp2plus-{module}-{1280|390}.png`, 4 files. `{module}` is the slug in the module's heading, for example `mp2plus-media-390.png`.

### 4.3 Submit (1 pt)
```bash
uv run python scripts/check_submission.py --stage 2
uv run pytest -q
git tag mp2plus-final && git push --tags
```

**Expected:** no `FAIL` lines, all tests pass, and `* [new tag] mp2plus-final -> mp2plus-final`.

Email the instructor before the grace end, Fri Oct 23, 2:40 pm:

- **Subject:** `IPHS400 MP2plus - First Last` (any dash counts; the words must match)
- **Body:** repo URL, Pages URL, tag `mp2plus-final`, and the live admin URL if you built module 9.
- **Attach:** your MP2plus report.

---

## Appendix A — How you are graded (summary)
Part 0 is a gate worth 0 points. Part 1 is 30 (15 per module), Part 2 is 10, Part 3 is 5, and Part 4 is 5, for 50 in total. Every point is a checklist item, and every lost point comes back with a specific fix. One flaw costs points in one item only. The full rubric is `docs/mp2plus-grading-rubric_20261012.md` in the starter repo; read it before you start.

## Appendix B — Where this content comes from
Everything in MP2plus was introduced in MP2 but not required or not scored. "MP2 rubric" is `docs/mp2-grading-rubric_20260922.md`; "MP2 manual" is `docs/manual_iphs400_mp2-web-cms_20260922.md`.

| MP2plus item | Introduced in | Line |
|---|---|---|
| Modules 1–8 | MP2 rubric, section 5 stretch menu, one row each in module order | 234–241 |
| Module 9, live admin deploy | MP2 rubric, section 5; `docs/adr/ADR-001-local-admin-static-public.md` | 243; 23 |
| One `stretch` issue, tests, and review per goal | MP2 rubric, section 5 | 230 |
| 2.1 Branch and pull request | MP2 rubric, section 5, "last two tickets done as branch + pull request" | 242 |
| 2.2 `/prototype` with `/handoff` | Lab 2 recorded segment; handoff path in MP2 manual Part 6.4 | 505 |
| 2.3 ADR | MP2 manual Part 6.1, "Expect few or no ADRs"; format in `docs/adr/ADR-002-stack.md` | 435 |
| 2.4 Re-forecast with `--remaining` | MP2 manual Part 5.6 | 399 |
| 3.1 Login error, 3.2 Access matrix | `SECURITY-CHECKLIST.md`, #2 and #10 | 9; 17 |
| 3.3 Rate-limited login | MP2 rubric, section 5, live-deploy security gate | 243 |
| "Review, revise, and extend" | Course page, Week 8 (`weeks/week-08.md`) | 10 |
