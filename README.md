# Canon Drift

## Objective

Test whether different prompt and context workflows produce more internally consistent, less repetitive AI generated storytelling, and check whether the best workflow is the same across different language models.

The project simulates a small alternate history sandbox game, run turn by turn under three different prompting strategies and three different models, then scores the results with automated evaluation instead of relying on manual read through.

## Background

Interactive AI storytelling tools run the same default prompting logic across many different models, but models do not all behave the same way. A workflow that keeps one model consistent over a long story may not help, or may even hurt, a different model.

This project focuses on two things a storytelling platform needs to measure:

1. **Consistency.** Does the story contradict facts it already established earlier, such as a character, alliance, or outcome.

2. **Novelty.** Is the story developing new content turn to turn, or repeating itself.

## Method

1. Define a single alternate history scenario, "the Western Roman Empire survives in 476 AD," with a world state object that tracks established facts and characters as the story progresses.

2. Run the story across 8 scripted player actions per playthrough.

3. Build three prompting workflows: naive raw history dump, structured fact injection, and structured facts plus an explicit self consistency check instruction.

4. Run every workflow against three different models, for 9 full playthroughs total.

5. Use a judge model to read each complete transcript and return a structured contradiction count with short examples.

6. Score turn to turn word overlap as a lightweight proxy for repetition versus creative development.

7. Combine both scores into a single comparison table.

8. Visualize the results per model and per workflow to compare consistency against repetition.

## Results

### Contradiction Count by Model and Workflow

The first chart compares how many contradictions each model produced under each workflow.

### Consistency vs Repetition Tradeoff

The second chart plots contradiction count against turn overlap for every model and workflow, showing where a workflow improved both metrics at once, and where it traded one for the other.

### Average Contradictions per Workflow

The third chart aggregates contradiction count across all three models per workflow, to show the overall trend before looking at per model differences.

## Validation

The judge model scores each story independently of the original narration model, and returns a structured count plus short quoted examples of the contradiction rather than a single opaque score, so every flagged issue can be checked against the transcript directly.

The novelty metric is computed with plain word overlap rather than an LLM judgment, so it is fast, free, and reproducible without depending on another model call.

## Key Finding

No single workflow wins across all models.

The strongest model was contradiction free under every workflow tested, meaning workflow choice mattered less for it on a short scenario. A mid sized model improved under structured fact injection but got worse when an explicit self check instruction was added. A different model showed the opposite pattern, going from multiple contradictions down to zero once the self check instruction was added, with only a small increase in repetition.

This mirrors the real problem behind running AI storytelling across dozens of models: a uniform default workflow does not serve every model equally well, and a per model workflow strategy likely outperforms one default applied everywhere.

## Deliverables

`Canon_Drift.ipynb`, the complete notebook that runs from top to bottom in Google Colab

Comparison table of contradiction count and turn overlap across all 9 model and workflow combinations

Three charts comparing consistency and repetition across models and workflows

Written summary of findings

## Why this is useful

Any platform running AI driven, player facing narratives across multiple models faces the same tension this project measures, internal consistency against creative development, and that tension behaves differently model to model.

Canon Drift is a small, reproducible way to quantify that tradeoff with real scores instead of manual read through, and the same approach scales directly to more models, longer scenarios, and real gameplay data.

## How to run it

Open `Canon_Drift.ipynb` in Google Colab and run the cells in order from the top.

A free [Groq API key](https://console.groq.com) is required and will be requested inside the notebook. No other setup or external dataset is needed.

## Built with

Python, Groq API, pandas, matplotlib, Google Colab, and LLM as judge evaluation.

## Possible next steps

Testing across additional models and longer scenarios, replacing the word overlap novelty metric with an embedding based similarity score, and testing workflows on real player authored scenarios instead of a single scripted one.

## Author

**Sourabh Gopinath More**

MS Computer Science [LinkedIn](https://www.linkedin.com/in/sourabhmore73/) | [Portfolio](https://sourabhmore.carrd.co/) | [GitHub](https://github.com/sgm7373)
