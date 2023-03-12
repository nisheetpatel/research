---
{"dg-publish":true,"permalink":"/basic-analyses-for-slot-machines-task/","created":"","updated":""}
---

For each subject:

1. Psychometric curves of response for each slot machine
	- %(response yes) vs difficulty
2. Psychometric curves of reaction times for each slot machine
	- %(response yes) vs difficulty
3. Aggregate choice accuracy for each slot machine
	- %(correct) vs state
4. Aggregate reaction times for each slot machine, difficulty
	- RT vs state

#### Sanity checks

5. Training vs test splits
6. Whether the slot machine ids and images are consistent within participants but randomized across participants
7. Whether correct response is coded properly
8. Whether the states are coded properly
9. Whether the distribution of trial frequency/difficulty is correct across all participants
10. How long did they spend reading the instructions
11. How quickly do they respond?
12. Correlations of reaction times with choice accuracy
13. How much time did participants spend reading instructions?

### Code

- [x] get all csv files in directory greater than 1 kb
- [x] for each csv file, do:
	- [x] instructions validity check:
		- [x] is it possible to get total RT?
		- [x] do columns match a template?
	- [x] data validity checks:
		- [x] total number of trials
		- [x] other points in sanity checks section
- [x] mark all valid csv files
- [x] read and merge all in a dataframe
- [x] add following columns:
	- [x] human readable participant id (numbers 1-10)
	- [x] block id (categorical {1,2,3,4})
	- [x] block type (binary train/test)
	- [x] difficulty (categorical {2, 1, -1, -2})
	- [x] response (binary yes/no)

- [x] Plots 1-4


### Logistic regression

- [x] Does participants' CA/RT/Response depend on:
	- [x] Slot machine
	- [x] Difficulty (mean payoff - price)
	- [x] mean payoff
	- [x] price
	- [x] time spent reading instructions