---
{"dg-publish":true,"permalink":"/results-for-slot-machines-pilot-v3-1/","created":"","updated":""}
---


#DRA #human-memory #slot-machines 

We find that most subjects perform at chance level and of the ones that don't, about 40% of them could be classified as DRA but we're not sure what the others do. Crucially, this is not very correlated with the reward they get on the task.

## Analyses

#### Story

- Analyze the performance of each of the subjects and create two populations based on subjects' performance:
	- at or below chance
	- significantly above chance
		- for each subject, create a null distribution:
			- replace their choices with random responses for the test blocks
			- do this 1000 times and pick 95% CI
		- alternatively, maybe we don't do this for each subject if their distributions look the same
- Mark the ones above chance and the ones below
- Build a hierarchical Bayesian logistic regression model for the each population
- Do a sanity check that the ones performing at or below chance indeed have $\beta_i\leq0\ \forall\ i$
- For the good subjects, see which ones pass which tests
- Split them again into ones that pass the test for DRA vs ones that don't
- Build another hierarchical model for each sub-population
- For each subject in the each group, use posterior fits and psytrack to see if there are trends in the data relating to how they learn:
	- for the DRA group:
		- whether they all learn in the same way, or
		- whether there is still some who learn 2 better and 3 and vice versa but 1 and 4 are extremes
	- for the non-DRA group: 
		- whether they only learn about a subset of slot machines, which is what effectively leads to a "random" ordering of slopes at the hierarchical level

#### Roadmap

 - [x] Bayesian Hierarchical Logistic Regression
	- [x] With common bias $\alpha$
	- [x] With independent bias for each slot machine
 - [x] Model prob. after sorting subjects by reward
 - [ ] Significance test for difference from chance to discard subjects
 - [ ] Psytrack for individual subjects
 - [ ] Plan analysis code refactoring
 - [ ] Analyze non-DRA subjects
	 - [x] Group-level
	 - [ ] Individually
	 - [ ] Psytrack
 - [ ] 