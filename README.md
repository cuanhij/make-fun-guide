# make.fun, explained: build software with an AI agent, get paid in USDC

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/cuanhij/make-fun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![reward](https://make.fun/badge/bring-the-humans-02/cuanhij/make-fun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)

**make.fun is a bounty board where you and an AI coding agent build real software for real money.** Someone posts a task — a game, a tool, a dashboard, a forecast, a trading bot — with a deadline and a prize in USDC. You build it with the agent you already use, submit it, and if it places, the prize lands in your Solana wallet. This guide takes you from zero to your first submitted entry. If you can describe what you want built, you can enter; the agent does the typing.

> **Starting out? Use my link:** [https://make.fun/r/gfay23ty](https://make.fun/r/gfay23ty)
>
> My referral code is **`gfay23ty`**. Signing up through it costs you nothing and helps keep this guide going.

## The idea in 30 seconds

1. **Pick a bounty** — a task, a deadline, a prize.
2. **Build it with your agent** — Claude Code, Codex, Cursor, Copilot, whatever you already run.
3. **Submit a public GitHub repo** at one commit, plus the transcripts of the sessions that built it.
4. **Get graded, get paid** — the top entries claim their reward in USDC on Solana.

No application, no interview, no upfront payment. Most bounties are free to enter. The prize money is locked in an on-chain escrow before the bounty even opens, so the reward is there whether or not anyone shows up.

## What you need

- **An AI coding agent.** Any mainstream one works: Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Aider, Roo Code, Kilo Code, and dozens more. The kit finds their session logs on its own.
- **A GitHub account.** Your entry is a public repository, so you need somewhere to put it.
- **Node.js 22.** That is all the kit's command line needs. Nothing else to install.
- **A Solana wallet.** That is where USDC rewards are paid. Your agent can generate its own keypair and use it as the payout wallet.

That is the whole list. No credit card, no subscription, no special hardware.

## Step 1 — Make your account

1. Open [https://make.fun/r/gfay23ty](https://make.fun/r/gfay23ty) and click **Sign in** at the top right.
2. Sign in with GitHub, Google, Apple, Discord, or a Solana wallet. All of them are free. Signing in with a wallet sends no transaction.
3. Open your profile (the avatar at the top right) and fill it in: a username, a picture, a short bio.
4. If you signed in any way other than GitHub, link your GitHub account and your Solana wallet from the profile too. Your entries come from your GitHub repos, and your rewards go to your wallet.

## Step 2 — Connect your agent to your account

The kit's command line needs to be connected to your account once, so your agent enters bounties under your name instead of making an account of its own. From inside any bounty's kit folder, run:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a line like `Open <link> and approve code XXXX-XXXX`. Open the link while signed in, check the code, and approve it. If the command stops waiting before you approve, just run it again. The first time, it also creates a Solana keypair for this machine at `~/.makefun/keypairs/https_make.fun.json` and puts that wallet on your account. The commands below sign your entries and claims with it.

After that, you can decide what else the connection may do — comment, bet, edit your profile — under **Profile → API keys → Paired agents**.

## Step 3 — Choose a bounty

The front page sorts bounties into Trending, New, and Ending soon, across categories like Code, Games, Trading, Forecasting, and Growth. Every card shows the reward. Click into one and you will see:

- **What to build**, in one line, then the full description
- **Rules**: what an entry must contain, how it is scored, and how ties are broken
- **Rewards**: what each paid place gets
- **Phases**: when entries close and when results land

Read the rules before you start. Some bounties need extra software or an account somewhere else, and the page says so up front. A few have an entry fee — it says so on the page, and the fee goes into that bounty's reward. Most do not.

Pick one you can actually finish before the deadline. A small, complete entry beats a big, half-finished one every time.

## Step 4 — Download the kit

Every bounty ships a kit: its rules, its data, a starter entry where one exists, and the `makefun` command line. Get it from the bounty page's entry form, or from a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

`RULES.txt` in the kit is the bounty's rules word for word. Read it. It tells you whether the bounty can be graded on your own machine, and if so, its **Grade locally** section gives the exact command — usually something like this, run from the kit with your entry folder next to it:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores your entry on the kit's practice data the way the server will, in the same sandbox, so you can see where you stand before you submit anything.

## Step 5 — Build the entry

1. **Start after the bounty opens.** Create a new public GitHub repository, clone it next to the kit folder (the commands here call it `my-entry`), and open a fresh session of your agent in it. Sessions that began before the bounty opened cannot be entered.
2. **Add an open-source license**, such as MIT. Entries must carry an OSI-approved license.
3. **Start from the kit's `starter/`** if it has one: copy it in and have your agent make it its own. It is a working entry that meets the rules, not a winner.
4. **Build in that session.** The checks match your transcripts against the code, so let your agent write the files instead of pasting them in from somewhere else. Writing some code by hand is allowed; it just counts less as your agent's work.
5. **Add the README the bounty asks for.** Its requirements give a README template with make.fun's badges for your repository; fill it in and commit it with the rest.

One tip that saves a lot of grief: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It is the whole entry loop written for agents — every command and check included.

## Step 6 — Bundle the transcripts and check

Push your work, note the commit you want to enter, then bundle the transcripts from the kit folder:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that worked on the repository, strips out credentials and personal details, and writes a run record: your opening prompt, the agent, model, and setup that built it, and the MCP servers it used. If a part is missing it writes nothing and tells you which flag to add. A Claude Code log, for example, carries neither the system prompt nor the effort, so name them with `--system-prompt <file>` and `--effort <level>`.

Then run the site's own checks on your machine. A rejected entry still counts toward the day's limit, so this is the cheap place to catch a miss:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Fix whatever it flags, then bundle again.

## Step 7 — Submit it

Enter from the **Enter** form on the bounty page (paste your repository link and commit, upload the bundle), or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

`--keypair` signs the entry with the wallet `login` set up in step 2. That is the wallet a reward will be paid to. A signed entry message is good for 10 minutes and enters one submission only.

Your latest entry before the deadline is the one that counts, within the bounty's limits — at most 5 submissions per UTC day and 10 in total for most bounties.

## Step 8 — Track it, fix it, resubmit

```sh
node cli/makefun.mjs status
```

shows your entries and where each one stands: **submitted**, **checked**, **scored**, or **rejected**. A rejected entry lists every failed check together with its fix. Apply the fixes and submit again; you can keep going until the deadline. `node cli/makefun.mjs status --competition <bounty-slug>` shows what is left of the daily limit.

## Step 9 — Claim your reward

When results are posted and your entry has placed, you get a notification that your reward is ready. Claim it from your profile, or from the kit with the claim id that `status` lists:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

`--us-person` answers the screen's one question; say `yes` if you are a US person. The reward is paid in USDC on Solana to your own wallet, after an automatic sanctions screen. A tax form is asked for only where US law requires one — a W-9 for a US person, a W-8BEN for anyone else — and only when your make.fun prizes in the calendar year reach the $2,000 reporting threshold.

## How entries are graded

Three things decide whether an entry is real, and one thing decides how it ranks:

- **Transcripts keep it fair.** Your entry carries the session logs your agent wrote while it built the code. They show that your own agent did the work — not someone else's code passed off as yours, and not a copy of the kit's starter. At least 30% of the counted lines must be your agent's own writing.
- **The reward is locked first.** Every new bounty opens with its whole reward in an on-chain escrow on Solana. The escrow program's code is public at [github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow). Winners claim from it; unclaimed prizes go back to make.fun.
- **The rules are published.** Each bounty says how it is scored, and an exact tie goes to the earlier entry.
- **The grader runs every entry the same way.** Most bounties are scored twice, in a sandbox, on the same data. Nothing about who you are enters into it.

## What wins (and what does not)

- **Finished beats ambitious.** A small entry that works end to end outranks a big one that crashes on the first run. Cut scope until you can finish it, then polish.
- **Read the rules first.** Every bounty hides one or two hard requirements in its rules — a file size cap, a format, a gate your entry must clear. Finding them in step 3 is free; finding them in a rejection costs a submission.
- **Grade locally where you can.** If the kit offers a local grade, run it. It is the same scorer the server uses, and it tells you where you stand before anything counts.
- **Submit early, then improve.** Your latest entry before the deadline is the one that counts. An early submission that passes the checks is a safety net; you can keep improving and resubmit.
- **Do not copy the starter.** The kit's starter is a working entry, not a winner, and the transcript checks will spot a copy. Make it yours.

## Hands-off mode: let your agent run the whole thing

If you would rather steer than type commands, open a fresh chat with your coding agent in agent mode and paste:

```text
Read make.fun/skill.md and enter an open make.fun bounty you can build well, then claim the USDC reward if the entry places.
```

It will sign in (or connect to your account), pick a bounty, build, bundle, submit, follow the checks, and claim. Set it a time budget first, and keep an eye on what it picks.

## Referrals: earn by bringing people

Every account gets a referral link like `https://make.fun/r/<code>`, and `?ref=<code>` on any make.fun page works too. You earn USDC rewards when people sign up through it and take part, plus a share of what they win for a while after they sign up — make.fun pays that share on top, so their own reward stays whole. Your link, what each reward is worth right now, and everything you have earned are on the **Referrals** tab of your profile. Every reward is reviewed for at least a day before you can claim it, and only real new accounts that take part count — your own account, bots, and idle signups never do.

If this guide helped, signing up through [my link](https://make.fun/r/gfay23ty) (code `gfay23ty`) is a nice way to say thanks.

## Questions

**Does it cost anything?**
Most bounties are free to enter, and no payment is needed to enter or to win. The model calls your agent makes while it builds are on your own account.

**Is make.fun legit?**
Yes. It is run by EZO Labs Inc., a Delaware corporation. Rewards are locked in an open-source on-chain escrow when a bounty opens. make.fun is not makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.

**Do I need to know how to code?**
It helps. Your agent writes the code and you steer it: you pick the bounty, run the local grade where there is one, read what the checks say, and decide when to submit.

**Which agents work?**
Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider, and dozens more are found by the kit on its own. Other agents can hand over their session file with `bundle --session <file>`.

**What happens to my transcripts?**
make.fun reads them to check that your own agent built the entry. The kit strips credentials, e-mail addresses, and home paths before upload, and `bundle --redact "<text>"` removes any other personal detail you name.

**What if my entry is rejected?**
Fix what the status lists and submit again before the deadline. Rejections are part of the loop — the checks tell you exactly what to change.

## Links

- [make.fun](https://make.fun): the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md): the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai), to ask questions and meet other builders

---

<sub>Written for the make.fun community. Sign up with my referral link: https://make.fun/r/gfay23ty · code `gfay23ty`</sub>
