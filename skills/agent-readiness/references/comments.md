# Comments and docstrings

Use these rules to classify comments in the inventory and to edit them in pass 2. Binding rules, as step 1 of `SKILL.md` defines them, override these defaults. A lint rule that requires docstrings is one example.

## Exempt comments

Exempt comments need no judgment. Set them aside first. Count them. Do not classify or edit them.

- Legal or license headers.
- Directives that a tool reads, such as shebangs, build tags, pragmas, and code generation markers. These are code.
- Formatter directives such as `// prettier-ignore`, `# fmt: off`, and `#[rustfmt::skip]`.
- Issue or RFC links that explain a constraint the code cannot express, and a `TODO` or `FIXME` that has an issue link.

## Classes

For each remaining comment, ask one question: what would a competent reader misunderstand without this?

| Class | Applies when the comment |
| --- | --- |
| Delete | Repeats names, types, control flow, rendered UI, or a nearby comment. Records implementation history, an abandoned alternative, the task, or a conversation. Gives a rhetorical explanation or an unsupported claim. Holds commented-out code. Draws a section banner. |
| Keep | Explains behavior that an external dependency, platform, vendor, or protocol forces, and names the external cause. Explains non-obvious domain meaning, a compatibility requirement, an external contract, a unit, an ordering requirement, or failure behavior. Defines a public API contract. Suppresses a lint rule that is faulty, pedantic, or style-only. |
| Code fix | Explains a surprise in the project's own code. Suppresses a sound lint rule. A rename, an extraction, a type, or a fix to the code removes the need for the comment. Record the symbol and the fix. |
| Decide | Contradicts the code. Makes a claim that you cannot verify. Is a `TODO` or `FIXME` with no issue link. Is a public API doc comment that only restates the signature. |

Verify each Keep comment against the code and the relevant callers. When a comment contradicts the code, the comment or the code can be wrong. Classify it as Decide and report both readings.

Docstrings follow the same classes:

- A doc comment on a public API is a Keep when it states something the signature does not: behavior, units, errors, or limits.
- A doc comment on a public API that only restates the signature is a Decide. A documentation generator or a lint rule can need it.
- A doc comment on private code that only restates the signature is a Delete.

## Edit rules for pass 2

- Apply the classifications from the approved inventory. You can reclassify a comment after you read the code. Report each reclassification.
- Delete each Delete comment.
- Rewrite a Keep comment only when it is correct but long or imprecise. State the constraint in short sentences. Do not change a comment to agree with the code when the two contradict.
- Leave each exempt comment as it is.
- Leave each Code fix comment and each Decide comment as it is, unless the brief tells you what to do with it.
- Change only comments and docstrings. The diff changes no behavior.
- Follow `writing-rules.md` for each comment that you write.
