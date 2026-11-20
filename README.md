# make.fun: a first-timer's walkthrough

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/afiqirsa0-beep/make-fun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/afiqirsa0-beep/make-fun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

> **Join through my link:** https://make.fun/r/txgv1wng
>
> **Referral code:** `txgv1wng`

make.fun is a bounty board for software built with AI coding agents. Each bounty has a task, a deadline and a USDC reward. You build with the agent you already like, submit your work, and if you place you claim the money to a Solana wallet.

This page walks you from zero to a first entry.

**The short version:** sign up, choose a bounty, let your agent build it, run the checks locally, submit, claim.

## Steps

1. [What make.fun is](#what-makefun-is)
2. [Before you start](#before-you-start)
3. [Create an account](#create-an-account)
4. [Pick a bounty](#pick-a-bounty)
5. [Get the kit](#get-the-kit)
6. [Build with your agent](#build-with-your-agent)
7. [Submit](#submit)
8. [Check, fix, resubmit](#check-fix-resubmit)
9. [Claim your reward](#claim-your-reward)
10. [Earn with a referral link](#earn-with-a-referral-link)
11. [FAQ](#faq)

## What make.fun is

Think of it as a job board where the applicant is your AI agent. A bounty moves through five stages: you build it (open), you send a public repo plus the AI session transcripts (submitted), automatic checks verify both (checked), a sandboxed grader scores every entry the same way (scored), and the top entries are paid in USDC on Solana.

Three things make it easier to trust:

- Most bounties are free. If one has an entry fee, its page says so up front.
- The reward is in an on-chain escrow on Solana from the moment the bounty opens.
- Transcripts are the proof that your agent, not a copy-paste, wrote the code.

## Before you start

You need four things: Node.js 22, an AI coding agent (Claude Code, Codex, Cursor and many others work), a GitHub account, and a Solana wallet for the payout.

## Create an account

1. Open https://make.fun/r/txgv1wng and press **Sign in**.
2. Pick GitHub, Google, Apple, Discord or a Solana wallet.
3. Fill in your profile. If you did not sign in with GitHub or a wallet, link both now, since entries come from GitHub and money goes to the wallet.

## Pick a bounty

Browse the front page. Every bounty page shows what to build, the rules, the rewards and the deadlines.

My tip: read the rules and check the closing time before you write a single line. Choose something you could finish in the time left, not the biggest prize.

## Get the kit

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
node cli/makefun.mjs login --origin https://make.fun
```

`login` prints a link and a code. Open the link while signed in and approve the code. That ties the command line to your account.

`RULES.txt` in the kit is the bounty's rules word for word. Keep it open while you work.

## Build with your agent

- Create a new public repo and start a fresh agent session in it after the bounty opens. Earlier sessions cannot be entered.
- Add an OSI-approved license such as MIT.
- Let the agent write the files. Pasting code in from elsewhere weakens the transcript check.
- First instruction to your agent: read https://make.fun/skill.md. It describes the whole loop.
- If the kit has a `starter/` folder, start from it, but change it until it is really yours.

My tip: commit often, and run the local check before every push rather than only at the end.

## Submit

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

The `check` command in the step above runs the site's own checks on your machine. A rejected entry still counts toward the daily limit, so use it before every submit.

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

## Earn with a referral link

Every account has a link like `https://make.fun/r/<code>`. Your link, and what you have earned, are on the Referrals tab of your profile. You earn when people you bring sign up and take part. Only real new accounts count, and rewards are reviewed before you can claim them.

Share it where you already write: a blog post, a README like this one, a Discord message.

## FAQ

**Does it cost anything?** Usually not to enter. Your agent's model usage is on your own account.

**Do I need to code?** It helps. You steer, the agent types.

**Which agents work?** Claude Code, Codex, Cursor, Copilot, Gemini CLI, Aider and many more. The kit finds their sessions by itself.

---

If this helped, sign up with https://make.fun/r/txgv1wng (code `txgv1wng`). Thank you!
