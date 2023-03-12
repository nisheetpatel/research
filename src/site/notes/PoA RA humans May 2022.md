---
{"dg-publish":true,"permalink":"/po-a-ra-humans-may-2022/","created":"","updated":""}
---

[[DRA human experiments\|DRA human experiments]]

# Human memory pilot data
## Suggested changes to the experiment
It seems like subjects categorize options and perhaps learn based on outliers. Additionally, it may be the case that some subjects learnt to exploit the actual values of $\Delta_\text{PMT}$ to perform well in all states.

### New thought
Are we giving away too much information by having a PMT trial directly follow the trial of the same group? Although the shape presented in the PMT trial is not present in the previous trial, maybe they already know what's coming?

### To stop subjects from potentially cheating
1. Round converged $\Delta_\text{PMT}$ value to nearest 0.5
	- otw all good bonus options will end in say 0.8, bad bonus options in 0.2

### To make experiment more difficult
2. Lower initial $\Delta_\text{PMT}$ value, $\Delta_\text{PMT}^0$, down to 1.5 or 2
	- currently $\Delta_\text{PMT}^0 = 4$
	- all subjects converged to $\Delta_\text{PMT}\in\{1.5,3\}$
3. Change adaptive procedure for $\Delta_\text{PMT}$ 
	- to not decay quickly 
	- to not decay all the way down to zero
	- maybe also ignore the first few trials if $\Delta_\text{PMT}^0 = 1.5$
4. Squash the stakes a bit and/or increase the noise (standard deviation) of the reward

### To stop subjects from categorizing stimuli on easy trials (states 1, 3)
5. Remove the colors? 
	- Don't tell the subjects that there are four classes of shapes, at least not explicitly
	- This could make categorization a little more difficult, at least for the stimuli that do not appear so frequently
6. Have a small probability of getting low rewards from the good options and of getting high rewards from the bad options
	- This would prevent subjects from learning based on outliers since the low probability options could also give high rewards now
7. Change the distributions from which the rewards are drawn
	- don't use Gaussians because they have equal tails extending outwards and produce more outliers for the options with values 6, 14, etc.
	- kind of the same idea as the previous point
8. Increase the number of stimuli?
	- Induces more of a memory load such that subjects can't really categorize

## Code
- [x] Test low-cost regime in DRA
	- [x] Does it produce the pattern given by subjects 1 & 5?
- [x] Test low-cost regime on other models, esp. low $\sigma_{base}$ 
	- [x] Could they produce the same pattern?
	- [x] Is that why they aren't distinguishable?
- [x] See performance of all models (in low-cost regime) on regular trials
	- [x] Can that tell us something (since PMT is not informative)? No!