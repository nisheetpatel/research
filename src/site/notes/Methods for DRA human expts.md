---
{"dg-publish":true,"permalink":"/methods-for-dra-human-expts/","created":"","updated":""}
---

[[DRA human experiments\|DRA human experiments]]

# Task
## Basic task
On each trial, subjects are forced to choose from one of two available stimuli on the screen, a paradigm known as two-alternative forced choice (2AFC). There are a total of twelve distinct stimuli that are grouped into four groups of three such that only stimuli from within a group are presented at the same time. We refer to the groups as states and the stimuli as options. The mean payoff for all options is 10 reward points.

Across trials, the state is chosen with a certain probability of occurrence $\pi(s)$, and then two of the three options from that state are chosen uniformly at random. Thus, the transitions across states are independent of the subjects' choice, the previous state, and time. Formally, $p(s'|s,a)=\pi(s')$. 

In addition to the states varying in their probability of occurrence $\pi(s)$, they also vary in what is at *stake* if subjects choose the wrong option, i.e. the difference in the mean rewards offered for each of the options at that state, $\delta(s)$. Concretely, the mean payoffs for the three options in a state $s$ are $\{10+\delta(s), 10, 10-\delta(s)\}$. The four states thus have a factorized representation of the probability of occurrence and the stakes at the state as shown in the table below.

|              | $\delta(s)$=4 | $\delta(s)$=1 |
| ------------ | ------------- | ------------- |
| $\pi(s)$=0.4 | $s_1$         | $s_2$         |
| $\pi(s)$=0.1 | $s_3$         | $s_4$         |

### Training and test phase
Subjects go through a training phase of 500 trials** during which they learn about the options' payoffs. After this, they go through a test phase of 1000 trails* which follows the exact same procedure except that on some pre-select trials (N=20 x 12 options = 240 of the 1000 test trials), there is a bonus choice that the subjects have to make. This task is specifically designed to test the precision with which subjects remember the payoffs for each of the options. Thus, we call it the precision measuring task (PMT).

** this is the number we used for simulations, which use a single temporal difference (TD) update to the mean values encoded in memory after each trial, but perhaps humans may be much quicker at learning.

## Precision measuring task (PMT)
On the pre-select set of 240 trials during the testing phase, subjects are given a bonus choice after the standard 2AFC choice. This choice is also a 2AFC but it is between the third option from the state, i.e. the one that was not shown during the main task, and a deterministic reward that appears as a number on the screen. The payoff of the deterministic reward is $\Delta_{PMT}$ points more or less than the standard option. The trials are pre-selected such that each of the 12 options gets 20 bonus trials against a deterministic option, 10 of which offer $\Delta_{PMT}$ points more than the mean payoff of the option in question, and 10 which offer $\Delta_{PMT}$ points less. Doing this allows us to measure the precision with which subjects encode the value of each of the options since it can now be computed in closed form given the following equations:
$$ 
\begin{align*} 
p(\text{choose det.}) &=p(v_i<\mu_i+\Delta_{PMT})\\
 &=p\bigg(\frac{v_i-\mu_i}{\sigma_i} < \frac{\Delta_{PMT}}{\sigma_i} \bigg)\\
 &=1-\Phi\bigg(\frac{\Delta_{PMT}}{\sigma_i} \bigg)
\end{align*} 
$$
where $i$ indicates the index of the option whose value is assumed to be encoded as $v_i \sim N(\mu_i,\sigma_i^2)$. Since the will observe the $p(\text{choose det.})$, and we as experimenters set the $\Delta_{PMT}$, the only unknown in the equation is $\sigma_i$, which is directly related to the precision of the option $i$ as $\lambda_i = \frac{1}{\sigma_i^2}$.

Though this seems a little involved, it is important to note that it is only so for us as the experimenters. The subjects simply see some 2AFC trials where they choose between one of two stimuli on screen, and on some randomly chosen trials, there is a bonus choice where they could choose between a stimulus and a deterministic, numerical reward. 

###### Antonio's paper: methods
In the initial rating phase subjects entered liking ratings for 70 different foods using an on-screen slider bar (“how much would you like to eat this at the end of the experiment?”, scale −10 to 10). The initial location of the slider was randomized to reduce anchoring effects. This rating screen had a free response time. The food was kept in the room with the subjects during the experimental session to assure them that all the items were available. Furthermore, subjects briefly saw all the items at this point so that they could effectively use the rating scale.

In the choice phase, subjects made their choices by pressing the left or right arrow keys on the keyboard. The choice screen had a free response time. Food items that received a negative rating in the rating phase of the experiment were excluded from the choice phase. We did not tell subjects about this feature of the experiment because doing so could have changed their incentives during the rating phase.

The items shown in each trial were chosen pseudo-randomly according to the following rules: (i) no item was used in more than 6 trials; (ii) the difference in liking ratings between the two items was constrained to be 5 or less; (iii) if at some point in the experiment (i) and (ii) could no longer both be satisfied, then the difference in allowable liking ratings was expanded to 7, but these trials occurred for only 5 subjects and so were discarded from the analyses. The spatial location of the items was randomized.

After subjects indicated their choice, a yellow box was drawn around the chosen item (with the other item still on-screen) and displayed for 1 s, followed by a fixation screen before the beginning of the next trial.