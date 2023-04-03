---
layout: default
title: Designing studies
parent: Probability and statistics
grand_parent: Data science notes
permalink: /docs/data-science-notes/probability-and-statistics/study-design/
nav_order: 7
---

# Designing studies

## Sampling methods

In a statistical study, **sampling methods** refer to how we select members from the population to be in the study.

If a sample isn't randomly selected, it will probably be **biased** in some way and the data may not be representative of the population.

### Simple random sample (SRS)

Every member and set of members has an equal chance of being included in the sample. Technology, random number generators, or some other sort of chance process is needed to get a simple random sample.

Why it's good: Random samples are usually fairly **representative** since they don't favor certain members.

### Stratified random sample

The population is first split into groups. The overall sample consists of some members from every group. The members from each group are chosen randomly.

Why it's good: A stratified sample guarantees that members from each group will be represented in the sample, so this sampling method is good when we want some members from every group.

### Cluster random sample

The population is first split into groups. The overall sample consists of every member from some of the groups. The groups are selected at random.

Why it's good: A cluster sample gets every member from some of the groups, so it's good when each group reflects the population as a whole.

## Biases

* **Voluntary response sampling**: Bias resulting from members of the population deciding to whether or not to be sampled.
* **Convenience sampling**: sampling from that part of the population that is close to hand.
* **Selection bias**: bias introduced into a study by the way individuals, groups or data are selected for analysis, if such a way means that true randomization is not achieved, thereby ensuring that the sample obtained is not representative (under-coverage or over-coverage) of the population intended to be analyzed.
* **Participation bias** or **non-response bias**: the participants disproportionately possess certain traits which affect the outcome.
* **Response bias**: when people are systematically dishonest when answering a question (e.g. because respondents are afraid to respond honestly, etc.).
* **Biased wording**: wording causing people to favor certain responses over others.

## Study methods

- In an *observational study,* we measure or survey members of a sample without trying to affect them.
- In a *controlled experiment,* we assign people or things to groups and apply some treatment to one of the groups, while the other group does not receive the treatment.

## Experiments

### Definitions

* An **explanatory variable** explains changes in another variable.
* A **response variable** measures the result of a study.
* A **treatment** is the specific level of the explanatory variable given to individuals in an experiment. If there are multiple explanatory variables, a treatment is a combination of specific levels from each explanatory variable.
*  The group that does not receive the treatment (no treatment or the standard treatment) is called the **control group**.
* An **experimental unit** is who or what we are assigning to a treatment. Treatments are applied to experimental units in the **treatment group**.
* Input variable are called **independent variables**.
* Output variables (or response variables) are called **dependent variables**.
* Variables that need to remain constant throughout the experiment to prevent external factors from affecting the results are called **control variables**.
* A **blind** or **blinded**-**experiment** is an experiment in which information about the test is masked (kept) from the participant, to reduce or eliminate bias, until after a trial outcome is known. If both tester and subject are blinded, the trial is called a **double-blind** experiment.

### Principles of experiment design

* Subjects in the control group and the treatment group need to be **randomly assigned**. Random assignment creates roughly similar groups by approximately balancing potentially confounding variables between the two groups.
* Subjects in the control group are normally given a **placebo treatment** in order that a difference between the two groups may not be due to the placebo effect as people in an experiment will often respond to any treatment at all, even a treatment that has no real therapeutic value. A third **nontreatment control group** can be used to measure the **placebo effect**, as the difference between placebo control group and the nontreatment control group.
* A **blind** or **blinded**-**experiment** is an experiment in which information about the test is masked (kept) from the participant, to reduce or eliminate bias, until after a trial outcome is known. If both tester and subject are blinded, the trial is called a **double-blind** experiment. The blinding of the researchers protects against potential bias that could arise if researchers favor one type of treatment over another.

## Scope of conclusions

The table below summarizes what type of conclusions we can make based on the study design.

|                          | Random sampling                                              | Not random sampling                                          |
| ------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **Random assignment**    | <u>Can determine causal relationship</u> in **population**. *This design is relatively rare in the real world.* | <u>Can determine causal relationship</u> in that **sample only**. *This design is where most experiments would fit.* |
| **No random assignment** | Can detect relationships in **population**, but <u><u>cannot determine causality</u>. *This design is where many surveys and observational studies would fit.* | Can detect relationships in that **sample only**, but <u><u>cannot determine causality</u>. *This design is where many unscientific surveys and polls would fit.* |

