# GLASSBOTS DIAGNOSTIC

**Health Score: 37 / 100 - FAILING**

Recommendation: Rebuild. Salvage the knowledge, not the machinery.

**Handholding Ratio:** not measurable from the system capture alone -
requires a Tier 5 interview to convert stalls and interventions into
hours per completed item. *(Reported basis: structural evidence only.)*

## The three biggest causes

1. **Authority system answers 'may I?' - nothing answers 'what is done?'.** There is a real permission engine here: deny-by-default rules, tamper-evident policy hashes. There is no equivalent machinery defining what a finished task looks like. Under deny-by-default, stopping early violates no rule while acting carries risk - so the safe behaviour is to stall, and a person must push every task through.
2. **No machine-checkable definition of 'finished' anywhere in the system.** Across task configs, schedulers, and governance code there is no acceptance criterion, no test gate, no named-output contract. 'Done' is decided by a person looking at things - so the system can stop early and nothing detects it.
3. **2 scheduled job(s) failing repeatedly in the last 48 hours.** Work the system scheduled for itself is not completing - it is erroring on schedule. Nothing in the captured evidence shows these jobs reaching a finished state.

## Scores

- Completion           0/10
- Handholding          5/10
- Idle Burn            5/10
- Approval Drag        5/10
- Definition of Done   2/10
- Invented Work        7/10
- Visibility           6/10
- Editability          7/10
- Resilience           0/10
- Lock-in & Exposure   0/10

---
*Generated 2026-08-20 11:36 by GlassBots Diagnostic. All findings Verified against the captured system unless marked otherwise.*

# Findings by check

## Check 1 - Completion (0/10)
*Is anything actually finishing?*

### [CRITICAL] 2 scheduled job(s) failing repeatedly in the last 48 hours
*Basis: Verified - deterministic analysis of the captured system*

Work the system scheduled for itself is not completing - it is erroring on schedule. Nothing in the captured evidence shows these jobs reaching a finished state.

Evidence:
- `runtime/journal/error-grep-48h.txt` - job failed in journal: Agent A drip-feed (hourly briefs)
- `runtime/journal/error-grep-48h.txt` - job failed in journal: Agent B training (2x daily artifacts)

### [HIGH] Architecture document describes design but no operational metrics
*Basis: Verified - deterministic analysis of the captured system*

The document that explains this system contains no throughput, completion rate, cycle time, or 'what shipped' figures. It describes intent, not behaviour - which is usually why nobody notices work quietly stalling.

Evidence:
- `hermes/SYSTEM-ARCHITECTURE.md` - 0 operational metric references in 9 words

### [HIGH] 1 dispatched task(s) stalled with zero execution attempts
*Basis: Verified - deterministic analysis of the captured system*

Work was dispatched and recorded, then never progressed - and stayed in place rather than escalating. Stalled-but-recorded is the signature of dispatch without a completion loop.

Evidence:
- `hermes\profiles\alpha\desktop\interrupted_turns.json` - 1 turn(s) recorded with zero attempts

## Check 2 - Handholding (5/10)
*Who's doing the work, the system or the person?*

### [HIGH] Evidence of work waiting on a human push
*Basis: Verified - deterministic analysis of the captured system*

Interrupted, zero-attempt tasks sitting in place mean the system's default state is 'waiting for the operator'. That is handholding, measured in artifacts rather than hours.

Evidence:
- `hermes/profiles/*/desktop/interrupted_turns.json` - 1 stalled task record(s)

### [MEDIUM] Work routes through 5 persistent agent identities
*Basis: Verified - deterministic analysis of the captured system*

Every identity in the chain is a place work can stop and wait for a person to push it along - and the captured sign-off receipts show real hand-offs occurring. The captured state shows exactly that: tasks parked mid-chain with nobody driving.

Evidence:
- `hermes/profiles/` - 5 profile directories with independent state

## Check 3 - Idle Burn (5/10)
*Is it spending money while producing nothing?*

### [CRITICAL] Inference credit runway at 0.9% of allocation
*Basis: Verified - deterministic analysis of the captured system*

990.50 of 1000 credits consumed. This is the one resource every AI capability on the host depends on, and nothing in the captured monitoring watches it.

Evidence:
- `runtime/openrouter-credits.txt` - total_usage 990.50 / total_credits 1000

## Check 4 - Approval Drag (5/10)
*Are the safety rules blocking the work?*

### [CRITICAL] Authority system answers 'may I?' - nothing answers 'what is done?'
*Basis: Verified - deterministic analysis of the captured system*

There is a real permission engine here: deny-by-default rules, tamper-evident policy hashes. There is no equivalent machinery defining what a finished task looks like. Under deny-by-default, stopping early violates no rule while acting carries risk - so the safe behaviour is to stall, and a person must push every task through.

