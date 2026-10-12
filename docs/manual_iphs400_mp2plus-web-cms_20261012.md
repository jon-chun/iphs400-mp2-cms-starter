# Mini-Project #2plus: Extend Your Web CMS
**Course:** IPHS 400: Frontiers in AI, Kenyon College
**Instructor:** Jon A. Chun · **Date:** 2026-10-12 · **Weeks 8–9** · **Extra credit**
**Format:** Self-guided lab manual. A menu of optional components: pick any, work at your own pace.

---

## Front matter
### What this is
MP2plus is a **menu of 15 independent components** that extend the CMS you built in MP2. Each takes 1 to 2 hours and carries its own points. Pick any number, in any order. Your points are **added to your MP2 score**, up to **+15**.

The menu has three groups. **Features** (F1–F9) come from MP2's stretch menu. **Process** (P1–P4) covers habits MP2 introduced but never scored. **Security** (S1–S2) hardens your login and admin routes.

MP2plus is **not a repair pass**. Fixes to MP2 belong to the MP2 resubmission, due **Thu Oct 15**. MP2plus starts where your MP2 ends.

### The menu
| Id | Component | Time | Pts |
|---|---|---|---|
| F1 | Media uploads with alt text | 2 h | 4 |
| F2 | Categories and tags | 1.5 h | 3 |
| F3 | Revision history with rollback | 2 h | 4 |
| F4 | Scheduled publishing | 1.5 h | 3 |
| F5 | Site search | 1.5 h | 3 |
| F6 | Comments with moderation | 2 h | 4 |
| F7 | Plugin or hook system | 2 h | 4 |
| F8 | Switchable theme | 1.5 h | 3 |
| F9 | Live admin deploy behind the security gate | 2 h | 4 |
| P1 | Pull-request workflow | 1 h | 2 |
| P2 | `/prototype` side session | 1 h | 2 |
| P3 | Decision record | 1 h | 2 |
| P4 | Budget re-forecast | 1 h | 2 |
| S1 | Direct-URL access matrix | 1 h | 2 |
| S2 | Login hardening | 1.5 h | 3 |
| | **Menu total (only +15 count)** | **22.5 h** | **45** |

Every component pays about 2 points per estimated hour, so no choice is a shortcut. Pick what interests you, not everything. Reaching the +15 cap takes at least four components, about 7.5 hours at these estimates. Two or three components, 3 to 6 hours, is a solid MP2plus. Many of you finished MP2 well under its estimate, so you may be faster here too.

### Time and deadlines
| Milestone | What | When (Eastern, EDT) |
|---|---|---|
| **Assigned** | This manual and the rubric | **Tue Oct 13, 2026** |
| **Due** | Tag `mp2plus-final` pushed, report emailed | **Thu Oct 22, 2026, 2:40 pm** |
| **Grace** | No penalty | until **Fri Oct 23, 2026, 2:40 pm** |

Around the components you pick, two short parts are required and worth 0 points. Part 0, before you start, takes 15 minutes. Part 2, submit, takes 30 minutes.

### Prerequisites
- **A submitted MP2 repo**, `iphs400-mp2-cms`, with `mp2-final` pushed. If you are resubmitting MP2, finish that first (Thu Oct 15).
- Your MP2 setup still working: `uv`, `gh` logged in, Claude Code with the mattpocock skills, the hooks, and the usage ledger. Features reuse MP2 Part 6's per-ticket runbook.

> **Policy (set by the instructor).** MP2plus points are **added to your MP2 score** after any Oct 15 resubmission is graded. They are not a separate grade. Example: MP2 84 + MP2plus 7 = 91.
> **Cap: +15 points per student.** Points beyond the cap are not counted. The sum may pass 100 and is recorded as is: MP2 96 + MP2plus 8 = 104.
> **Points count for every student**, including an MP2 below 70. MP2 stays Pass/No-Pass at 70, applied after the addition. For MP2plus work only, this replaces the MP2 rubric's rule that extra credit counts only at 70 or above.
> **The gate.** A component earns points only if two checks hold at `mp2plus-final`. The MP2 checker still reports 0 failures, and every test that existed at `mp2plus-base` still passes. There is no partial credit against the gate.
> One resubmission within one week of feedback. Solo work: every commit after `mp2plus-base` must be authored by you. No `mp2plus-base` tag, no points.

### Deliverables checklist
Tick every line that applies before you tag `mp2plus-final`.

