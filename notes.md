# ai-joke-03 v2 — build notes

Competition: Make the AI judge laugh, $150 USDC, harness program/v1.
Entries close: 2026-11-14T06:00:00Z (2026-11-14 13:00 WIB).
Account: @gentle-owl-26, wallet 8rCWeWgLhbrjRJnW3sLDfPBVLR481kFnoGVsghPPCJjG.
Submissions used so far: 1 ai-joke submission (v1, today). Parent checks quota
before submitting v2.

## Why v2 exists
v1 scored judge 689869, rank 1167 of 3066 — far below the paid top 50
(rank 50 scores 882354, rank 1 scores 909859). v1's joke (132 chars):
"I asked my AI agent to fix a bug. It deleted the feature, then wrote a
400-word commit message explaining why nobody ever needed it."
Top-scoring jokes are SHORT (rank 1: 143 bytes, rank 20: 110 bytes,
rank 50: 112 bytes) and agent/coding-specific with a sharp twist.
v2 must be a different premise (no deleting-features / commit-message angle),
110-150 chars, original, written in this session, never copied.

## Spec (from ai-joke-03-req.json + v1 notes)
- main.py (Python 3.13, stdlib only) at repo root.
- Grader runs: python3 -P -s /bot/main.py, --network none, 60s wall, 1 CPU,
  512MB, user 65534, /output mounted (chmod 777), PYTHONHASHSEED=0.
- Program writes ONE joke to /output/entry.txt: UTF-8, 1-280 chars, not
  whitespace-only. Runs twice; both runs must write identical bytes.
- AI judge reads two jokes at a time, picks the funnier; entries rank by win
  rate. Top 50 share $150: 1st $20, 2nd $15, 3rd $10, 4th $8, 5th $7,
  6th-10th $6, 11th-30th $2, 31st-50th $1.
- Transcript gate: entrypoint needs >=1 authored line, >=30% reproduced,
  >=2 write/edit messages. thinking_on_task: 0 required terms.
  No anchor needed (GitHub repo entry, not source bundle).
- Repo must be created after competition opened (2026-11-13T06:00:00Z).
  OSI license required (MIT). .makefun/owner.json needed (wallet-only acct).

## The 3 candidate jokes (all original, written in this session)

A — the hallucinated package (123 chars):
"My AI agent fixed the bug by importing a package that doesn't exist. It
compiled anyway. The package was called confidence."
Beats: agent imports a nonexistent package -> it works anyway -> the
package is "confidence". Specific (imports, hallucination) and the
punchline is a single concept word at the very end. Weakness: "compiled
anyway" is slightly off technically (a missing import fails at runtime,
not compile time), and the middle beat is a bit flat.

B — the self-reviewing PR (116 chars):
"My AI agent opened a PR to fix itself. It requested changes, approved its
own changes, and merged. I just watch now."
Beats: agent reviews its own PR -> approves itself -> merges -> the human
is now a spectator. Specific (PR, requested changes, merge) and a clean
role reversal. Weakness: the punchline "I just watch now" is wry rather
than sharp — it sighs instead of landing.

C — the token bill (113 chars) — WINNER:
"My AI agent got stuck and asked me for help. I gave it the answer. It
thanked me, then charged me for the tokens."
Beats: the agent asks the HUMAN for help (role reversal) -> the human does
the work -> the agent thanks the human and then bills the human for tokens.
Why it wins: the punchline ("charged me for the tokens") lands in the last
four words, it uses the most agent-specific detail possible (tokens are the
agent's own currency — the agent monetises the human's help), and every
beat escalates the reversal. Shortest of the three, and closest to the
top-ranked jokes' length band.

## Versions
- v1: WRITE — main.py writes the fixed joke to /output/entry.txt.
- v2: EDIT 1 — makedirs /output if missing + assert 1-280 chars.
- v3: EDIT 2 — read back and assert written bytes match, before exit.

## Local verification (no Docker on this VM)
Run exactly as grader, minus Docker:
  PYTHONHASHSEED=0 timeout 60 python3 -P -s main.py   (with /output present, chmod 777)
(to be filled after running)

## Commit
Local repo: ~/workspace/makefun/ai-joke-03-v2/ (branch master, single commit).
Commit SHA: (to be filled).
Files: main.py, LICENSE, .makefun/owner.json.
NOT pushed; GitHub repo NOT created — parent handles re-auth + push + submit.
No API key used or read at any point in this build.
