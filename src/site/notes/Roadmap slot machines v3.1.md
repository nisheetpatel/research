---
{"dg-publish":true,"permalink":"/roadmap-slot-machines-v3-1/","created":"","updated":""}
---


### 3.1.0

##### Experiment

- [x] Get experiment ready to collect data
- [x] Collect data for 45 extra subjects
	- [x] Seems like we only have data for 42 subjects; 1 failed the test
- [x] Check data and tell Liz about missing data

##### Code

- [x] Independent hierarchical models for each slot machine
- [x] function for cleaning and organizing data for stan
- [x] function to test for model signatures


### 3.1.1

- [x] Code proper hierarchical models
	- [x] joint distribution for betas with common alpha
	- [x] joint distribution for betas and alphas

### 3.1.2

#### Major refactoring needed

- [x] `process`
	- [x] Separate `get_dataframes()`:
		- [x] `load_choice_data()`
		- [x] `load_instructions_data()`
- [ ] 

## Goal for analyses

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