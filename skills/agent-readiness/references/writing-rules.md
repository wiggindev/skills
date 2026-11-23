# Writing rules

These rules apply to all text that goes into a file or into a message that other people read: comments, docstrings, documentation, READMEs, commit messages, PR descriptions, and agent instruction files. Binding rules, as step 1 of `SKILL.md` defines them, override these rules.

- Follow ASD-STE100 Simplified Technical English loosely, at about 80%. Do not check words against the STE dictionary. Break a rule when the strict form is awkward or less clear.
- Use short sentences. Use one word for one meaning. Use the active voice. Use the imperative for instructions. Write one instruction in each sentence. Technical names and terms from the codebase are permitted.
- Write for a reader who did not see the conversation. State what is true now.
- Remove task history, dropped approaches, things that did not change, and descriptions of old behavior, such as "Kept the existing retry logic" or `// no longer uses the cache`.
- Mention an absence only when the reader expects the thing to be there. An unnecessary negative suggests that something was expected.
- Keep documentation concise and factual. Do not use slogans or promotional language.
- Do not hard-wrap Markdown paragraphs.