- [ ] Tag `mp2plus-base` pushed before your first MP2plus commit
- [ ] `notes/mp2plus-plan.md`: the components you intend to do, hour estimates, base check results
- [ ] Each feature: a `stretch` issue, named passing tests, a findings comment before close, two screenshots
- [ ] Each process or security component: the files its section names
- [ ] `docs/iphs400_mp2plus-web-cms_report_{first}-{last}_{YYYYMMDD}.md` with one evidence row per component
- [ ] MP2 checker at 0 failures and `uv run pytest -q` green, then tag `mp2plus-final` pushed and email sent
- [ ] F9 only: live admin URL and host in the README

Grading: the rubric is `docs/mp2plus-grading-rubric_20261012.md` in the starter repo, `jon-chun/iphs400-mp2-cms-starter`. Your repo was copied before these files existed, so read both on GitHub.

---

## Part 0 — Before you start
**Time: 15 min. No points, but required: without the base tag, no component earns points.**

### 0.1 Tag your baseline
The tag `mp2plus-base` marks the last commit of MP2. Everything after it is MP2plus, so it must exist before your first MP2plus commit. Without it the judge cannot tell the two projects apart.

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

### 0.2 Write your plan
Skim the menu in Part 1 and list what you intend to do. The plan may change. Your report says what you actually did. Create `notes/mp2plus-plan.md`, then commit it on `main` with the message `mp2plus: plan` and push:

```text
# MP2plus plan
Components: <id> about <N> h; <id> about <N> h; <id> about <N> h
At mp2plus-base: <last line of check_submission>; <last line of pytest>
```

If you pick P4, add its "before" forecast to this file now (see P4).

> **The gate.** At `mp2plus-final` the judge runs the same two checks. The MP2 checker must still report 0 failures, and every test that existed at `mp2plus-base` must still pass. If either fails, every component scores 0 until a resubmission fixes it. MP2plus must not break MP2.

---

## Part 1 — The menu
**Time: 1–2 h per component. 45 points on the menu; up to 15 count.**

- **Pick any.** The components are independent. Two need a partner: P1 needs one piece of work to carry, and P4 needs one other component to measure.
- **New work only.** A stretch goal you already built for MP2's extra credit cannot be chosen again as a feature.
- **Every point is a checklist item** with a measure and a check, listed in the rubric. One flaw costs points in one item only.

### 1.1 Features (F1–F9)
Each feature is a spec: an intent, an acceptance table, what the judge runs, what is out of scope, and a prompt.

- **The acceptance table is the contract.** Copy it into the issue word for word. The judge checks those steps and no others.
- **The Checked by column** uses five words. *walk*: the judge clicks through your app from a clean clone, at 1280 and 390 px. *publish*: the judge runs `uv run cms publish` and inspects `site/`. *test*: a named test for this step must exist and pass. *read*: the judge reads the file or code. *live*: the judge probes your live URL.

> **How a feature scores.** **Function, 2:** 2 if every acceptance step passes, 1 if at least half pass, 0 if fewer. A 4-point feature adds **Tests, 1**: every *test* step has a named, passing test. It also adds **Trail, 1**: the issue existed before your first commit reached GitHub, and a findings comment came before the issue closed. A 3-point feature has one combined **Tests and trail** point that needs all three conditions. **Screenshots missing:** minus 1, once per feature, never below 0.

#### The feature runbook
Run it once per feature. It is MP2 Part 6's per-ticket loop.

**1. The issue, before any code.** Title: `MP2plus <id>: <name>`, for example `MP2plus F2: Categories and tags`. Body: the intent, the acceptance table as a checklist, and `Blocked by: none`.
```bash
gh label create stretch --description "MP2plus feature"    # once; "already exists" is fine
gh issue create --label stretch
```

**2. Build it test first** with the feature's prompt and your issue number. Afterwards `cat .claude/state/phase` should print `T14`, so the ledger labels the work. The template's `CLAUDE.md` tells Claude to write it; if it is missing, run `echo T14 > .claude/state/phase` yourself.
```text
/clear
/implement #14 <the rest of the feature's prompt>
```

**3. Push after the first commit** with `git push`. The judge dates your work by when GitHub received it, so the issue must already exist.

**4. Review, post the findings, then close.** Two messages, then check with `gh issue view 14`:
```text
/code-review
Post the code-review findings and how each was resolved as a comment on issue #14, then commit and push with a message that starts "T14:" and ends "Closes #14".
```