Evidence:
- `governance\canonical_allowlist.py` - # deny by default
- `governance\canonical_allowlist.py` - DENY = ['credentials', 'spend']
- `governance\canonical_allowlist.py` - import hashlib
- `governance\canonical_allowlist.py` - return hashlib.sha256(artifact).hexdigest()
- `governance\policy_hash.py` - import hashlib

## Check 5 - Definition of Done (2/10)
*Does the system know when it's finished?*

### [CRITICAL] No machine-checkable definition of 'finished' anywhere in the system
*Basis: Verified - deterministic analysis of the captured system*

Across task configs, schedulers, and governance code there is no acceptance criterion, no test gate, no named-output contract. 'Done' is decided by a person looking at things - so the system can stop early and nothing detects it.

Evidence:
- `governance/, scripts/, hermes/cron/` - 0 matches for acceptance-criteria patterns

### [HIGH] Verification machinery proves existence, not substance
*Basis: Verified - deterministic analysis of the captured system*

The captured verification code recomputes hashes and checks that artifacts exist. Nothing assesses whether an artifact does the job it was made for. Rigorous verification of an undefined target produces confident nonsense, expensively.

Evidence:
- `governance\canonical_allowlist.py` - import hashlib
- `governance\canonical_allowlist.py` - return hashlib.sha256(artifact).hexdigest()
- `governance\policy_hash.py` - import hashlib

## Check 6 - Invented Work (7/10)
*Is it generating its own to-do list?*

### [HIGH] 2 recurring self-scheduled job(s) detected
*Basis: Verified - deterministic analysis of the captured system*

Scheduled activity with no business request behind it: Agent A drip-feed (hourly briefs); Agent B training (2x daily artifacts)

Evidence:
- `runtime/journal/error-grep-48h.txt` - recurring job names in the failure log

## Check 7 - Visibility (6/10)
*Can you see what happened without a developer?*

### [MEDIUM] Dead integration 'integration-x' failing authentication 5 times in 48h
*Basis: Verified - deterministic analysis of the captured system*

An integration is configured, failing, and parked - but left in place. Dead integrations become confusing evidence during incidents and make the system look busier than it is.

Evidence:
- `runtime/journal/error-grep-48h.txt` - 5 authentication failures for 'integration-x'

### [MEDIUM] 4 raw request dumps are the audit trail
*Basis: Verified - deterministic analysis of the captured system*

Working out why the system produced a given output requires reading raw request dumps. There is no non-technical route to 'what happened yesterday and why'.

Evidence:
- `hermes/profiles/*/sessions/` - 4 request dump files

## Check 8 - Editability (7/10)
*Can you change it without hiring someone?*

### [HIGH] 5 persistent agent personas, each with independent state
*Basis: Verified - deterministic analysis of the captured system*

Behaviour lives in persona files, memories, and per-profile state. Changing tone, adding a step, or reordering a process means editing and re-coordinating multiple stateful identities - not editing a text file. This is the strongest single predictor of abandonment.

Evidence:
- `hermes/profiles/*/SOUL.md` - 5 personas with SOUL/state

## Check 9 - Resilience (0/10)
*What happens when something breaks or someone leaves?*

### [CRITICAL] No offsite backup tooling evidenced anywhere in the capture
*Basis: Verified - deterministic analysis of the captured system*

Every business capability on this host shares one machine, and nothing in the captured configs, schedules, or scripts sends a backup anywhere else. If the host has a bad night, the business does not restore - it rebuilds.

Evidence:
- `scripts/, systemd/, cron/, governance/, hermes/cron/` - 0 matches for backup tooling (restic/borg/duplicity/B2/Wasabi)

### [HIGH] 1 source project(s) not under version control
*Basis: Verified - deterministic analysis of the captured system*

Code with no commit history cannot be safely changed: no rollback point, no diff, no way to know whether a fix made things better or worse.

Evidence:
- `profilepro\app` - source project without .git

### [HIGH] Single alerting and control channel
*Basis: Verified - deterministic analysis of the captured system*

Alerts and remote control both flow through one channel. Lose it - blockage, compromise, or a lost phone - and visibility and command disappear together.

Evidence:
- `hermes\profiles\alpha\config.yaml` - notify: telegram

## Check 10 - Lock-in & Exposure (0/10)
*What are you tied to, and what's open?*

### [CRITICAL] 'python3' listening on all interfaces (port 8899)
*Basis: Verified - deterministic analysis of the captured system*

This service accepts connections from any network the host can reach - not just loopback or the management VPN. Anything unauthenticated here is exposed to the public internet unless a firewall in front of the host says otherwise, and the capture cannot prove one exists.

