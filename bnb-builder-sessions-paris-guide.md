# BNB Builder Sessions Paris: Builder Guide

10 November 2026

## Before you start

Install these before 18:30 so you can start building the moment we open the build session.

- Node.js 22 or newer
- Corepack with pnpm 10 (the next section enables it)
- Bun 1.3 or newer, needed to deploy
- Claude Code or Cursor, the primary way to use Studio (no editor? see the manual route below)
- A throwaway wallet, created tonight on BSC testnet and never reused on mainnet

## Set up in five steps

Run these in a terminal, in order.

1. Check Node: `node --version` must print 22 or higher.
2. Enable pnpm: `corepack enable`
3. Install Studio: `npm install --global @bnbagent/studio-cli`
4. Teach your editor: `bag skills install`, then restart Claude Code or Cursor. It asks which editor and scope you want; the scripted form is `bag skills install --target both --scope user`.
5. Install Bun 1.3 or newer from [bun.sh](https://bun.sh).

## Build your agent

Open Claude Code or Cursor in an empty folder, then describe your agent in one prompt. Pick your own name and say what it sells.

```
/bnbagent-studio Create a BNB Chain seller agent named bnbbrief that sells a short BNB market brief. Start on BSC testnet, use the default wallet and LLM, store deliverables on IPFS, and prepare it for the 48-hour BNB testnet trial.
```

Name rule: letters and numbers only, starting with a letter, 23 characters at most. No `-`, `_` or `.`.

The skill asks all its questions in one round, then shows a to-do list and builds the project. You normally edit one file only:

| File | What it is | Your job |
| --- | --- | --- |
| `app/agent/src/sellerCore.ts` | The work your agent sells (the `runWork` hook) | Edit this |
| `app/agent/src/signing.ts` | Quotes and signatures | Don't touch |
| `app/agent/studio.toml` | Network, wallet, LLM, prices | Check it, change only if needed |
| `.studio/wallets/` | Encrypted keys | Never commit |

## No Claude Code or Cursor?

The skill installs only for those two editors, but `bag` works without it. Scaffold the project by hand, then edit `sellerCore.ts` in any editor, with any assistant to help you write the code:

```
bag init bnbbrief --network bsc-testnet --storage-provider ipfs
```

`bag init --help` lists every option, including `--protocols`, `--wallet-kind`, `--llm-provider` and `--no-onboard`. The name rule above applies. After that, continue with the local run and the deploy steps below. You skip only the one-prompt scaffold.

## Run and diagnose locally

Run `bag doctor` first: it checks the project, wallet, balances, LLM, network and local runtime. All green means you are good; a red line tells you what to fix. Then start your agent:

```
bag doctor
bag dev
```

`bag dev` serves the faces you selected:

| Face | Local address |
| --- | --- |
| A2A | Agent card and JSON-RPC on port 9000 |
| MCP | `http://localhost:8000/mcp` |
| X402 | `/x402` on the same runtime |

## Deploy and verify onchain

Deploy to the managed 48-hour BNB testnet trial, then check that your onchain identity matches the live agent:

```
bag deploy prepare                # readiness checks
bag deploy --provider bnb         # 48-hour testnet trial
bag deploy verify --provider bnb  # identity matches the live agent
```

Before you deploy, switch deliverable storage to IPFS. Local storage fails the readiness check by design, because a local path is unreachable from a deployed runtime.

The trial runs in the operator's cloud, so your signing material leaves your control. Use a throwaway wallet (`bag wallet new`), never reuse it on mainnet, and expect the trial to be disabled after it expires. `bag deploy status --provider bnb` shows whether your agent is live. If a command differs on your version, `bag deploy --help` is the reference.

## Four rules for tonight

1. Approve every command. Never allow all `bag` commands at once, because the CLI includes deploy and payment commands.
2. Keep passwords out of chat. Type your wallet password in the hidden prompt only, never in chat or as a command-line argument.
3. Testnet only. Never reuse tonight's wallet on mainnet.
4. Stuck for 5 minutes? Raise your hand and a DVB mentor will come to you.

## Stuck? Try this first

For anything odd, run `bag doctor` before you raise your hand.

| Symptom | Try this |
| --- | --- |
| Anything odd | `bag doctor` |
| Node error | `node --version` (needs 22 or higher) |
| Build fails | `corepack enable`, then reinstall |
| Skill not found | Restart your editor |
| LLM error | `bag llm status --full`, then `bag llm activate` |
| Deploy blocked on storage | Switch deliverable storage to IPFS, then run `bag deploy prepare` again |
| Still stuck | Raise your hand, a mentor comes |

## What to build, and how you are judged

The jury looks for three things: your agent runs (deployed and verified onchain), it is original (not a copy of the demo), and it is useful (someone would pay for it). We pick 3 finalists during the build, and each pitches for 1 min 30.

Need an idea? Sell something people need:

- Transaction explainer: a hash in, what happened in plain English out
- Wallet summary: an address in, a readable profile out
- Contract checker: what a contract can do with your tokens
- Token red flags: ownership and honeypot checks on a token
- BNB price watcher: alerts when BNB crosses your target
- Cited research report: a short report, with its sources

Submit before 19:50 with four things: your agent's name, its agent ID or address on BSC testnet, one sentence on what it sells, and your feedback on BNB Agent Studio. Everyone fills in the feedback, even without a deployed agent.

## Links and sources

Studio is under active development, so `bag --help` is the reference for the version you installed. This guide follows the official docs as read on 10 November 2026.

- [BNB Agent Studio docs](https://docs.bnbchain.org/developer-kit/bnbchain-studio/)
- [Quickstart](https://docs.bnbchain.org/developer-kit/bnbchain-studio/quickstart/)
- [Configuration](https://docs.bnbchain.org/developer-kit/bnbchain-studio/configuration/)
- [CLI reference](https://docs.bnbchain.org/developer-kit/bnbchain-studio/cli-reference/)
- [Deployment](https://docs.bnbchain.org/developer-kit/bnbchain-studio/deployment/)
- [BNB Agent SDK](https://github.com/bnb-chain/bnbagent-sdk)
- [BNB Agent Studio](https://www.bnbchain.org/en/bnb-agent-studio)
- Telegram: [t.me/BNBChainFRdevs](https://t.me/BNBChainFRdevs) · X: [@BNBCHAIN](https://x.com/BNBCHAIN) · [DeVinci Blockchain](https://linktr.ee/devinci_blockchain)
