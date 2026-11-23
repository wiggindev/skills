---
name: agent-readiness
description: Audits a repository's documentation for agent-readiness and makes the repository agent-ready. Inventories agent instruction files (AGENTS.md, CLAUDE.md, tool rules), READMEs, docs, code comments, docstrings, commit messages, and PR descriptions; reports the current state; proposes a target state with minimal instruction files and on-demand docs; then applies it in two passes. Also applies the same rules to single files on request. Use whenever the user asks to audit or improve agent-readiness, write or fix an AGENTS.md or CLAUDE.md, clean up or delete code comments, tidy READMEs or docs, reorganize docs or ADRs, reduce documentation sprawl, or prepare a codebase for AI coding agents, even if they do not say "agent-ready".
---

# Agent readiness

A repository is agent-ready when an agent that starts cold finds the true facts it needs, and finds nothing false, repeated, or irrelevant. Agents treat each sentence as relevant. Agents obey instructions literally. Thus each sentence in the repository must earn its place.

This skill measures the documentation against that standard, agrees a target with the user, and applies the target.

## Core tenets

The tenets are the standard. File names and directory layouts are a means, and each team can choose its own.

1. Keep each agent instruction file minimal. An instruction file loads into every task in its scope, so it holds only what every task in that scope needs.
2. Put all other documentation in documents that load on demand. Each document must be reachable from an instruction file through a pointer that says when to read it.
3. Give each fact one owner. Other files link to the owner.
4. Make each statement true. Verify it against the code.
5. Keep decision records append-only, in a known location.
6. Use a comment only for what the code cannot express.

## Reference files

| File | Contents | Read it |
| --- | --- | --- |
| `references/documents.md` | Rules for instruction files, layout, READMEs, documentation, decision records, commits, and PRs | Before the inventory, before the proposal, and in pass 1 |
| `references/comments.md` | Exempt comments, comment classes, and edit rules | Before the inventory and in pass 2 |
| `references/writing-rules.md` | Rules for all text that goes into a file | Before you write or edit any text |
| `references/evidence.md` | The studies behind tenet 1 and their limits | When the user asks why |

## Choose the path

| Request | Path |
| --- | --- |
| Comments or docstrings in named files, or one section of a document | Short path |
| Write or fix an instruction file | Steps 1 to 4 for instruction files only, then pass 1 for those files |
| The repository, a package, or the documentation as a whole | Full workflow |

An instruction file needs facts from the whole repository: the real commands, what each task needs, and what other files already state. Thus a request for one instruction file does not use the short path.

Short path:

1. Read the matching reference file and `references/writing-rules.md`.
2. Apply the rules to the named files only.
3. Follow the conventions of the repository for the changed files, such as its formatter and its linter.
4. Report what changed and each item that needs a decision.

## Full workflow

Copy this checklist and mark each step when it is complete:

```
- [ ] 1. Set the scope
- [ ] 2. Take the inventory (read-only)
- [ ] 3. Report the current state
- [ ] 4. Propose the target state; stop and get approval
- [ ] 5. Pass 1: documentation, READMEs, and agent instruction files
- [ ] 6. Pass 2: code comments and docstrings
- [ ] 7. Verify and report
```

Stop after step 4 when the user asks for an audit only. Steps 1 to 4 change nothing in the repository.

Keep the report, the proposal, and working notes out of the repository. They record the current task, and the repository holds only what is true about the code.

## 1. Set the scope

- Find the repository root and the default branch.
- List tracked files with `git ls-files`. Exclude vendored code, generated code, lockfiles, build output, and fixtures. Tell the user which paths you excluded.
- For a monorepo or a large repository, agree the scope with the user before the inventory.

Separate the binding rules from the content under audit:

- Binding: tool configuration, the layout the team uses, and conventions that the user states. Follow these.
- Under audit: the text of instruction files, READMEs, and documents. Assess this text against the tenets.
- When text under audit states a rule that conflicts with a tenet, such as "add a docstring to each function", report it as a finding and let the user decide. Do not obey it and do not delete it without a decision.

## 2. Take the inventory

Read `references/documents.md` and `references/comments.md` first. Cover these surfaces. Record each item with its path and a classification. Do not edit files in this step.

