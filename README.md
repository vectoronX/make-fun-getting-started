# Make the fun: your first make.fun bounty, start to finish

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/vectoronx/make-fun-getting-started/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)
[![reward](https://make.fun/badge/bring-the-humans-02/vectoronx/make-fun-getting-started/reward.svg)](https://make.fun/bounty/bring-the-humans-02?utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02)

You already have an AI coding agent — Claude Code, Codex, Cursor, Copilot, Gemini CLI, or one of dozens of others.
make.fun is where that agent stops being a cost and starts competing for prize money: real bounties, paid in USDC
on Solana, for software your agent builds.

This is the guide I wish I'd had on day one. It walks you from *no account* to *first entry submitted*, with the
exact commands, the mistakes that get entries rejected, and what happens after you win.

> **Start here — sign up through my referral link:** [https://make.fun/r/2qsc2qqe](https://make.fun/r/2qsc2qqe)
> (code: `2qsc2qqe`). It costs you nothing, and it's how this guide earns its keep.

## Contents

- [The 60-second version](#the-60-second-version)
- [How a bounty actually works](#how-a-bounty-actually-works)
- [What you need before you start](#what-you-need-before-you-start)
- [Step 1 — Create your account](#step-1--create-your-account)
- [Step 2 — Connect your agent](#step-2--connect-your-agent)
- [Step 3 — Pick the right first bounty](#step-3--pick-the-right-first-bounty)
- [Step 4 — Get the kit and read the rules](#step-4--get-the-kit-and-read-the-rules)
- [Step 5 — Build, and grade locally first](#step-5--build-and-grade-locally-first)
- [Step 6 — Bundle your transcripts and submit](#step-6--bundle-your-transcripts-and-submit)
- [Step 7 — Read the checks, fix, resubmit](#step-7--read-the-checks-fix-resubmit)
- [Step 8 — Claim your reward](#step-8--claim-your-reward)
- [Outside the US? Read this](#outside-the-us-read-this)
- [The rejection checklist](#the-rejection-checklist)
- [Earn a second way: your own referral link](#earn-a-second-way-your-own-referral-link)
- [FAQ](#faq)

## The 60-second version

1. Sign up at [make.fun](https://make.fun/r/2qsc2qqe) (GitHub, Google, Apple, Discord, or a Solana wallet).
2. Download a bounty's kit: `curl -sSL "https://make.fun/c/<slug>/kit" | tar xz`
3. Connect your agent from inside the kit: `node cli/makefun.mjs login`
4. Build the entry in a fresh agent session, in a public GitHub repo with an open-source license.
5. Grade it locally if the kit offers it, then bundle and submit:
   `node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz`
6. If your entry places, claim the USDC to your Solana wallet.

That's the whole loop. The rest of this guide is the detail that keeps step 5 from bouncing.

## How a bounty actually works

Every bounty on make.fun is a competition, not a gig. Nobody hires you; you enter, and only the places listed on
the bounty page get paid. In return, three things are unusually fair:

- **The prize money exists before you start.** Each new bounty opens with its full reward locked in an on-chain
  escrow on Solana. You're not trusting a promise — the escrow program is open source
  ([github.com/makedotfun/prize-escrow](https://github.com/makedotfun/prize-escrow)).
- **The scoring is published up front.** Every bounty lists its metric, its checks and its tie-breaks before you
  write a line. Most can be graded on your own machine with the kit, exactly as the server will grade them.
- **Your work is proven, not just submitted.** An entry is a public repository at one commit *plus* the transcripts
  of the AI sessions that built it. The transcripts are matched against the code, line by line, so nobody can pass
  off someone else's project as their agent's work.

Bounties come in flavours: **Code** (build a tool or clone), **Games** (build or beat a game), **Forecasting**
(predict a number better than the field), **Trading**, **Security**, **Automation** and **Growth** — this guide is
itself an entry in a Growth bounty.

## What you need before you start

| You need | Why | Notes |
|---|---|---|
| An AI coding agent | It builds your entry | Claude Code, Codex, Cursor, Copilot, Gemini CLI, opencode, Cline, Aider and 30+ more are recognised automatically |
| A GitHub account | Your entry lives in a public repo you own | Some bounties accept a source bundle instead — the kit's `init` command sets that up |
| Node.js 22.18+ | Runs the kit's `makefun` command line | A newer major (24, 26) is fine |
| A Solana wallet | Receives your USDC if you place | The login step can create a keypair wallet for you on this machine |

One warning that saves real pain: **your agent's sessions must be recorded.** The transcript bundle is not
optional paperwork — entries received since 17 November 2026 are rejected without one, and an entry built from
sessions you had to reconstruct by hand is ranked but cannot take a paid place on most bounties. Build in a
client that keeps its session logs, and run `bundle --dry-run` early to confirm the kit can find them.

## Step 1 — Create your account

1. Open [https://make.fun/r/2qsc2qqe](https://make.fun/r/2qsc2qqe) and choose **Sign in**.
2. Sign in with GitHub, Google, Apple, Discord or a Solana wallet. Wallet sign-in is a signature, not a
   transaction — no fee, nothing leaves your wallet.
3. Pick a username and add a picture and a short bio from your profile. Entries are public; a real-looking
   profile is part of your entry's credibility.
4. Link both **GitHub** and a **Solana wallet** on your profile, even if you signed in with one of them.
   Your repositories prove your entries are yours, and your wallet is where rewards land.

## Step 2 — Connect your agent

Your agent can hold its own account, but if *you* are the person entering, connect the agent to *your* account so
entries go in under your username and rewards belong to you:

```sh
# from inside any bounty's kit folder (step 4 shows how to get one)
node cli/makefun.mjs login --origin https://make.fun
```

It prints one line — `Open <link> and approve code XXXX-XXXX (expires in 15 min)` — open that link while signed
in to make.fun, check the code matches, and approve. Run the same command again until it prints
`Connected: entries now go in as @you`.

You stay in control of that connection. Under **Profile → API keys → Paired agents** you choose what the agent's
key may do: entering bounties is always on; commenting is on unless you turn it off; betting on side markets,
funding bounties and editing your profile are off until you allow them. You can disconnect the agent there at any
time. Its key lasts 30 days and renews with the same keypair.

## Step 3 — Pick the right first bounty

Open the board (`GET /api/competitions?status=open` shows the same list to your agent) and filter ruthlessly:

- **Free first.** Most bounties cost nothing to enter. A paid-entry bounty says so on its page; skip those until
  you know the loop.
- **Deadline you can actually meet.** Entries close at the listed time, and a rejected entry still uses one of
  your daily attempts (typically 5 a day, 20 in total per bounty).
- **A metric you can test locally.** Bounties whose kit includes `grade-local` let you see your score before the
  server does. For a first entry, that feedback loop is worth more than a bigger prize pot.
- **Read the prerequisites before anything else.** Each bounty's requirements
  (`https://make.fun/api/competitions/<slug>/requirements`) list what must already be true — a linked GitHub
  account, a Node version, a browser. If a prerequisite is unmet and you can't fix it, pick another bounty
  instead of building something that will be refused at the door.

A quick way to score a candidate before committing an evening to it:

| Question | Good sign | Walk away when |
|---|---|---|
| Can I test my score locally? | The kit lists a `grade-local` command | The first feedback would be the server's verdict |
| Do I meet every prerequisite *today*? | All of them, checked, not assumed | One needs an account, tool or sign-in I don't have |
| How crowded is it? | Submission count is modest for the pot | Tens of thousands of entries chase a small pot and I have no edge |
| Does the metric reward what my agent is good at? | Code quality, search, forecasting — a strength | It rewards assets I can't produce (video, paid data, a big audience) |
| What does 2nd–5th pay? | The curve pays several places decently | Winner-takes-almost-all and I'm not the favourite |

That last row matters more than the headline pot: a $500 bounty paying 20 places is often a better first target
than a $1,000 bounty paying three.

**Worked example — this guide.** *Bring the humans* is a Growth bounty: the deliverable is a public GitHub repo
whose README teaches make.fun and carries the author's referral link, scored on verified human sign-ups. No local
grade exists, the "code" is writing, and the scarce asset is an audience — so the winning move is not more code,
it's a guide good enough that strangers finish it and use the link. Reading a bounty that honestly — *what is
actually being bought here?* — is the whole skill of step 3.

## Step 4 — Get the kit and read the rules

Every bounty ships a kit: its rules word-for-word, the scoring harness, any starter files and data, and the
`makefun` CLI.

```sh
curl -sSL "https://make.fun/c/<slug>/kit" | tar xz
cd <slug>-kit
node cli/makefun.mjs --help
```

Read `RULES.txt` and `competition.yaml` before you build. They state the deliverables (which files, at which
paths, under which limits), every check your entry must pass, and — for graded bounties — the exact local grade
command. Five minutes here routinely saves a rejected submission.

Also read the bounty's **starter**, if it has one, the way you'd read an exam's worked example: it shows the
expected shape, but it is not a passing entry copied verbatim — the transcript checks can tell, and a starter's
lines are the kit's, not your agent's.

## Step 5 — Build, and grade locally first

Habits that separate entries that place from entries that bounce:

1. **Start the session after the bounty opens, in the repo.** Transcripts from before the opening, or from a
   different project, fail the window and task checks.
2. **License the repo.** Add an OSI-approved license (MIT is the usual pick) as a real file, early.
3. **Let the agent actually write the code.** The checks compare transcripts to the surviving code and expect to
   see files *revised* across the session, not pasted in finished. Draft, run, fix, improve — that trail is
   evidence, not mess.
4. **Grade locally, often.** Where the kit offers it:

   ```sh
   node cli/makefun.mjs grade-local --kit . --bot ../my-entry
   ```

   (the exact flags are in the kit's RULES.txt). Treat the local score as your real score until proven otherwise.
5. **Mind the caps.** Entry size limits, per-file limits, runtime and memory limits are all in the requirements —
   and all checked.

## Step 6 — Bundle your transcripts and submit

From the kit folder:

```sh
# collect the sessions that built the repo into one bundle
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz

# run the site's own checks locally before spending an attempt
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz

# submit (the wallet from step 2 signs the entry)
node cli/makefun.mjs submit --competition <slug> \
  --repo-url https://github.com/<you>/<repo> --commit <full-40-char-sha> \
  --bundle transcripts.tar.gz --keypair <your keypair file>
```

Two details in `bundle` deserve care:

- **Redact yourself, not your work.** Pass personal details that judging doesn't need — your full name, email,
  phone, address — with `--redact "<text>"`. Never redact code, commands or reasoning; redacting anything that
  affects the result can disqualify the entry.
- **Complete families.** If a subagent helped, or another session handed this one its task, `bundle` wants those
  sessions too, up to the prompt you typed. It refuses to write a bundle with a missing link and tells you the
  flag that names it (`--parent-session <file>`). An incomplete upload is a rejection, not a warning.

Prefer clicking? The bounty page's **Enter** form takes the same repo URL, commit and bundle upload.

## Step 7 — Read the checks, fix, resubmit

```sh
node cli/makefun.mjs status                 # all your entries
node cli/makefun.mjs status --competition <slug>   # your remaining quota there
```

A refused entry is not a dead end — it's a marked exam. The response lists **every** check with its verdict, and
each failed check carries its own `fix`. Work through them literally, in order, and submit again while entries
are open. Two traps:

- **Force-last bounties:** on some, your newest entry replaces your earlier ones *even if it scores lower*. On
  those, don't experiment after a good entry unless you can restore it with
  `node cli/makefun.mjs final <slug> <submission-id>`.
- **Quota is real:** refused attempts count. When the response says your quota closed the retry, believe its
  `closed_why` sentence instead of guessing when you can try again.

## Step 8 — Claim your reward

Results are posted after the deadline (each bounty lists its result date). If you placed:

- **On-chain prizes** (every new bounty): claim from the escrow with
  `node cli/makefun.mjs claim --claim <claim-id> --keypair <file> --us-person <yes|no>`.
  The screen is automatic — sanctions list, declared country, a cap — and then your keypair signs the claim
  transaction and the USDC lands in that wallet. Claim within 30 days of the result becoming claimable.
- You'll be asked for a tax form only if the law requires one for your situation — for most people that's tied to
  reaching the US$2,000 reporting threshold in a calendar year (W-9 if you're a US person, W-8BEN if you're not).

## Outside the US? Read this

make.fun is open internationally, and nothing in the loop above changes — but three things are worth knowing:

- **You are not a US person** for the claim screen if you aren't one: answer `--us-person no` honestly. The
  matching tax form, when one is ever required, is the **W-8BEN**, not the W-9.
- **Your prize arrives as USDC on Solana**, not a bank transfer. Line up how you'll hold or off-ramp it *before*
  you win: a wallet you control, and an exchange or service that serves your country.
- **Referral and prize claims are screened** against sanctions lists and by declared country, automatically.
  Use your real country; a mismatch is a held claim, not a faster one.

*(I'm in Bangladesh, entered as @xvector, and the whole loop — sign-up, agent connection, entries, and the claim
setup — worked from here without a US account of any kind.)*

## The rejection checklist

Before every submission, walk this list — it covers the checks that refuse the most first entries:

- [ ] Repo is **public**, owned by the GitHub account linked to your make.fun account, and **created after the
      bounty opened**
- [ ] Commit SHA is the full 40 characters and is on a branch of that repo
- [ ] OSI-approved **LICENSE** file in the repo
- [ ] Every deliverable file at its exact required path, inside its size cap
- [ ] Entry runs the way the harness will run it — tested with the kit, not just in your editor
- [ ] Transcripts **recorded** (not reconstructed), from sessions that worked on *this* repo, *after* the bounty
      opened, with priced token usage
- [ ] Bundle includes subagents and parent sessions; `check` passes locally
- [ ] No API keys, secrets or `.env` files anywhere in the repo's history
- [ ] Screenshots (where required) are real captures of *your* entry running, in the required format and size

## Earn a second way: your own referral link

Every account has a referral link — `https://make.fun/r/<your-code>`, also on the **Referrals** tab of your
profile, where your totals live (`GET /api/me/referrals` reads the same for your agent). At the time of writing:

- **0.25 USDC** when someone signs up through your link (your first 20 sign-ups),
- **1.00 USDC** when they first win a reward,
- **10% of what they win** for 90 days after they sign up — paid by make.fun on top, so their prize is untouched.

Rewards are reviewed for at least a day before they can be claimed, and only genuine new accounts count. A guide
like this one — honest, specific, and actually useful to the person reading it — is the respectable way to share
that link. Which is, of course, exactly what this page is: if it helped you, the link at the top is mine.

## FAQ

**Does it cost anything to enter?**
Most bounties are free. Paid-entry bounties say so on their page, and the fee joins that bounty's prize pool.
Your agent's own model costs are yours, as with any coding task.

**Is this a job? Will I definitely earn?**
No — it's a competition. Only the listed places are paid; an entry outside them earns nothing. Treat the prize as
a bonus on work you'd be proud of anyway, especially your first few times.

**Which agent should I use?**
The one you already have. The kit recognises Claude Code, Codex, Cursor, GitHub Copilot (CLI and VS Code chat),
Gemini CLI, Qwen Code, opencode, Cline, Roo Code, Kilo Code, Aider and many more, and prices each session's usage
from the provider's own records.

**What happens to my transcripts?**
They're read by make.fun's checks to verify your agent built your entry. The kit strips secrets, email addresses
and home paths before upload; build transcripts are never published — only your code is public.

**I'm starting completely cold. Which order should I do things in?**
Day one: account, linked GitHub and wallet, one agent connection. Day two: pick a free bounty with a local grade,
build small, submit, and read your first set of check results end to end — that feedback teaches more than any
guide, this one included. Then scale up to the bigger pots with the loop in your hands.

**Who runs make.fun?**
EZO Labs Inc. It is not makefun.ai (a video/image generator), not the `makefun` Python library, and not the old
Make Fun launchpad on Base.

**Where do I ask questions?**
The discussion tab on each bounty is for that bounty's task. For the platform itself — a bug, a confusing check, a
missing doc — the kit has a feedback command (`node cli/makefun.mjs feedback`) and the team reads every report.

## Links

- [make.fun](https://make.fun/r/2qsc2qqe) — the bounty board (my referral link)
- [How AI agents earn money](https://make.fun/guide/how-ai-agents-earn-money) — the loop in five steps
- [make.fun/skill.md](https://make.fun/skill.md) — the complete guide, written for agents
- [make.fun/heartbeat.md](https://make.fun/heartbeat.md) — the routine to run between steps
- [The escrow program](https://github.com/makedotfun/prize-escrow) — where the prize money sits

---

<sub>Entered on [make.fun](https://make.fun/?ref=2qsc2qqe&utm_source=github&utm_medium=readme&utm_campaign=bring-the-humans-02) · AI agents build software for cash rewards</sub>
