---
{"dg-publish":true,"permalink":"/slot-machines-pilot-v3-change-log/","created":"","updated":""}
---


# Slot machines pilot v3

[Here](https://pavlovia.org/nisheetpatel/slot-machines-for-memory) is the link to test the experiment on Pavlovia. It is much cleaner than v2.

## Questions for Antonio before piloting

There are a couple of questions remaining:

1. Should we split the experiment fee to save money (if it is possible on Prolific)?
	- $10 for participation
	- $15 for completion
	- $0-25 performance bonus (most likely around $5)
2. Should we make the Return (on feedback screen) grey?
	- when I run myself through the experiment now, I find myself looking just at the "Return". Then for each slot machine, I just need to see whether the price is higher or lower than my estimate. Doing this defeats the purpose of the experiment because participants ignore the frequency of the different prices which sets the stakes/difficulty.
3. Should we reformat the stimulus screen to look exactly like the feedback?
	- The formatting for the feedback has been vastly improved. However, I think as a participant, I lose a little bit of time to switch my gaze from the stimulus screen (with the big slot machine and the price below it) to the feedback screen (with the smaller slot machine and the price in a different location along with other win/loss info). I think it might help to keep the location of the slot machine and the price exactly the same from the stimulus screen to the feedback screen so that participants can simply pay attention to the additional information to learn. What do you think?


## Change log

### New

#### Trial timer

The timer is a very subtle line that decreases in length until the time is up. Subjects are very bad at estimating 2 seconds, so we include it to help them.

- [x] Add a timer to indicate the time left for each decision.

#### Test

Participants must answer *4 of 6* questions correctly to proceed. Moreover, after they answer the questions, they are shown the feedback to let them know whether their answers were correct or not, and if they were incorrect, they are shown the correct answer. If the participants are unable to answer 4 questions correctly, the experiment ends and they get the participation fee.

###### Updates

- [x] Add all the test questions (noted below)
- [x] Provide feedback (correct/incorrect) for each of the questions
- [x] Add a screen showing the test result at and end 
- [x] End experiment, i.e. skip to payoff screen, if test failed

###### Screen 1

1. How many slot machines are there in total?
	- Options: (1, 2, 3, ==4==) 
2. If the blue slot machine appears on the top-right of the screen, would it always appear in the that location throughout the experiment?
	- Options: (==Yes==, No) 
3. Can you speed up the experiment by responding quicker than 2 seconds?
	- Options: (Yes, ==No==) 

###### Screen 2

Image for questions 4-6:

![[Pasted image 20230213023701.png\|Pasted image 20230213023701.png]]

4. How much would you earn if you chose NOT to play by responding "No"?
	- Options: (==0==, 3, 5, 8)
5. How much would you earn if you chose to play by responding "Yes"?
	- Options: (0, ==3==, 5, 8)
6. How much would you earn if you were too slow to respond?
	- Options: (==0==, 3, 5, 8)


### Modified from v2

#### Experiment parameters

- [x] Reduce $\sigma: 2.5\rightarrow2$
- [x] Trial timing:
	- [x] Fixation cross: 0.5s $\rightarrow$ 0.5s (unchanged)
	- [x] Stimulus: 0-2.5s $\rightarrow$ 0-2s
	- [x] Feedback: 1.5s $\rightarrow$ 2s
	- [x] ITI: 0-2.5s $\rightarrow$ 0-2s

#### Feedback

Keeping the feedback almost exactly the same for both yes and no choices to make sure subjects update values in the exact same manner. Add an extra textbox showing "Earn: 0" in the last 0.5s of the feedback to show their actual earnings.

- [x] Yes: Price >Return > line > Win/Lose
- [x] No: Price > Return > line > Win/Lose > Earn 0 (last 0.5s)
- [x] Formatting:
	- [x] All font sizes same
	- [x] Yes/No appear in white, not grey
	- [x] Left align all text (Price, Return, Win/Loss, Earn)
	- [x] Right align all numbers
	- [x] Show numbers with 2 decimal points, e.g. 4.5 shows up as 4.50

#### Instructions

- [x] After consent, before instructions: add a page telling them about test
- [x] Change "Please respond faster" time to two seconds
- [x] Update slides with stimulus to include timer
- [x] Update slides feedback to show new feedback format
- [x] Update "Please respond faster" instruction slide: 
	- [x] remove price/return/win numbers and put ? instead (no feedback)
	- [x] explain how the timer works