**Expected:** state CLOSED, and your findings comment dated before the close. To ship the feature through a branch and pull request instead, follow P1; it also earns P1's points.

| Symptom | Cause | Fix |
|---|---|---|
| `gh issue create` says the label is not found | your copy has no `stretch` label | `gh label create stretch`, then retry |
| Issue did not close | the commit message or PR body lacked `Closes #N` | `gh issue close 14 --comment "closed by <sha or PR>"`; use the exact phrase next time |
| Your PR (P1) shows unrelated commits | you branched from another branch, not `main` | ask Claude to explain a rebase onto `main` before running it |
| MP2 checker fails on a transcript name | a new file in `docs/transcripts/` breaks MP2's pattern | use `iphs400_mp2-cms_chat-session_{NN}_{first}-{last}_{YYYYMMDD}.md` and continue your numbering |
| Scheduled test passes today, fails tomorrow | the code reads the real clock | pass `now` into the publish filter; tests pass a fixed time |
| Scheduled item goes public hours early or late | naive times compared with UTC | store times with their timezone; compare in the site's timezone |
| Images work locally, broken on Pages | a root-absolute `src="/media/x.png"`, or a post page missing `../` | use paths relative to the page; run MP2's `git grep` path check (MP2 Part 7) |
| Images missing from `site/` | `cms publish` does not copy `media/` | copy files into `site/media/` on publish; `media/` itself stays git-ignored |

#### F1 Media uploads with alt text (`media`, 2 h, 4 pts)
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

#### F2 Categories and tags (`categories`, 1.5 h, 3 pts)
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

#### F3 Revision history with rollback (`revisions`, 2 h, 4 pts)
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

#### F4 Scheduled publishing (`scheduled`, 1.5 h, 3 pts)
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

#### F5 Site search (`search`, 1.5 h, 3 pts)
Readers find a post without scrolling. `cms publish` writes a small search index of published items and a `search.html` page that queries it with one short JavaScript file. The admin content list gets a keyword search too. ADR-002 rejected a JavaScript application stack, not one script on the public site. If you also do P3, that exception makes a good ADR.

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

#### F6 Comments with moderation (`comments`, 2 h, 4 pts)
Readers send comments, and only the ones you approve are published. Comments enter through an admin "Moderation queue" form, because a static site cannot accept posts. How readers reach you is out of scope; if you also do P3, record that choice in your ADR. Approved comments are exported under their post and sanitized like Markdown.

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

#### F7 Plugin or hook system (`plugins`, 2 h, 4 pts)
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

#### F8 Switchable theme (`theme`, 1.5 h, 3 pts)
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

