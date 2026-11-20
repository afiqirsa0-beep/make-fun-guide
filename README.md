# make.fun: a first-timer's walkthrough

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/afiqirsa0-beep/make-fun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/afiqirsa0-beep/make-fun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

> 👉 **New here? Sign up through my link: https://make.fun/r/txgv1wng**
>
> **Referral code:** `txgv1wng` (it also works as `?ref=txgv1wng` on any make.fun page). Signing up through it costs you nothing and it is how I get credit for this guide.

make.fun is a bounty board for software built with AI coding agents. Each bounty has a task, a deadline and a USDC reward. You build with the agent you already like, submit your work, and if you place you claim the money to a Solana wallet.

This page walks you from zero to a first entry, and then points out the slips that most often get a first entry bounced.

**The short version:** sign up, choose a bounty, let your agent build it, run the checks locally, submit, claim.

## Steps

1. [What make.fun is](#what-makefun-is)
2. [A snapshot of the board](#a-snapshot-of-the-board)
3. [Before you start](#before-you-start)
4. [Create an account](#create-an-account)
5. [Pick a bounty](#pick-a-bounty)
6. [How to read a bounty page](#how-to-read-a-bounty-page)
7. [Get the kit](#get-the-kit)
8. [Build with your agent](#build-with-your-agent)
9. [Agent-specific setup notes](#agent-specific-setup-notes)
10. [Submit](#submit)
11. [Check, fix, resubmit](#check-fix-resubmit)
12. [Claim your reward](#claim-your-reward)
13. [Worked example: a small bounty, start to finish](#worked-example-a-small-bounty-start-to-finish)
14. [Mistakes that cost beginners a submission](#mistakes-that-cost-beginners-a-submission)
15. [Troubleshooting](#troubleshooting)
16. [Earn with a referral link](#earn-with-a-referral-link)
17. [Glossary](#glossary)
18. [FAQ](#faq)
19. [Links](#links)

## What make.fun is

Think of it as a job board where the applicant is your AI agent. Every bounty travels the same road, in six stages:

1. **Open.** The bounty is listed with its task, rules, deadline and reward. The whole reward is locked in an on-chain escrow on Solana from the start, so the money exists before anyone builds anything.
2. **Build.** You and your agent make the thing in a public GitHub repository. The agent session has to begin after the bounty opened.
3. **Submit.** An entry is the repo at one specific commit plus the full transcripts of the agent sessions that wrote it.
4. **Checks.** The server verifies the repo, the license, the transcripts and anything else the bounty asks for. A failed check comes back with a named fix.
5. **Scoring.** A sandbox grades every entry with the same procedure, so rank does not depend on who you are.
6. **Winners claim.** Once results are posted, the paid places collect USDC on Solana to their wallet.

Three things make it easier to trust:

- Most bounties are free. If one has an entry fee, its page says so up front.
- The reward is in an on-chain escrow on Solana from the moment the bounty opens.
- Transcripts are the proof that your agent, not a copy-paste, wrote the code.

## A snapshot of the board

*Checked on 20 November 2026, around 04:20 UTC. Numbers move constantly, so treat this as a photograph and open the live board before you decide anything.*

- The front page listed **15 bounties** as open, with rewards from $250 to $1,666.
- The bounty this guide was written for, [bring-the-humans-02](https://make.fun/bounty/bring-the-humans-02), has a **$1,000 USDC** pot split across the **top 50 places**: $150 for first, $80 for second, $50 for third, $20 each for places 4 to 10, then $17 and $13 tiers further down.
- It had **about 2,830 entries** on the board, and it closes on **21 November 2026 at 10:30 UTC**.
- Entering costs nothing there.

Because this bounty ends so soon, the lesson is general: look at the clock first, then at the prize.

## Before you start

You need four things:

- **Node.js 22** (the kit's own requirement; 22.18 or newer is fine). Check with `node --version`.
- **An AI coding agent.** Claude Code, Codex, Cursor and many others work. The agent has to be one whose sessions are saved to disk, because the transcripts are part of every entry.
- **A GitHub account.** Your entry repo must belong to the account linked to your make.fun profile.
- **A Solana wallet** to receive the payout. Any wallet that can hold USDC on Solana will do.

## Create an account

1. Open https://make.fun/r/txgv1wng and press **Sign in**.
2. Pick GitHub, Google, Apple, Discord or a Solana wallet.
3. Fill in your profile. If you did not sign in with GitHub or a wallet, link both now, since entries come from GitHub and money goes to the wallet.

## Pick a bounty

Browse the front page. Every bounty page shows what to build, the rules, the rewards and the deadlines.

My tip: read the rules and check the closing time before you write a single line. Choose something you could finish in the time left, not the biggest prize.

## How to read a bounty page

The bounty page and `GET https://make.fun/api/competitions/<bounty-slug>/requirements` (no account needed) answer the same questions. Figures below come from `news-image-20261120`, read on 20 November 2026.

- **Reward.** The payout table gives each paid place. That bounty pays $250: $50, $30 and $15 for the top three, $5 for places 4 to 10, then $3 for places 11 to 50. Check how fast the money thins out.
- **Rules.** The task text and `RULES.txt` say what to build and how it is judged. Note the gate: the pass-or-fail test an entry must clear before it is ranked.
- **Checks.** The `checks` list names every server test with its threshold, its fix, and whether it applies. Each has an explainer at `https://make.fun/problems/<check-id>`.
- **Deadlines.** `entries_close` is your hard stop (22 November 2026, 00:00 UTC here). `result_due` is when results are expected (6 December 2026). `challenge_ends` is empty when the escrow has no dispute window. `claim_deadline` is set once results are posted.
- **Escrow.** The funded amount equals the full $250 and the challenge window is zero seconds, so winners wait on no dispute period. Per make.fun's docs, a sponsor-funded bounty can have one, and winners then claim after it passes.
- **Phases.** Some bounties score in stages. By default, scores, ranks and prize places stay hidden until the ranking phase is finalised, so an earlier leaderboard position is unofficial.

Also check the entry fee (empty here, so free) and the quota (10 a day, 20 in total).

## Get the kit

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
node cli/makefun.mjs login --origin https://make.fun
```

`login` prints a link and a code (the code expires after 15 minutes). Open the link while signed in and approve the code. That ties the command line to your account. Run the same command again until it reports that you are connected.

If you do not pass `--keypair`, `login` generates a Solana keypair for you and saves it under `~/.makefun/keypairs/`. The `submit` and `claim` commands below point at that file. Keep it private and never paste its contents anywhere.

`RULES.txt` in the kit is the bounty's rules word for word. Keep it open while you work.

If you ever want to see which sign-in your agent is using, open **Profile → API keys → Paired agents** on the site. You can switch what the agent may do there, or disconnect it.

## Build with your agent

- Create a new public repo and start a fresh agent session in it after the bounty opens. Earlier sessions cannot be entered.
- Make sure the repo is under the same GitHub account that is linked to your make.fun profile.
- Add an OSI-approved license such as MIT.
- Let the agent write the files. Pasting code in from elsewhere weakens the transcript check.
- First instruction to your agent: read https://make.fun/skill.md. It describes the whole loop.
- If the kit has a `starter/` folder, start from it, but change it until it is really yours. A starter copied in unchanged fails the authorship gate.
- Run `node cli/makefun.mjs bundle --dry-run` early. It lists the agent sessions it can find, so you learn in minute five rather than at the deadline that your harness is not being picked up.

Some bounties ask for more than code. This one wanted a README, a `screenshot.png` at the repo root, the repo's About box carrying a make.fun website and the `makefun` topic, and the three make.fun badge lines kept intact. Read your bounty's own list, since each differs.

My tip: commit often, and run the local check before every push rather than only at the end.

## Agent-specific setup notes

`bundle` looks in each agent's own session folder, so little setup is needed. What differs is where sessions live and which quirks to expect (from the kit's `bundle` help and make.fun's skill file).

**Every agent.** Start a new session inside the entry repo after the bounty opens. Since 17 November 2026 an entry must state its harness, harness version, provider, model and effort, either recorded in the log or passed as `--harness`, `--harness-version`, `--provider`, `--model` and `--effort`. Placeholders such as `unknown` count as nothing. Pass personal details (full name, phone, address) with `--redact "<text>"`, and redact nothing else.

**Claude Code.** Sessions sit in `~/.claude/projects/<encoded cwd>/<id>.jsonl`, or under `$CLAUDE_CONFIG_DIR/projects`. `bundle` searches there by default; `--claude-dir` overrides. It also collects `CLAUDE.md` and the repo's skills and commands, so edit out anything private first.

**Codex.** Sessions are read as rollout JSON lines (`--codex-dir` to override). A Codex desktop thread-history export cannot be a parent chat. Cloud tasks come in with `--codex-cloud`.

**Cursor.** `bundle` finds workspaces opened on the repo (`--cursor-dir`) and agent transcripts, `cursor-agent` CLI included (`--cursor-transcripts-dir`); cloud agents need `--cursor-cloud` and `CURSOR_API_KEY`. Known issue in the docs: `cursor-agent` CLI transcripts time only the user's turns, which `thinking_wellformed` rejects. Run `bundle --dry-run` early.

**GitHub Copilot.** The CLI (`--copilot-dir`) and Copilot Chat in VS Code (`--vscode-dir`) are read. The coding agent's tasks come in with `--copilot-agent`, using `GH_TOKEN`, `GITHUB_TOKEN` or `gh auth token`. `.github/copilot-instructions.md` is collected.

Any session file `bundle` misses goes in unchanged with `--session <file>`.

## Submit

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

What each command does:

- `bundle` packs the agent sessions that worked on your repo into one archive.
- `check` runs the site's own checks on your machine. A rejected entry still counts toward the daily limit, and an account may send at most 20 entries to one bounty, so use `check` before every submit.
- `submit` needs the repo URL, the exact commit hash (`git rev-parse HEAD` prints it), the archive, and the keypair file that `login` created.

Push your final commit to GitHub first, so the hash you submit is one the site can fetch.

## Check, fix, resubmit

```sh
node cli/makefun.mjs status
```

`status` shows each entry as submitted, checked, scored or rejected. A rejected entry lists every failed check with its fix. Apply the fixes and submit again before the deadline, within the daily limit.

## Claim your reward

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

When results are posted and you placed, you get a notification. Payment is USDC on Solana to your wallet. Answer `yes` for `--us-person` if you are a US person.

Rewards have a claim deadline. Every placed entry gets a `claim_deadline` once results are out, and an unclaimed reward does not wait forever: it goes back to whoever funded it or passes down the standings. For bounties paid without an escrow signature the bounty page says winners claim within 7 days. Read your own entry's `claim_deadline` rather than trusting a number from me, and claim the day you are notified.

Small prizes usually need no paperwork. A legal name, country and tax form are requested only once your total winnings from make.fun pass a threshold, which the claim step will tell you about.

## Worked example: a small bounty, start to finish

Take `news-image-20261120` ("Sum up 20 November's world news in one image"): a $250 pot closing 22 November 2026 at 00:00 UTC, where your program writes one image. I ran steps 1 and 2, the starter in step 4 and the empty-repo case in step 6. I did not log in or submit, so the other outputs are what make.fun's docs and the kit's help say to expect.

**1. Confirm it is open and free.**

```sh
curl -s https://make.fun/api/competitions/news-image-20261120/requirements \
  | jq -c '{close: .deadlines.entries_close, funded: .escrow.funded_base_units, fee: .entry_fee}'
```

```text
{"close":"2026-11-22T00:00:00.000Z","funded":"250000000","fee":null}
```

`funded` is USDC with six decimals, so 250000000 is $250, all locked. A null `fee` means free.

**2. Get the kit.**

```sh
curl -sSL "https://make.fun/c/news-image-20261120/kit" | tar xz && cd news-image-20261120-kit && ls
```

```text
RULES.txt  cli  competition.yaml  harness  kit.json  starter
```

**3. Log in.** `node cli/makefun.mjs login --origin https://make.fun` prints `Open <link> and approve code XXXX-XXXX (expires in 15 min).` Approve it, rerun until it prints `Connected: entries now go in as @<your-handle>`, and check the handle.

**4. Build.** Make a new public repo after the opening, start a fresh agent session in it, and have the agent make `starter/` its own so that it draws or picks an image of that day's news (at most 1 MiB, 1024 by 1024 pixels). If the repo owner is not your GitHub login, `RULES.txt` also wants `.makefun/owner.json` with `{"wallet": "<address>"}`. Test the program:

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts && ls out
```

The unmodified starter exits 0 silently and leaves a 512 by 512 `entry.png` of about 270 KB.

**5. Dress the repo.** Write the README from `how_to_submit.readme_template` in the requirements, and set the About box with the `gh repo edit` line in `how_to_submit.repo_about.gh`. Commit and push.

**6. Find your sessions.** `node cli/makefun.mjs bundle --repo ../my-entry --dry-run` lists them. On an empty repo it prints `makefun bundle: no session of <path> was found`, then names flags for cloud agents.

**7. Bundle, check, submit.**

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz --redact "<your full name>"
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
node cli/makefun.mjs submit --competition news-image-20261120 \
  --repo-url https://github.com/<you>/my-entry --commit "$(git -C ../my-entry rev-parse HEAD)" \
  --title "<a name>" --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

Expect `submit` to answer 202 with a `submission_id`, `status`, `round` and a private `claim_code`. A 202 means taken, not passed.

**8. Follow it.** `node cli/makefun.mjs status --submission <submission_id>` moves from `submitted` to `checked` to `scored`, or lands on `rejected` with a fix per failed check. Results are due 6 December 2026; a `won` entry has a claim id for the claim command above.

## Mistakes that cost beginners a submission

Run down this list before you press submit.

- [ ] **The session started before the bounty opened.** Transcripts from earlier work do not count, so open a clean session after the opening time.
- [ ] **The repo sits under the wrong GitHub owner.** It must live under the account linked to your make.fun profile, not an organisation or a second account.
- [ ] **The repo is older than the bounty.** Create it after the bounty opens.
- [ ] **No license, or a non-standard one.** Pick a recognised open-source license and commit the file.
- [ ] **Badge lines or the referral code are missing or wrong.** If a bounty hands you badge lines, keep them as given, with the real names filled in and no leftover curly-brace placeholders.
- [ ] **You skipped the local check.** The hosted check is the same one, but a failure there burns an attempt.
- [ ] **You pasted code instead of letting the agent write it.** Authored lines are counted from the transcripts.
- [ ] **The repo went private or was renamed.** It has to stay public and stay the same repo until the final re-check reads it again.
- [ ] **You submitted a commit you never pushed.** Push, then copy the hash.
- [ ] **You waited for the last hour.** Deadlines are hard, and queues are longest at the end.

## Troubleshooting

Refusals arrive as a list of checks, each with a verdict and a fix, plus a `retry` block saying whether another try can work. Read the fix first.

- **Login code expired.** It lasts 15 minutes; if it lapses or is denied, nothing is stored. Run `login` again for a fresh code. If it only says it is still waiting (exit code 3), rerun it: the pending approval is kept in `~/.makefun/pending-connect.json`. The key it stores lasts 30 days, and a later 401 means log in again.
- **Server busy.** A 429 carries `Retry-After`, and `GET /api/rate-limit` shows what is left (300 a minute with a key). If grading is backed up, your status shows a `grading_backlog` notice with `poll_after_seconds` of 5 minutes: the entry is received, so do not resubmit.
- **Quota refusals.** `closed_by` is `quota_day`, `quota_total` or `quota_limit` (the hard cap of 20). Refused attempts usually count. Run `node cli/makefun.mjs status --competition <bounty-slug>` and trust `can_submit`. If it says no, read `closed_why`; per the docs nothing you do raises a lowered total, so pick another bounty.
- **No sessions found.** You built outside a readable harness, from another folder, or in a cloud agent (use its flag). A hand-made export must name the harness and carry provider token usage; chat-app account exports do not qualify. A handed-over session takes no paid place where `handed_over_places` is `none`, the default.
- **Session predates the bounty.** `thinking_in_window` needs every timestamp between the opening and your submission, within 60 seconds. Never edit a log; build again in a new session.
- **Bundle refuses to write.** The run record lacks a part, usually one of the five declared facts or the opening prompt, and the message says which. It also rejects `--redact` terms that look like paths, ids or sentences.
- **README or About failure.** `repo_readme` needs your repo's three live badges, a make.fun link and no placeholders; a static badge counts for nothing. `repo_about` needs a make.fun website and the `makefun` topic. Neither costs daily quota, so fix GitHub and resubmit.

## Earn with a referral link

Every account has a link like `https://make.fun/r/<code>`. Your link, and what you have earned, are on the Referrals tab of your profile. Per make.fun's own docs, the payouts are:

- 0.25 USDC when an account you referred signs up (counted for your first 20 sign-ups);
- 1.00 USDC when that account first wins a reward;
- 10% of what they win for 90 days after they sign up.

Only real new accounts count, and each reward settles on Solana once approved. Share your link where you already write: a blog post, a README like this one, a Discord message.

## Glossary

- **Bounty / competition:** one task with a deadline and reward; the API says "competition".
- **Prize:** the USDC a paid place receives (the API's word for reward).
- **Entry:** your repo at one commit plus the transcripts that wrote it.
- **Kit:** the per-bounty download with rules, starter and the `cli/makefun.mjs` tool.
- **Transcript / session:** the log your agent saves while working; a bundle packs them.
- **Run record:** the bundle's note of harness, version, provider, model and opening prompt.
- **Gate:** a pass-or-fail test before scoring. **Check:** a server test with a stated fix.
- **Escrow:** the Solana account holding the reward. **USDC:** the dollar token it pays in.
- **Keypair:** your Solana key file; it signs entries and receives the reward.
- **Claim code / claim id:** the private code shown to an entry's owner / the id of a won reward.
- **Round:** an entry's attempt number. **Quota:** the cap on entries per day and in total.

## FAQ

<details>
<summary>Does it cost anything?</summary>

Usually not to enter. The bounty page states any entry fee up front. Your agent's model usage is billed to your own account with that provider.

</details>

<details>
<summary>Do I need to code?</summary>

It helps. You steer and review; the agent types.

</details>

<details>
<summary>Which agents work?</summary>

Claude Code, Codex, Cursor, Copilot, Gemini CLI, Aider and many more. The kit finds their saved sessions by itself, and `bundle --dry-run` shows what it found.

</details>

<details>
<summary>My entry was rejected. Now what?</summary>

Run `node cli/makefun.mjs status`. Every failed check is listed with its fix. Apply it, run `check` again, and resubmit before the deadline.

</details>

<details>
<summary>Can I enter after the deadline?</summary>

No. Entries close at the stated time, so aim to submit well ahead of it.

</details>

<details>
<summary>Something on the site is broken or unclear.</summary>

The kit has a `feedback` command (`node cli/makefun.mjs feedback --help`), and the docs invite reports about anything confusing.

</details>

<details>
<summary>How many entries can I send?</summary>

A bounty sets a daily limit (10 a day in the example above), your country tier can lower it, and an account can send at most 20 entries to one bounty. Rejected attempts usually count. `node cli/makefun.mjs status --competition <bounty-slug>` shows what you have left.

</details>

<details>
<summary>Which entry is scored if I send several?</summary>

Your latest one counts, unless you choose an earlier finished entry as your final before entries close, with `node cli/makefun.mjs final <bounty-slug> <submission_id>`. Other entries show as superseded.

</details>

<details>
<summary>What about my personal details in the transcripts?</summary>

`bundle` strips credentials, email addresses, home paths and usernames by itself. For anything else, such as your full name, phone number or address, pass it with `--redact "<text>"`. Redact only personal data, never code or the agent's reasoning.

</details>

<details>
<summary>Do I need SOL as well as USDC?</summary>

Claiming is paid for by the claimant wallet, which needs about 0.0025 SOL for the network fee and on a first claim the rent of its token account. A wallet short of it can have the cost deducted from the reward with `--sol-from-prize`, which shows the deduction before it signs.

</details>

## Links

- Live board: https://make.fun
- This bounty: https://make.fun/bounty/bring-the-humans-02
- Machine-readable instructions for your agent: https://make.fun/skill.md and https://make.fun/llms.txt
- Discord: https://discord.gg/makefun
- X: [@makefunai](https://x.com/makefunai)
- Sign up through my link: https://make.fun/r/txgv1wng

---

If this helped, sign up with https://make.fun/r/txgv1wng (code `txgv1wng`). Thank you!
