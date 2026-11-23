# Evidence

Read this file when the user asks why instruction files must be minimal. State the limits with the results.

## Lulla et al.

"On the Impact of AGENTS.md Files on the Efficiency of AI Coding Agents" (arXiv 2601.20404).

- The study ran 124 pull requests in 10 repositories with one agent, with and without the root `AGENTS.md`.
- With the file, median wall-clock time was 28.64% lower and median output tokens were 16.58% lower.
- The authors kept only files with information on conventions, on architecture and project structure, or on the project description. The paper does not say if a file had to cover all three.
- The study did not measure correctness. It did not test which content helps.

## Gloaguen et al.

"Evaluating AGENTS.md: Are Repository-Level Context Files Helpful for Coding Agents?" (arXiv 2602.11988).

- The study tested four agents.
- Context files raised average inference cost: by 20% (SWE-bench) and 23% (CTXbench) for LLM-generated files, and by about 19% for developer-written files. The abstract gives "over 20%" for both.
- Agents followed the instructions in the files, which led to more exploration, more testing, and more reasoning.
- LLM-generated files lowered task success slightly, in 5 of 8 settings. The difference was not statistically significant.
- Developer-written files did not change task success significantly. They did better than LLM-generated files.
- Context files did not meaningfully reduce the number of steps before an agent first touched a file from the patch. The authors conclude that context files do not give an effective overview of the repository.
- LLM-generated files helped (+2.7% on average) only when the authors removed the other documentation from the repositories. The authors conclude that the files mostly repeat existing documentation. This run excluded one agent and reports no significance test.
- The authors hypothesize that the additional instructions make tasks harder. For files that people write, they recommend only instructions that the README and the codebase do not already give. For LLM-generated files, they recommend omission for the time being.

## What the studies show

- An instruction file costs effort on every task, because agents follow each instruction.
- A file that a developer writes does better than a file that a model generates.
- LLM-generated files added cost and did not help in the main runs. They helped only when the documentation they repeat was absent.

Thus: keep only what is true, specific to the repository, and not stated elsewhere.

## What the studies do not show

- Neither study compared a short file with a long file.
- No experiment isolated repeated content. The result on repetition comes from LLM-generated files as a whole.
- Neither study tested a minimal instruction file that points to on-demand documents. That structure is the judgment of this skill.
- An instruction file that this skill drafts is LLM-generated. The generated files in the study followed the recommendations of agent developers, with no review. This skill limits the draft to lines that it verified against the repository, and the user approves the draft. Neither study tested a file made that way.
- The two studies differ in tasks, agents, and measures. One measured speed and one measured task success and cost. They do not give one combined result.