#### F9 Live admin deploy behind the security gate (`deploy`, 2 h, 4 pts)
**For students comfortable with hosting.** Your admin console goes on a free HTTPS host (Render, Fly, Railway or similar). ADR-001 kept it off the internet; if you also do P3, record the change in your ADR. Email the instructor when you choose F9: the reply holds a test login. Create that user on your live admin, and never commit it. Step 5 needs a rate limiter. If you also do S2, the limiter is scored once, in S2.

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
/implement #N Live admin deploy behind the security gate. Follow the issue's acceptance criteria exactly, with one named test per step marked test. Read the admin password and secret key from environment variables. Production never runs the demo seed. Reuse the S2 rate limiter if it exists; otherwise refuse a sixth failed login within 15 minutes.
```

### 1.2 Process (P1–P4)
MP2 introduced these habits but never scored them. Each takes about an hour.

#### P1 Pull-request workflow (1 h, 2 pts)
Carry one piece of work through a branch and a pull request: one feature from this menu, or one MP2 repair. It needs an issue first, as in step 1 of the feature runbook. Then:

```bash
git switch main && git pull
git switch -c mp2plus-<slug>
# build and commit, as in the feature runbook step 2
git push -u origin mp2plus-<slug>
gh pr create --title "MP2plus <id>: <name>" --body "Closes #14"
```

Review on the PR, before the merge. Then merge and check:
```text
/code-review
Post the code-review findings and how each was resolved as a comment on pull request #15, then commit and push the fixes to this branch.
```
```bash
gh pr merge 15 --merge --delete-branch
git switch main && git pull
gh issue view 14
```

**Expected:** state CLOSED, closed by pull request #15, and your review comment on the PR dated before the merge. **Scoring:** 2 for the complete chain; 1 if the PR merged without the review comment or without closing the issue. When P1 carries a feature, the PR comment also counts as that feature's findings comment.

- ✅ *"Findings: (1) upload accepted a renamed `.html` file; fixed by checking the first bytes, test added. (2) suggested Pillow for resizing; rejected, resizing is out of scope."* Posted before the merge.
- ❌ *"LGTM"*, or any comment posted after the merge.

#### P2 `/prototype` side session (1 h, 2 pts)
Pick one design question you cannot settle on paper. Examples: does a search index of 50 posts stay small enough for one file? Which theme folder layout keeps both themes' paths relative? Ask it in a separate session:

```text
/clear
/prototype <your question, in one sentence>
```

The skill builds throwaway code on its own branch, out of `main`. When you have an answer, run `/handoff` and save the result as `docs/handoff/{NN}_prototype-{topic}_{YYYYMMDD}.md`. It needs four parts: the question, what was tried, the finding, and what was carried back. Then add one `CONTEXT.md` entry naming the decision. **Scoring:** 2 if all four parts are there and the decision shows in your code. 1 if the handoff is there but the decision leaves no trace.

> **Folder name.** MP2 uses `docs/handoff/` (singular), and the MP2 checker counts handoffs there. The template's `CLAUDE.md` says `docs/handoffs/`. Either is accepted for MP2plus; the singular one also keeps the MP2 checker happy.

#### P3 Decision record (1 h, 2 pts)
Add a **Decisions** table to `CONTEXT.md` with at least three rows: the question, your choice, and why, in your own words. Use decisions you made in MP2 or MP2plus. Then write `docs/adr/ADR-003-{slug}.md` for the one that is hardest to reverse, in the format of `docs/adr/ADR-002-stack.md`: status, date, context, decision, consequences.

- ✅ `| Where do uploads live? | media/ at the repo root, git-ignored | Photos are 1–3 MB each; I did not want them in git history, same reason as cms.db |`
- ❌ `| Storage | Local filesystem | Simple, efficient, and follows best practices |` (it restates the model)

```text
Write docs/adr/ADR-003-<slug>.md in the format of docs/adr/ADR-002-stack.md. Decision: <what I decided>. Context: <what forced it>. Name the options I rejected and why. Keep it under 30 lines.
```

**Scoring:** 2 if the rows state real reasons and the ADR matches the code; 1 if only one of the two holds.

#### P4 Budget re-forecast (1 h, 2 pts)
Forecast your usage before another component, then check the forecast against what happened. Before you start that component, count the ticket-sized sessions ahead of you: about two per feature and one per other component. Use that count as N:

```bash
uv run python scripts/usage_report.py --remaining 4
```

**Expected:** the phase table, then `Forecast: 4 tickets need ~N% of the weekly cap; M% remains → FITS` (or `TIGHT`, or `OVER BUDGET`). Paste the "Tickets done" and "Forecast" lines into `notes/mp2plus-plan.md`. If the verdict is not `FITS`, apply MP2's fixes in order (MP2 Part 5.6), or pick a smaller component.

After at least one other component is done, run it again with the sessions you have left. Paste that output into your report. Then write three or four sentences comparing plan and actual: the cost per ticket before and after, and why it moved. Every number you cite must appear in `notes/usage-ledger.csv`. **Scoring:** 2 for both runs and the comparison; 1 for one run.

### 1.3 Security (S1–S2)
Two lines of `SECURITY-CHECKLIST.md` that MP2 did not score (#2 and #10), plus the rate-limited login from MP2's live-deploy stretch goal.

#### S1 Direct-URL access matrix (1 h, 2 pts)
Checklist #10: access control lives in code, not in hidden links. The test lists every admin-only route from the app's router, including any routes your features added.

```text
/clear
/tdd Write tests/test_access_matrix.py. Build the app with create_app() and collect every admin-only route from its router, marked in code so the test can find them. For each, assert client_as("editor") gets 403 or a redirect and an anonymous client is redirected to login. Do not hand-list routes.
```

**Expected:** `uv run pytest -q tests/test_access_matrix.py` passes, and a new admin route is tested without editing the test. **Scoring:** 2 if the list is generated from the router; 1 if it is hand-listed.

#### S2 Login hardening (1.5 h, 3 pts)
Two attacks, two fixes. Checklist #2: a wrong password and an unknown email must give the same message, or an attacker learns which emails have accounts. And guessing must be slowed: five failed logins within 15 minutes, then the next attempt is refused.

```text
/clear
/tdd Make a wrong password and an unknown email produce the same login error message and status code. Add a test that asserts both responses match.
```

```text
/clear
/tdd Rate-limit login: after 5 failed attempts within 15 minutes, refuse further attempts with 429 or a lockout message. Pass the clock in so the test can freeze it. Test that attempt 6 is refused and that logins work again after 15 minutes. No new dependencies.
```

**Scoring:** 1 for the same message and status, with a test. 1 for the limiter enforced. 1 for the limiter tested with a frozen clock.

| Symptom | Cause | Fix |
|---|---|---|
| Other tests start failing with 429 | limiter state is shared across tests | reset it in a fixture in `tests/conftest.py` |
| The limiter test passes once, fails on rerun | the test uses the real clock or `sleep` | freeze the clock; never sleep in a test |
| Live host: everyone locked out at once | behind a proxy, every request shares one address | key the limiter on the account as well (F9) |

---

## Part 2 — Submit
**Time: 30 min. No points, but required: no tag and email, no points.**

### 2.1 Report
Write `docs/iphs400_mp2plus-web-cms_report_{first}-{last}_{YYYYMMDD}.md`, one page. It has a table of the components you did, with the evidence for each. Under the table, write one sentence per component on what you left broken. If you did P4, add its second forecast and comparison. The judge checks every claim against the repo, not your style. The report earns no points itself. A component without its evidence row is scored from the repo alone, so the row only helps you.

```text
| Id | Evidence |
|----|----------|
| F2 | issue #14; tests/test_taxonomy.py; mp2plus-categories-1280.png, mp2plus-categories-390.png |
| P1 | PR #15 for F2 |
| S2 | tests/test_login_hardening.py |
```

### 2.2 Screenshots
Each feature you did, at 1280 px and 390 px, showing it working: `docs/screenshots/mp2plus-{slug}-{1280|390}.png`, two per feature. `{slug}` is the word in the feature's heading, for example `mp2plus-media-390.png`. A feature without both files loses 1 of its points.

### 2.3 Tag and email
```bash
uv run python scripts/check_submission.py --stage 2
uv run pytest -q
git tag mp2plus-final && git push --tags
```

**Expected:** no `FAIL` lines, all tests pass, and `* [new tag] mp2plus-final -> mp2plus-final`.

Email the instructor before the grace end, Fri Oct 23, 2:40 pm:

- **Subject:** `IPHS400 MP2plus - First Last` (any dash counts; the words must match)
- **Body:** repo URL, Pages URL, tag `mp2plus-final`, and the live admin URL if you built F9.
- **Attach:** your MP2plus report.

---

## Appendix A — How you are graded (summary)
Part 0 and Part 2 are required and worth 0 points. Each component on the menu carries its own points, 45 in all. Your points are summed, capped at +15, and added to your MP2 score. The gate applies to every component. Every point is a checklist item, and every lost point comes back with a specific fix. One flaw costs points in one item only. The full rubric is `docs/mp2plus-grading-rubric_20261012.md` in the starter repo; read it before you start.

## Appendix B — Where this content comes from
Everything in MP2plus was introduced in MP2 but not required or not scored. "MP2 rubric" is `docs/mp2-grading-rubric_20260922.md`; "MP2 manual" is `docs/manual_iphs400_mp2-web-cms_20260922.md`.

| MP2plus component | Introduced in | Line |
|---|---|---|
| F1–F8, the eight features | MP2 rubric, section 5 stretch menu, one row each in order | 234–241 |
| F9, live admin deploy | MP2 rubric, section 5; `docs/adr/ADR-001-local-admin-static-public.md` | 243; 23 |
| Issue, tests, and review per feature | MP2 rubric, section 5 | 230 |
| P1, pull-request workflow | MP2 rubric, section 5, "last two tickets done as branch + pull request" | 242 |
| P2, `/prototype` with `/handoff` | Lab 2 recorded segment; handoff path in MP2 manual Part 6.4 | 505 |
| P3, decision record and ADR | MP2 manual Part 6.1, "Expect few or no ADRs"; format in `docs/adr/ADR-002-stack.md` | 435 |
| P4, re-forecast with `--remaining` | MP2 manual Part 5.6 | 399 |
| S1, access matrix; S2 (a), login error | `SECURITY-CHECKLIST.md`, #10 and #2 | 17; 9 |
| S2 (b), rate-limited login | MP2 rubric, section 5, live-deploy security gate | 243 |
| "Review, revise, and extend" | Course page, Week 8 (`weeks/week-08.md`) | 10 |
