# Zinga & Co

A personal team of themed AI employees that runs inside [Claude Code](https://claude.com/claude-code). You describe a task and the output you want. **Nick Fury** plans the work and hands it from one specialist to the next: research, requirements, design, build, test, security, docs, deploy and support.

The characters are for fun. The deliverables stay professional.

> **Status: v0.1.0, early.** All 15 characters exist and one tiny end-to-end run has passed (a tip-calculator through Set 8, dry run). Trigger routing, the planted-flaw security test and a real support/incident pilot are not done yet. See [`docs/PROGRESS.md`](docs/PROGRESS.md).

## Install

Requires Claude Code. The repo is its own local marketplace.

```
/plugin marketplace add ~/gargantuas-army
/plugin install zinga-and-co@zinga-and-co
```

After pulling changes, run `/reload-plugins`. Check the manifest with `claude plugin validate .`.

## Use

| Command | Does |
|---|---|
| `/grand-slam <idea or task>` | Full run: intake, Mission Brief, then every Set the task needs |
| `/set <bucket> <task>` | Run one Set only |
| `/status` | Current run: done, running, blocked, open questions |
| `/continue` | Resume the last run after the last finished Set |
| `/roster` | Card view of every bucket and character |
| `/tesseract` | Memory summary: decisions, open bugs, AC coverage |

You can also just describe a job ("my site is down", "write the PRD") and Claude Code routes it to the character whose description fits. A named character always wins. A goal-level request ("take my idea to production") goes to Fury.

Example:

```
/grand-slam Build a tiny tip calculator web app with one save endpoint. No deploy.
```

## The roster

| Bucket | Character | Job |
|---|---|---|
| Nick Fury | Nick Fury | Plans runs, delegates, runs the gates. Never does the work himself |
| Caltech Lab | Doctor Strange, Penny | Market, competitor and user research |
| Pit Wall | Toto, Ted, Sheldon | PRD and scope; stories and acceptance criteria; devil's-advocate review |
| Shuri's Lab | Roger Federer | UX flows, UI spec, visual direction |
| Stark Industries | Tony, Jarvis, Friday | Architecture; frontend; backend and database |
| Hawk-Eye | Novak | Test plan before code, test execution, traceability |
| S.H.I.E.L.D. | Captain America | Security review (read-only) |
| Pit Crew | Max Verstappen | CI/CD, deploy, rollback plan |
| Homer | Homer | Documentation across the project |
| Pam's Front Desk | Pam | Support replies, FAQ, feedback |
| Wolowitz Workshop | Howard | Bug triage, incidents, dependency updates |

## How a run works

```
Intake -> Mission Brief (you approve)
 Set 1  Caltech Lab        research report
 Set 2  Pit Wall           PRD, stories + ACs, Bazinga review
        GATE 1: you approve the PRD and ACs
 Set 3  Shuri's Lab        UX/UI spec
 Set 4  Hawk-Eye           test plan from the ACs, before any code
 Set 5  Stark Industries   architecture, code and tests
 Set 6  Hawk-Eye + S.H.I.E.L.D.   test report, traceability, security review
 Set 7  Homer              README, API docs, user guide, runbook
        GATE 2 (Match Point): you approve go-live
 Set 8  Pit Crew           deploy
 Set 9  Homer              release notes, CHANGELOG
 After  Pam and Howard take over support and maintenance
```

- Fury sizes the run to the task. A bug fix does not get a Grand Slam.
- If Hawk-Eye or S.H.I.E.L.D. rejects the build, it goes back to Stark Industries with the failing items. Maximum 2 retries, then Fury asks you.
- Deploying, deleting data, sending messages, publishing and spending money always wait for your explicit OK.

## The Tesseract (memory)

Each product gets a `.army/` folder in its own repo, created from `templates/army/` on first use. It holds the research, PRD, stories, design, architecture, test plan, reports, deploy notes, runbook and a `handoffs/` folder with one Handoff Brief per Set. Typed IDs link everything: `REQ -> US -> AC -> TC -> BUG`, plus `DEC` for decisions. It is plain markdown in git, so you can edit any file.

## Repo layout

```
.claude-plugin/   plugin manifest and local marketplace
agents/           one file per character (subagents)
skills/           Nick Fury, the Tesseract, bucket playbooks, house rules
commands/         the six slash commands
templates/        .army/ scaffold and Handoff Brief
evals/            trigger prompts, dry-run scorecard and planted-flaw plan
docs/             design spec (docs/superpowers/specs/) and progress notes
references/       cloned source repos (git-ignored, read-only)
```

## Testing

- `evals/trigger-evals.json`: 40 plain-language prompts with the expected character. Not run yet; target 90% correct routing.
- `evals/dry-run/`: end-to-end scorecard on a tiny app, plus a planted-flaw security test. Part A passed; Part B is pending.

## Docs

- Design spec: [`docs/superpowers/specs/2026-09-29-gargantuas-army-design.md`](docs/superpowers/specs/2026-09-29-gargantuas-army-design.md)
- Progress and next steps: [`docs/PROGRESS.md`](docs/PROGRESS.md)

## Credits and license

Method adapted from open-source agent collections kept read-only in `references/` (msitarzewski/agency-agents, VoltAgent/awesome-claude-code-subagents, DietrichGebert/ponytail and others; see spec section 13). Coding house rules are written in our own words. MIT license per `plugin.json`; no LICENSE file has been added yet.
