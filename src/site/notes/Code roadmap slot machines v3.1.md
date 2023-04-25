---
{"dg-publish":true,"permalink":"/code-roadmap-slot-machines-v3-1/","created":"","updated":""}
---


## Analyses

Next up: [[Slot machines model fitting methods\|Slot machines model fitting methods]]

#### v3.2.0

- [x] Boilerplate code for likelihood computation
- [ ] Compute gradient update analytically for models



## Archive

#### Goal for analyses

Our goal is to do the following:

- Fetch data
- For each block individually, blocks 2-4, blocks 3-4:
	- Fit hierarchical model to data & draw 100,000 samples
	- Plot posterior over betas
		- at group level
		- for each individual
	- For varying similarity thresholds, plot test pass percent:
		- at group level
		- for each individual

- Pause and ponder

- Tag individuals based on tests passed
	- DRA (strict)
	- DRA (loose)
	- Frequency
	- Stakes
	- Equal Precision
	- None
- Remove individuals that pass none of the tests and re-run analyses

##### Data to use

- v1: 10 subjects
- v2: 10 subjects
- v3: 19 subjects
- v3a: 32 subjects
- v3 + v3a: 51 subjects
- v2 + v3 + v3a: 61 subjects

##### Models to fit

- Joint betas, common alpha
- Joint betas, joint alphas

#### Code

##### 3.1.0

- [x] Experiment
	- [x] Get experiment ready to collect data
	- [x] Collect data for 45 extra subjects
		- [x] Seems like we only have data for 42 subjects; 1 failed the test
	- [x] Check data and tell Liz about missing data
- [x] Code
	- [x] Independent hierarchical models for each slot machine
	- [x] function for cleaning and organizing data for stan
	- [x] function to test for model signatures

##### 3.1.1

- [x] Code proper hierarchical models
	- [x] joint distribution for betas with common alpha
	- [x] joint distribution for betas and alphas

##### 3.1.2

*Major changes*

- [x] `process`
	- [x] Separate `get_dataframes()`:
		- [x] `load_choice_data()`
		- [x] `load_instructions_data()`
- [x] `filter`
	- [x] performance metrics
	- [x] filterer
- [x] `hierarchical`
	- [x] stan hierarchical model fit, save, load
- [x] `workbench_psytrack`
	- [x] functional psytrack fitting & plotting

##### 3.1.3

- [x] Plot posteriors over beta for subjects above and at/below chance 
	- [x] Group-level
	- [x] Individual, ordered by performance
- [x] Plot correlation of beta with performance
	- [x] Color each slot machine or separate plot
	- [x] At group-level

##### 3.1.4

- [x] Document results & send
- [x] Refactor code (refer 3.1.x)
- [x] Merge `process`, `filter`, `hierarchical.clean_data`

##### 3.1.5

- [x] Make a module for `psytrack`
	- [?] Multiple runs with different starting points
	- [?] Verify that this avoids flat lines failure mode

##### 3.1.5

- [x] Fix paths by including `definitions` and using `pathlib`
- [x] Write a script to do analyses from scratch
- [x] Delete all old figures or move to archive
