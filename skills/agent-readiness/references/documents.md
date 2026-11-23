# Documents and instruction files

Use these rules to assess documents in the inventory, to write the proposal, and to edit documents in pass 1. Binding rules, as step 1 of `SKILL.md` defines them, override these defaults.

## Agent instruction files

Keep one canonical file at the root, `AGENTS.md` unless the repository uses a different name.

A tool-specific file must load the canonical file. A Markdown link loads nothing. Use the import that the tool supports, such as `@AGENTS.md` in `CLAUDE.md`. Use a symlink when the tool has no import. Confirm the mechanism in the documentation of each tool.

The root file holds three things:

- Tooling: the exact commands to install, build, test, run one test, lint, format, and type check, and each tool that differs from the ecosystem default.
- Core requirements: the constraints that apply to every task, such as files that agents must not edit, security rules, and the checks that must pass before the work is done.
- Where to find the rest: a pointer to the docs directory or its index, with one line for each area on when to read it.

One or two sentences on what the project is can open the file.

A scoped instruction file holds requirements for one part of the repository. The scope is a subtree for a nested file, or a glob for a rules file that has one. Use a scoped file when a requirement applies to every task in the scope and to no task outside it. For example, `app/AGENTS.md` in a Next.js project can state how to structure pages and layouts in `app/`. The same limit applies: tooling, core requirements, and pointers for that scope only. Do not repeat content from the root file.

Move everything else to on-demand documents and link to it. This includes architecture, procedures, background, and writing rules.

Delete these:

- Rules that a formatter, linter, or type checker enforces.
- General engineering advice.
- Directory listings that `ls` gives, and overviews of the codebase.
- Content that the README or another document states. Link to the owner.

Test each line three times:

- Is it true? Run the command when it has no side effects, or read the code.
- Does it apply to every task in the scope of the file? If not, move the line to a file with a smaller scope, or to an on-demand document.
- Is the reason for the requirement clear? If not, ask the user. Do not delete a requirement because you cannot find its reason.

## Skills and commands

The name and the description of a skill load into every task. The body loads when the skill starts. Files that the body links load when the agent reads them. Assess each level against these rules, from the [skill authoring best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices):

- The description states what the skill does and when to use it, in the third person, in 1,024 characters or fewer.
- The body is shorter than 500 lines and holds only what the agent does not already know.
- Each reference file links directly from `SKILL.md`. A reference file longer than 100 lines starts with a table of contents.
- One term has one meaning in all files of the skill.
- The skill holds no instruction that depends on the date.

## Layout

No layout is required. A layout is good when it satisfies the core tenets and the team can keep it. Use the example below when the repository has no layout, and adapt it freely.

```
AGENTS.md          minimal: tooling, core requirements, pointer to docs/
README.md          what the project is, how to install and use it
app/AGENTS.md      requirements for the app/ subtree only
docs/
  index.md         where to find everything (large projects)
  architecture/    structure, data flow, boundaries
  operations/      deploy, configuration, monitoring
  runbooks/        one procedure in each file
  decisions/       ADRs, append-only
```

- A small project keeps a flat `docs/` with a few files and no index. Add subdirectories when a flat directory becomes hard to scan.
- In a large project, an index lists each document with one line on when to read it. The instruction file points to the index.
- A monorepo can keep a docs directory for each app, such as `apps/<app>/docs/`, with an instruction file or index in each app that points to it. Repository-wide documents stay at the root.

## Decision records

- Keep decision records together in one known directory for each docs root, such as `docs/decisions/`. Move ADRs that sit next to code. Number the files in order.
- ADRs are append-only. Do not edit or delete an accepted ADR. To change a decision, add a new ADR that supersedes the old one, and add the link to the old one.
- The writing rule "state what is true now" does not apply inside an ADR or a changelog, because their purpose is the record.

## READMEs and documentation

- Give each fact one owner. Other files link to the owner.
- Link to authoritative code and configuration. Do not copy their details.
- Update the existing relevant section. A small feature does not need a headline, a new section, or an announcement.
- Delete a document that has no purpose beyond the record of a past task. ADRs and changelogs have that purpose by design and stay.
- Make each document reachable from an instruction file or an index.
- Verify each command, path, symbol, flag, and link against the repository before you keep or write it. Run a command only when it has no side effects.
- Move files with `git mv` so that history follows them. Update each link to a moved file.
- Follow `writing-rules.md`.

## Commit messages and PR descriptions

- State what the change does and why the project needs it.
- Follow `writing-rules.md`.
- Put the convention in an on-demand document. Add a commit or PR template when the team wants one.