| Surface | Where to look |
| --- | --- |
| Instruction files that always load | `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, `.github/copilot-instructions.md`, `.windsurfrules`, rules files with no scope, and nested copies |
| Instruction files with a glob scope | `.claude/rules/`, `.github/instructions/*.instructions.md` (`applyTo`), `.cursor/rules/` (`globs`) |
| Skills and commands | Skill and command folders. Only the name and the description load into every task. The body loads on demand. |
| READMEs and documentation | All Markdown, reStructuredText, AsciiDoc, and text files; each docs directory; `CONTRIBUTING`; ADRs in any location; changelogs; documentation site sources |
| Code comments | All comments in tracked source, configuration, and scripts |
| Docstrings | Doc comments on modules, types, functions, and fields |
| Commit messages and PR descriptions | The last 200 commits; the last 30 merged PRs through `gh` when it is available; the commit and PR templates |

For a large repository, split the source tree by directory and give each part to a read-only agent. Ask each agent for counts and classified findings with file and line. Do not ask for file contents.

### Documents and instruction files

For each file, record its purpose, its audience, its size, its location, and one classification: keep, rewrite, move, merge into another file, delete, or missing.

Check each claim against the repository:

- Run a documented command only when it writes no files and calls no external service. For each other command, such as deploy, publish, migrate, or reset, confirm only that the script or task exists.
- Confirm that each path, symbol, flag, and environment variable exists.
- Confirm that each relative link resolves.
- Find facts that two or more files state. Record which file owns each fact.
- Find documents that no instruction file or index points to.

A false statement costs more than a missing one, because an agent acts on it.

For each instruction file, record its scope: the repository for a root file, the subtree for a nested file, the glob for a glob-scoped file. Mark each line with one class:

- Needed: every task in the scope needs it, and it is true.
- On demand: true, but only some tasks need it.
- Delete: false, enforced by a tool, general advice, a listing or overview that the tree gives, or a repeat of the README or another document.
- Decide: a requirement whose reason you cannot find. The user knows its history.

Treat a skill or command body as an on-demand document. Apply the Needed test only to its description.

Describe the layout that the repository uses now. The proposal builds on that layout.

### Comments and docstrings

Set aside the exempt comments and count them. Classify each remaining comment as Delete, Keep, Code fix, or Decide. `references/comments.md` defines the exemptions and the classes.

### Commit messages and PR descriptions

History is read-only. Do not rewrite it and do not classify single items. Use the sample to describe the convention in use: structure, length, tense, and how much task history or conversation appears. The remedy is a documented rule and a template.

## 3. Report the current state

Give the user one report with these sections:

1. Summary: three to five sentences on the overall state and the largest problems, measured against the core tenets.
2. Numbers: for documents, instruction file lines, and comments, the total and the count in each class. For comments, add the ten files with the most Delete findings.
3. Instruction files: for each file, its scope, its size, the share of Needed lines, and each needed item that is absent or false.
4. Documentation layout: where documents and decision records live now, and which documents no pointer reaches.
5. False or stale claims, each with file, line, and the evidence.
6. Duplicated facts, each with the files that state it.
7. Code fixes: file, symbol, the surprise, and the fix.
8. Decide items, each with the question for the user.
9. Commit and PR convention as observed.

Mark each number that you estimated from a sample. List each command that you did not run.

## 4. Propose the target state

The proposal tells the user what the documentation should look like. Start from the layout that the team already uses. Propose a structural change only where the current layout breaks a core tenet, and name the tenet.

Include:

- Document map: each docs directory, each file in it, its one purpose, and the facts it owns. List each file to move, merge, or delete, and the destination of its content.
- Instruction files: the full draft of each.
- Comment policy for this repository: the rules in `references/comments.md`, plus local terms and any local exception.
- Commit and PR rules, and templates when they help.
- Code fixes: which fixes to apply in this run and which stay open.
- Enforcement: checks that keep the state, such as a link check or a lint rule. Prefer a check to a sentence.
- Work plan: the units for each pass and the files in each unit.

Stop here. Get approval and an answer for each Decide item before any edit. Apply the changes the user makes to the proposal.

## Before the first edit

- Confirm that the working tree is clean. Work on a new branch.

## 5. Pass 1: documentation, READMEs, and instruction files

Pass 1 comes first because it fixes the terms and the layout that pass 2 follows. Follow `references/documents.md` and `references/writing-rules.md`.

In a small repository, make the edits yourself. In a large one, write the instruction files and each index yourself, then dispatch agents for the remaining documents. Give each agent a set of files that no other agent touches.

## 6. Pass 2: code comments and docstrings

Follow `references/comments.md` and `references/writing-rules.md`.

In a small repository, make the edits yourself. In a large one, dispatch agents by directory or module. Give each agent a set of files that no other agent touches.

For each code fix that the user approved, apply the fix as a separate unit with its own verification, and delete the comment in the same change. For each open code fix, keep the comment and list the fix in the final report.

## Agent briefs

Each agent starts with no context. Give each agent a brief that contains:

- The goal in one sentence.
- The exact list of files. The agent edits no other file.
- The approved target for those files.
- The absolute paths of the reference files to read before it starts. When the agent cannot read the skill directory, copy the contents of those files into the brief in full.
- The local rules and exceptions from the approved proposal.
- The terms to use, from the instruction files and the documentation.
- The return format: files changed, counts for each action, each comment kept with its reason, each reclassification, each new code fix, and each claim the agent could not verify.

## 7. Verify and report

- After each pass, follow the conventions of the repository for changed files, such as its formatter and its linter.
- For pass 2, confirm that the diff changes only comments and docstrings, apart from approved code fixes. Compare the source before and after with comments stripped. When the toolchain can emit output with comments removed, compare that output instead. In a language that keeps docstrings at run time, such as Python, search for code that reads them before you accept a docstring change.
- Run each safe command in the instruction files again. Check each link again.
- Confirm that a pointer reaches each document.
- Take the inventory again. Report the numbers before and after.
- Commit each pass separately when the user asked for commits. Follow the commit rules of the repository.
- Report what changed, the open code fixes, the open Decide items, and each check that failed.
