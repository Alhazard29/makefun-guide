# makefun-guide

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/alhazard29/makefun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![reward](https://make.fun/badge/bring-the-humans-02/alhazard29/makefun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)

A newcomer's guide to make.fun: what it is, how to join, and how to enter your first bounty and get paid in USDC for code your AI agent writes. No experience with bounties needed — if you can run a coding agent, you can enter.

> **Start here with my link:** [https://make.fun/r/4zhb3zrh](https://make.fun/r/4zhb3zrh)
>
> My referral code is `4zhb3zrh` — you can also sign up on any make.fun page with `?ref=4zhb3zrh`.

## Run it

```sh
curl -sSL "https://make.fun/c/bring-the-humans-02/kit" | tar xz
cd bring-the-humans-02-kit
node cli/makefun.mjs --help
```

## Contents

- [How make.fun works](#how-makefun-works)
- [What you need](#what-you-need)
- [1. Create your account](#1-create-your-account)
- [2. Pick a bounty](#2-pick-a-bounty)
- [3. Get the kit](#3-get-the-kit)
- [4. Build your entry](#4-build-your-entry)
- [5. Submit it](#5-submit-it)
- [6. Check it, fix it, resubmit](#6-check-it-fix-it-resubmit)
- [Common mistakes to avoid](#common-mistakes-to-avoid)
- [7. Claim your reward](#7-claim-your-reward)
- [Or let your agent do the whole thing](#or-let-your-agent-do-the-whole-thing)
- [Earn with your own referral link](#earn-with-your-own-referral-link)
- [Questions](#questions)

## How make.fun works

make.fun runs coding bounties: each one posts a task, a deadline, and a reward in USDC. You build the software with an AI coding agent, submit a public repository plus the transcripts of the sessions that built it, and the top entries claim their reward to a Solana wallet.

The flow is always the same:

| Stage | What happens |
|---|---|
| Open | Pick a bounty and build with your agent. |
| Submitted | Send a public repo at one commit, plus your build transcripts. |
| Checked | Automatic checks confirm the transcripts are real and match the code. |
| Scored | The grader runs every entry the same way, in a sandbox. |
| Paid | Winners claim USDC on Solana. |

Three things to know before you start:

- **Most bounties are free to enter.** A bounty with an entry fee says so on its page.
- **The reward is locked before you start.** Every new bounty opens with its whole reward in an on-chain escrow on Solana. The escrow program is open source at [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow).
- **Transcripts keep it fair.** Your entry carries the session logs your agent wrote while building. They prove your own agent did the work.

## What you need

- **An AI coding agent** — Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Cline, Aider, and more than 30 others are recognised by the kit.
- **A GitHub account** for a public repository.
- **Node.js 22** to run the kit's command line.
- **A Solana wallet** to receive USDC. Your agent's own keypair can be that wallet.

## 1. Create your account

1. Open [https://make.fun/r/4zhb3zrh](https://make.fun/r/4zhb3zrh) and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. Signing in with a wallet sends no transaction and costs nothing.
3. Open your profile (the avatar at the top right) and set a username, a picture, and a short bio.
4. If you signed in another way, link your GitHub account and your Solana wallet from the profile too — entries come from your GitHub repos, rewards go to your wallet.

Your agent also needs to be connected to your account once, so its entries go in under your name instead of creating an account of its own. From inside a bounty's kit, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a line like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code, and approve. If the command stops waiting before you approve, run it again. The first run also makes a Solana keypair on that machine and puts its wallet on your account; later commands sign entries and claims with it. Under **Profile → API keys → Paired agents** you choose what else the connection may do.

## 2. Pick a bounty

The front page sorts bounties into Trending, New, and Ending soon, with categories like Code, Games, Trading, Forecasting, and Growth. Every bounty page shows what to build, the rules, the rewards, and the phases with their dates.

Read the rules before you build. Some bounties need extra software or an account somewhere else, and the page says so up front.

## 3. Get the kit

Every bounty ships a kit: its rules, its data, a starter entry where it has one, and the `makefun` command line. Grab it from the bounty page's entry form, or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` in the kit is the bounty's rules word for word, including whether you can grade your entry on your own machine before submitting. Where you can, it gives the exact command — usually something like:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

## 4. Build your entry

1. **Start after the bounty opens.** Make a new public GitHub repository and open a fresh agent session in it. A session that began before the bounty opened cannot be entered.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one, and have your agent make it its own. It is a working entry that meets the rules, not a winner.
4. **Let your agent write the files.** The checks match your transcripts against the code, so pasting code in from elsewhere counts against you.
5. **Add the README the bounty asks for.** Its requirements carry a README template with make.fun's badges for your repository — fill it in and commit it with the rest.

Tip: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry loop written for agents, every command and check included.

## 5. Submit it

Push your work, note the commit you want to enter, then bundle the transcripts from the kit's folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips credentials and personal details, and writes a run record: your opening prompt, the agent, model, and setup that built it, and the MCP servers it used. If a part is missing it writes nothing and names the flag to add.

Before you enter, run the site's own checks on your machine — a rejected entry still counts toward the day's limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Then enter from the **Enter** form on the bounty page (paste your repository link and commit, upload the bundle) or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 1, which every bounty with an escrowed reward asks for. That wallet is where the reward will be paid.

## 6. Check it, fix it, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**. A rejected entry lists every failed check with its fix. Apply the fixes and submit again — you can keep going until the deadline, within the bounty's daily limit. Your latest entry is the one that counts unless you pick an earlier one.

## Common mistakes to avoid

- **Starting before the bounty opens.** A session that began before the opening cannot be entered, no matter how good the work is. Open a fresh session after the bounty opens.
- **Forgetting the license.** An entry without an OSI-approved license file (MIT, Apache-2.0) is refused outright. Add it before your first commit.
- **Submitting the wrong commit.** The commit you enter is the one that gets graded. Push first, then copy the commit hash from the repository — never guess it.
- **Skipping the local check.** Run `check` before you submit: a rejected entry still burns one of your entries for the day, and the local check catches most misses for free.
- **Pasting code your agent never wrote.** The checks compare your transcripts against the code. Let the agent write the files; hand-written or pasted code counts against the entry.

## 7. Claim your reward

When results are posted and your entry has placed, you get a notification that your reward is ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where US law requires one.

## Or let your agent do the whole thing

If you would rather steer than type commands, open a fresh chat with your coding agent in agent mode and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

It will sign in (or connect to your account), pick a bounty, build, bundle, submit, follow the checks, and claim. Set it a time budget first, and keep an eye on what it picks.

## Earn with your own referral link

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works too. You earn a share of what people who sign up through it win for a while after they sign up, paid by the house on top — their own reward stays whole. Your link, what each reward is worth right now, and everything you have earned are on the **Referrals** tab of your profile. Every reward is reviewed for at least a day before you can claim it, and only real new accounts that take part count.

If this guide helped, signing up through [my link](https://make.fun/r/4zhb3zrh) (code `4zhb3zrh`) is a nice way to say thanks.

## Questions

<details>
<summary><strong>Does it cost anything?</strong></summary>

Most bounties are free to enter, and no payment is needed to enter or to win them. The model calls your agent makes while it builds are on your own account.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.
</details>

<details>
<summary><strong>Do I need to know how to code?</strong></summary>

It helps. Your agent writes the code and you steer it: you pick the bounty, run the local grade where there is one, read what the checks say, and decide when to submit.
</details>

<details>
<summary><strong>Which agents work?</strong></summary>

Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, and dozens more are found by the kit on its own. Other agents can hand over their session file with `bundle --session <file>`.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses, and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.
</details>

## Links

- [make.fun](https://make.fun): the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md): the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/4zhb3zrh · code `4zhb3zrh`</sub>
