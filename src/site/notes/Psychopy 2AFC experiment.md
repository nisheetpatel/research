---
{"dg-publish":true,"permalink":"/psychopy-2-afc-experiment/","created":"","updated":""}
---


[[DRA human experiments\|DRA human experiments]]

# PsychoPy experiment

[Memory 2AFC Github repository](https://github.com/nisheetpatel/2AFC-memory-experiment)

## Archive

### New features to add
- [x] Same objects, rewards for a given subject (seed set by subject id)
- [x] Shuffle trial sequence across sessions for given subject
- [x] Shuffle sets across subjects
- [x] Change from random blocks to fully random
- [x] Repeat trials where subject took too long ([link](https://discourse.psychopy.org/t/repeat-skip-trial-to-replay-later/909))
- [x] Adaptive staircase on PMT trials
- [x] Make config file separate
	- Monitor specifications
	- Stimuli colors
	- Stimuli positioning
	- Window specifications
	- Distance from monitor
	- Background color
- [x] Change colors to be from a [colorblind](https://jfly.uni-koeln.de/color/) [friendly](https://davidmathlogic.com/colorblind/#%23000000-%23E69F00-%2356B4E9-%23009E73-%23F0E442-%230072B2-%23D55E00-%23CC79A7) [palette](https://thenode.biologists.com/data-visualization-with-flying-colors/research/)
- [x] Dark background, no borders

### Archive
- [x] Create new github repo and virtualenv
- [x] Sketch out all classes needed OOP style
	- [x] TrialRoutine
	- [x] ScreenText (ABC)
	- [x] Stimulus (ABC)
		- [x] NonChoiceOptions
			- [x] Fixation
			- [x] FeedbackChoice
			- [x] FeedbackReward
		- [x] ChoiceOptions
			- [x] Shape
			- [x] Number
	- [x] AssignStimuliToSets
- [x] Create functional training session
- [x] One PMT trial
- [x] Write condition set for PMT trial/test session
- [x] Create functional test session
- [x] Include instructions
- [x] Q/escape to exit
- [x] Include example session
- [x] ABC for all on-screen objects
- [ ] enum to restrict types, e.g. newPos can only take on 'left' or 'right'

## Notes
- Known issue for Linux: Psychopy doesn't run without wxPython, which cannot be specified in the requirements, but needs to be manually downloaded for your specific machine ([link](https://github.com/psychopy/psychopy/issues/2418))

#### Archived: Initial thoughts
- adjust sizes according to Rangel's comments
- Run training set first with collection of data
	- trials should be randomized in a session of 180, i.e. nReps = 15 
	- stimuli should appear on both sides randomly

## Antonio's remarks