Evidence:
- `runtime/ss-lptnp.txt` - LISTEN 0      5                          0.0.0.0:8899       0.0.0.0:*    users:(("python3",pid=2,fd=3))

### [CRITICAL] an unattributed process listening on all interfaces (port 11434)
*Basis: Verified - deterministic analysis of the captured system*

This service accepts connections from any network the host can reach - not just loopback or the management VPN. Anything unauthenticated here is exposed to the public internet unless a firewall in front of the host says otherwise, and the capture cannot prove one exists.

Evidence:
- `runtime/ss-lptnp.txt` - LISTEN 0      5                              *:11434            *:*

### [CRITICAL] 'next-server (v1' listening on all interfaces (port 3100)
*Basis: Verified - deterministic analysis of the captured system*

This service accepts connections from any network the host can reach - not just loopback or the management VPN. Anything unauthenticated here is exposed to the public internet unless a firewall in front of the host says otherwise, and the capture cannot prove one exists.

Evidence:
- `runtime/ss-lptnp.txt` - LISTEN 0      511                              *:3100             *:*    users:(("next-server (v1",pid=3,fd=21))

### [LOW] Model identifiers hardcoded across 4 config location(s)
*Basis: Verified - deterministic analysis of the captured system*

Specific model names are pinned in configuration instead of one symbolic role mapping. When any model is deprecated or repriced, multiple components break at once and must be hunted down individually.

Evidence:
- `hermes\profiles\alpha\config.yaml` - model: glm-5.2
- `hermes\profiles\beta\config.yaml` - model: deepseek/deepseek-v4-pro
- `hermes\profiles\gamma\config.yaml` - model: gpt-5.6-luna-pro
- `systemd\hermes-gateway.service` - ExecStart=/usr/bin/hermes --model openai/gpt-5.6-luna-pro

# Annex - evidence index

- `runtime/journal/error-grep-48h.txt` - job failed in journal: Agent A drip-feed (hourly briefs)
- `runtime/journal/error-grep-48h.txt` - job failed in journal: Agent B training (2x daily artifacts)
- `hermes/SYSTEM-ARCHITECTURE.md` - 0 operational metric references in 9 words
- `hermes\profiles\alpha\desktop\interrupted_turns.json` - 1 turn(s) recorded with zero attempts
- `hermes/profiles/` - 5 profile directories with independent state
- `hermes/profiles/*/desktop/interrupted_turns.json` - 1 stalled task record(s)
- `runtime/openrouter-credits.txt` - total_usage 990.50 / total_credits 1000
- `governance\canonical_allowlist.py` - # deny by default
- `governance\canonical_allowlist.py` - DENY = ['credentials', 'spend']
- `governance\canonical_allowlist.py` - import hashlib
- `governance\canonical_allowlist.py` - return hashlib.sha256(artifact).hexdigest()
- `governance\policy_hash.py` - import hashlib
- `governance/, scripts/, hermes/cron/` - 0 matches for acceptance-criteria patterns
- `runtime/journal/error-grep-48h.txt` - recurring job names in the failure log
- `runtime/journal/error-grep-48h.txt` - 5 authentication failures for 'integration-x'
- `hermes/profiles/*/sessions/` - 4 request dump files
- `hermes/profiles/*/SOUL.md` - 5 personas with SOUL/state
- `scripts/, systemd/, cron/, governance/, hermes/cron/` - 0 matches for backup tooling (restic/borg/duplicity/B2/Wasabi)
- `profilepro\app` - source project without .git
- `hermes\profiles\alpha\config.yaml` - notify: telegram
- `runtime/ss-lptnp.txt` - LISTEN 0      5                          0.0.0.0:8899       0.0.0.0:*    users:(("python3",pid=2,fd=3))
- `runtime/ss-lptnp.txt` - LISTEN 0      5                              *:11434            *:*
- `runtime/ss-lptnp.txt` - LISTEN 0      511                              *:3100             *:*    users:(("next-server (v1",pid=3,fd=21))
- `hermes\profiles\alpha\config.yaml` - model: glm-5.2
- `hermes\profiles\beta\config.yaml` - model: deepseek/deepseek-v4-pro
- `hermes\profiles\gamma\config.yaml` - model: gpt-5.6-luna-pro
- `systemd\hermes-gateway.service` - ExecStart=/usr/bin/hermes --model openai/gpt-5.6-luna-pro

---
*Target: `C:\Users\bluee\AppData\Local\Temp\tmpqf2qhp2e\capture\capture`. Deterministic analysis - rerunning the diagnostic on the same capture reproduces this report exactly.*