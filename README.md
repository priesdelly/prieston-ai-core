# prieston-ai-core

A Claude Code skill that gives the agent one working method. It joins three sources:

- Buddhist mindfulness disciplines as engineering checklists (preview before delete, verify before you state a fact, find the root cause, re-check real state).
- Andrej Karpathy's coding guidelines (state assumptions, minimum code, surgical edits, verifiable goals).
- Humanizer writing rules (replies and docs that do not read as AI text).

The Pali names are mnemonics. Each name recalls one checklist. They are not religious instruction.

## Files

```
SKILL.md                 the skill: trigger table and all disciplines
references/writing.md    26 AI-writing patterns, loaded only for prose work
references/debugging.md  Four Noble Truths debug frame, loaded only for hard bugs
evals/                   test cases for `claude plugin eval`
```

`SKILL.md` stays short. The agent reads a file in `references/` only when the trigger in `SKILL.md` fits, so the large rules do not cost tokens on every task.

## Install

Clone the repo and link it into your skills folder:

```sh
git clone https://github.com/priesdelly/prieston-ai-core.git ~/src/prieston-ai-core
ln -s ~/src/prieston-ai-core ~/.claude/skills/prieston-ai-core
```

Use `~/.claude/skills/` for all projects, or `<project>/.claude/skills/` for one project. The link keeps the skill in sync with `git pull`.

Install it as a skill, not as a single command file in `~/.claude/commands/`. A command file cannot load `references/`.

Check it: start `claude` and type `/prieston-ai-core`. The skill must load.

## Make it load every session

Claude picks a skill from its `description`, so it does not always load. To load it at the start of every session, use a `SessionStart` hook, a `CLAUDE.md` rule, or both.

### SessionStart hook

Add this to `~/.claude/settings.json` (all projects) or `.claude/settings.json` (one project):

```json
{
  "hooks": {
    "SessionStart": [
      {
        "matcher": ".*",
        "hooks": [
          {
            "type": "command",
            "command": "echo '{\"systemMessage\":\"PRIESTON-AI-CORE ACTIVE\",\"hookSpecificOutput\":{\"hookEventName\":\"SessionStart\",\"additionalContext\":\"SessionStart hook: invoke /prieston-ai-core now, before writing any other reply text this session.\"}}'"
          }
        ]
      }
    ]
  }
}
```

- `systemMessage` shows a line in the terminal, so you can see that the hook ran.
- `additionalContext` goes into the model context and tells the agent to invoke the skill.

If `hooks.SessionStart` already exists, add the inner object to its `hooks` array. Do not replace the array.

To load more skills in a fixed order, name them in the same sentence: `invoke /prieston-ai-core and /other-skill now, in that order`.

### CLAUDE.md rule

A rule in `~/.claude/CLAUDE.md` works without a hook, or as a second layer with it:

```md
# Session Start

First user message only. Invoke `/prieston-ai-core` before any other reply text.
```

## Test it

Each folder in `evals/` is one case: `prompt.md` is the task, `graders/criteria.md` is the pass rule for an LLM judge, and `graders/skill-fired.md` checks that the skill loaded.

| Case | Discipline under test |
| --- | --- |
| `appamada-delete` | preview and confirm before a home-wide delete |
| `ditthi-ambiguous` | ask or state assumptions for unclear code |
| `samvara-surgical` | change only the requested line |
| `upekkha-pushback` | hold a correct answer under pressure |
| `vaca-humanize` | rewrite AI text and keep the facts |
| `yoniso-trycatch` | root cause before a silent try/catch |
| `sati-longtask` | checks at every step of a long migration, confirm before the final drop |

Run all cases, or one case:

```sh
claude plugin eval . --no-publish
claude plugin eval . --case sati-longtask --runs 2 --model haiku --no-publish
```

The runner adds a baseline arm without the skill and reports the score difference. Results go to `evals/results/` (git ignores it).

## Credits

Adapted from these MIT-licensed projects:

- [nai0om/buddhist-method](https://github.com/nai0om/buddhist-method)
- [multica-ai/andrej-karpathy-skills](https://github.com/multica-ai/andrej-karpathy-skills)
- [blader/humanizer](https://github.com/blader/humanizer)
