# HWRS640 - Final Project: Proposal Pitch

## Due date: Friday, March 27th at 11:59 PM

## Overview

The final project gives you the opportunity to apply the computational methods from this course to a problem of your own choosing. Over the next 5-6 weeks you will develop a complete data-driven analysis or modeling workflow, culminating in a written report and a short in-class presentation.

This first deliverable is a **~1 page written pitch** that articulates your proposed project. It should be specific enough that the instructor can give you useful feedback before you commit significant time to implementation.

Projects should ideally connect to your own research area, but must not simply reproduce work you have already done or are currently doing — the goal is to apply course methods to a new question, new dataset, or new angle on a familiar problem. New analysis beyond your existing research is also welcome, but may require more careful scoping to be feasible within the time frame.

## Project Requirements

Your project pitch must include all three of the following components:

1. **A dataset** — Real-world Earth science data that you will acquire, process, and analyze. The data should be non-trivial in size or structure (e.g., spatiotemporal fields, multivariate time series, remote sensing imagery). Toy or classroom datasets are not appropriate as the sole data source.

2. **A method** — At least one of the computational methods covered in this course must be central to your analysis. Appropriate methods include (but are not limited to):
   - Dimensionality reduction / EOF / PCA / SVD
   - Dynamic Mode Decomposition (DMD)
   - Deep learning (MLP, CNN, RNN, Transformer, PINN, diffusion model)
   - Physics-informed or hybrid/differentiable modeling
   - Optimization-based approaches
   - Other methods we have not covered but are relevant to data-driven Earth science (discuss with instructor if unsure)

3. **A scientific objective** — A clear question or goal that motivates the analysis. The project should produce a result that is interpretable in an Earth science context, not just a model that runs.


## Proposal Contents (~1 page)

Your pitch should address the following four points. Use these as section headings:

### 1. Motivation and scientific question
What Earth science problem are you addressing? Why is it interesting or important? State your driving question concisely (1-3 sentences).

### 2. Dataset
Describe the data you plan to use:
- What it is (variable(s), domain, temporal/spatial resolution)
- Where it comes from (source, how you will access it)
- Any known challenges (missing data, large file sizes, preprocessing needed)

### 3. Proposed method
Describe the computational approach you plan to apply:
- Which method(s) from the course will you use, and why are they appropriate for this problem?
- What will the inputs and outputs of your model or analysis be?
- If you are using deep learning, describe the architecture you have in mind (even if preliminary).

### 4. Expected outcomes and evaluation
What do you expect to find? How will you know if your method worked? Describe at least one concrete evaluation metric or diagnostic plot you plan to produce.

## Submission Instructions

Submit your proposal as a **PDF or Markdown file** through D2L by the due date. One page is the target; two pages is the hard limit. Figures or diagrams are welcome but not required.

You will receive written feedback within one week. After the proposal, subsequent project milestones are:

| Milestone | Due |
|---|---|
| Project proposal (this assignment) | Mar 27 |
| Dataset and preprocessing report | Apr 24 |
| Report on data and model inductive biases | May 1 |
| Final report + presentation | Week of May 4 |

## Grading

The proposal is graded as part of the final project (20% of course grade). The pitch itself will be evaluated on:

| Criterion | Weight |
|---|---|
| Scientific clarity — is the question well-defined? | 30% |
| Dataset — is it appropriate and feasible to obtain? | 25% |
| Method — is the chosen approach well-motivated? | 25% |
| Feasibility — can this realistically be completed in 5-6 weeks? | 20% |

## Tips and Scope Guidance

- **Scope down, not up.** A focused project that works is much better than an ambitious project that doesn't. If you are unsure, start simple and add complexity.
- **Data first.** Make sure you can actually access and load your dataset before the proposal is due. Many public datasets have quirks (format, access restrictions, size) that are only discovered when you try to use them.
- **Connection to your research.** You are encouraged to use data from your own research domain, but the analysis must go beyond reproducing existing results. Examples of good reuse: applying DMD to a dataset you normally analyze with other tools; training a neural network on observations you have already collected; using PINNs to emulate a model you normally run.
- **Come to office hours.** If you are uncertain whether your idea is appropriate or feasible, discuss with the instructor before the proposal deadline.
