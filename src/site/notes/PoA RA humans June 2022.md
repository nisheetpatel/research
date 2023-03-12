---
{"dg-publish":true,"permalink":"/po-a-ra-humans-june-2022/","created":"","updated":""}
---

[[DRA human experiments\|DRA human experiments]]

# Human memory pilot data
Initial analysis from [[PoA RA humans May 2022\|May 2022]] made it seem like subjects categorize options and perhaps learn based on outliers. Additionally, it may be the case that some subjects learnt to exploit the actual values of $\Delta_\text{PMT}$ to perform well in all states.

However, Antonio thinks that this conclusion is premature and would like to have a closer look at the data. He wants to see whether what the subjects are doing makes any sense at all. I think he has a very good sense of when something is off and is very good at telling apart whether what we want to measure is actually what we are measuring. We are going to examine a whole bunch of diagnostic plots.

## 1: Background: Experiment details
### 1.1: Options/shapes
There are 12 options/shapes in the experiment explicitly grouped into four sets $\{s_1,s_2,s_3,s_4\}$ that consist of three options/shapes each. In the experiment, each set is visually distinguished by a unique color from a colorblind-friendly palette. The sets have a factorized design as shown in the table below with two sets $\{s_1,s_2\}$ having a higher frequency of occurrence $\pi(s)$ and two sets $\{s_1,s_3\}$ having higher stakes $\delta(s)$. 

|              | $\delta(s)=4$ | $\delta(s)=1$ |
| ------------ | ------------- | ------------- |
| $\pi(s)$=0.4 | $s_1$         | $s_2$         |
| $\pi(s)$=0.1 | $s_3$         | $s_4$         |


### 1.2: Trial types
We have two types of trials in our experiments:
- Regular trials
	- consist of a 2AFC between two shapes of the same color/set
- PMT (precision-measuring task) trials or bonus trials
	- consist of a 2AFC between a shape and a number
	- number $\in\{\text{reward}(\text{shape})+\Delta_\text{PMT},\ \text{reward}(\text{shape})-\Delta_\text{PMT}\}$

### 1.3: Session types
We have four types of sessions in our experiment:
- Training session:
	- regular trials only
	- 210 trials
- Refresher session:
	- meant to refresh subjects memory on day 2
	- 30 trials
- Testing session:
	- regular trials are randomly interspersed with PMT trials
	- about ~20% of trials are PMT trials
- Adaptive testing session:
	- same trial structure as testing sessions, but $\Delta_\text{PMT}$ varies from trial to trial
	- to select a value of $\Delta_\text{PMT}$ based on subject's performance

### 1.4: Session and day schedule
The experiment is organized as follows:
- Day 1:
	- 2 $\times$ training sessions
	- 1 $\times$ adaptive testing session
	- 3 $\times$ testing sessions
- Day 2:
	- 1 $\times$ refresher session
	- 7 $\times$ testing sessions

## 2: Plots to make
Abbreviations:
- $CA$: Choice Accuracy
- $RT$: Reaction Time
- $R$: Reward

### 2.1: Summary statistics of diagnostic variables
- $y\in \{CA,\ RT\}$
- $x\in\{s_1,s_2,s_3,s_4\}$ 

For each subject, for the following splits:
- Training sessions
	- split/not by session id
- Testing sessions
	- Regular trials
	- PMT trials

### 2.2: Time evolution of diagnostic variables
- $y\in \{CA,\ RT,\ R\}$
	- Not the actual time-step values, but instead a moving average of them
- $x\in$ {Absolute trial number, relative index of filtered dataset}

For each subject, with the following splits:
- Regular trials
	- Subject
	- State
	- Difficulty in each state
	- All 3 separations in each state
- PMT trials
	- Subject
	- State

### 2.3: Correlation between performance on tasks
We want to see whether the subjects performance on the two tasks (Regular trials vs PMT trials) is correlated. We will examine the correlations separately for each subject and for each state. The final plot will consist of $N_\text{subjects} \times N_\text{states}$ panels:

- $y\in$ Moving average of $\{CA,\ RT\}$ on PMT trials
- $x\in$ Moving average of $\{CA,\ RT\}$ on regular trials

### 2.4: Memory effects
- $y\in$ $\{CA,\ RT\}$ for current trial
- $x\in$ $\{CA,\ RT\}$ for previous trial of the same state as current trial

For each subject, for the following splits:
- Regular trials only
- PMT trials  w.r.t previous PMT trial
- PMT trial w.r.t. previous regular trial (always previous trial in experiment)


## 3: June 9th meeting notes
- [ ] Plot moving average of CA for all plots in correlations folder, not correct
- [ ] Send Antonio all plots in single folder (link to folder)
- [ ] Verify and plot model predictions on regular vs PMT task
- [ ] Send Antonio:
	- [ ] latest version of theory paper
	- [ ] powerpoint (pdf) with all model/theory predictions
	- [ ] model predictions on regular vs PMT task