---
{"dg-publish":true,"permalink":"/dopamine-addiction-rpe-and-time/","created":"","updated":""}
---


# Laurena's project

This is Laurena's project on the role of Dopamine in an addiction task where the timing of the reward delivery is uncertain.

## Preliminary thoughts

Ethan mentioned that DA neurons in monkeys show negative ramping. This was the case when there was uncertainty in the time of the cue (the ITI was randomly picked between 2.2 & 3s). That's actually what standard TD theory should predict - a _darn it_ signal at each timestep when there is some probability with which the reward could have occurred. 

I'm really not sure because I can't imagine the dynamics of it unfolding, but maybe this intuition carries over to the case where the time of the reward delivery (or an observation of a cue) is fixed, but the agent's estimate of the state (time) itself is uncertain. I would have to simulate this to fully understand it, but if it does, then the idea would be that some probability mass/density of the belief state, i.e. the belief about what time it is right now, starts crossing the true reward-delivery time, which should, to some degree, elicit disappointment - a _darn it_ signal. And the degree to which it should elicit this should be proportional to the probability mass that has crossed over. One thing that we could take into account here is that we know the belief state empirically: that estimates of time have their uncertainty increase linearly with time according to weber law, i.e. $\sigma \propto t$ (check). What effect would that have on how much probability mass crosses over as a function of time? It would be interesting to simulate that as well. 

That being said, I think there's a big hole in my understanding here! In this case, the definitions of state and time are completely mangled with each other. This may be problematic in the RL setting, especially for TD theory, at least in the sense of carrying forward intuition. Given this POMDP setting, the true state transition $t \rightarrow t+1$, is reflected in the agent's brain with $b(s_t) \rightarrow b(s_{t+1})$, where $b$ is the belief (uncertain representation of time). Ask Pablo - he had done some math for RL with belief states - but I think there were some issues in how values of belief states were computed with TD learning, especially when actions are involved. 

In a POMDP setting, partially observed states with _similar_ features tend to have similar values, generally speaking. However, at the time of observation of a cue or reward, all the relevant uncertainty in the agent's belief collapses, causing a strong non-linearity in the transition $b(s_t) \rightarrow b(s_{t+1})$, because $b(s_{t+1})$ would be a $\delta$ function at $t+1$, whereas $b(s_t)$ and the preceding timesteps would have the growing Gaussian function. 

### Some older ramblings
##### Noisy observations w standard TD
On any given trial, construct the state as:
$s_0 = 0$, $s_1 = \delta t + \epsilon$, and $s_t = s_{t-1} + \epsilon$, where $\epsilon \sim N(0,\delta t)$. 
Unfortunately, this looks a lot if not exactly like evidence accumulation and drift-diffusion models. I'm so tired of it but Alex loves that shit. Unless this actually works, do not even mention it to him. It's his guilty pleasure; he won't let it go. 

##### Neural Net
Perhaps, a good way to capture the intuition of similarity in features of belief states might be with a neural network to which we manually feed in the features with the justification that these are the true (empirically driven) features that the animals operate with. Then, we could make it learn the value of these partially observed states with manually fed features in order to approximate their values. So states that have their means higher than the reward delivery time might still have a non-zero learnt value merely because they are similar to the state that had its mean ever so slightly smaller than the time of reward delivery. That fucks with TD and our standard intuitions of it. In fully observed case, the value of the rewarding state would be equal to $r$ (assuming that's the end), the one before that $\gamma r$, then $\gamma^2 r$, etc. But the states after would be zero-valued. Not sure what would happen if the values of the following states are artificially bumped up, because then you have leftover RPEs. But TD should make something converge, right? What am I missing? 

---

## In-person discussions
Friday, 23rd September 2022.
Laurena, Pablo, & Nisheet

#### Kaelbling et al. 1998
- computing belief states: section 3.3
- value functions for POMDPs: Section 4
- note how it doesn't work like normal TD. The value function is a max over multiple policy-trees, which depend on the belief state. So the intuitions from standard TD don't really carry forward.

#### Starkweather et al. 2017
- Nao Uchida's data, Sam Gershman's theory
- They claim that their results favor an associate learning rule that combines cached values with hidden-state inference.
- They test three models: standard TD, TD with reset, and some weird belief state model. We didn't go through the details, but their belief state model seems to capture the data, others don't.

#### Message from Peter Dayan (thanks to Pablo)
Dear Alex,

Thanks for your message.

> You don't buy the result of their experiment when they teleport the animal? They claim that the ramping activity is related to RPE, not value. However, they have to argue that their animal are still in the learning phase. Otherwise, the story makes no sense from a TD point of view.

Yes - I haven't tried to get the data from Nao. But there are a few things going on:  
  
1) the basic effect of teleportation is just what you expect from an RPE - you have a surprising change in the proximity of reward - and so that's fine, given discounting.
2) ramping is a problem - since you seem to have reliable release of DA when there is no obvious prediction error. There are a few potential sources of PE - Sam Gershman suggested the (not-very-good) idea that there is a problem with the function approximator; there could be a form of multiplicative forgetting; or there could be the mechanisms in the paper I sent.  
3) another possibility is to distinguish value-based VTA/accumbens DA a bit more from action-based SNc/dorsomedial/lateral striatum DA. A lot of work has been done in classical conditioning, for which the latter is less important. For instrumental conditioning - additional considerations come into play about causality. One idea I'm thinking about here has to do with counterfactuals - for instance between acting and not acting -- there is evidence from human studies that such counterfactuals are present in DA, and they are just right for this purpose.
  
Best wishes,
Peter

###### Notes
- Peter says that Sam's model is not good LOL
- On a slightly more serious note, he says that operant conditioning, which associates a voluntary behavior with a consequence, is different from classical conditioning, which associates an involuntary response and a stimulus.
- He says it might be worth considering totally different approaches in these two cases given the involvement of actions which necessitate considering causality. 
	- We discussed this partly with Laurena and Pablo during our discussions.
	- The example we talked about was the experiment Laurena is trying to model, where the mouse is free to go to the lever at any point and press it. This makes the timing of the lever press predictable to some degree! If it was fully predicted away, you should never see an RPE at the time of the press, but instead, only at the time when the mouse decided in its head to go press the lever. This is the causality issue that Peter is talking about.

---

## Belief state models
24 September 2022.

After reading the papers in the following subsection, I think we're in a very good place to formalize our ideas of a belief state that takes into account the temporal uncertainty. For now, I'm simply gonna note my takeaways from the papers.

### Existing formulations and data

1.  [Starkweather, Babayan, Uchida, and Gershman. 2017.](https://www.nature.com/articles/nn.4520) 
	- The layout of the framework looks similar to what we discussed on Friday, but it's a classic Sam paper: he's _Gershing it_ :D
	- I think the part that they're missing is a belief state that properly takes into account the uncertainty of time. Their "belief-state model" actually comes from the paper by  [Daw, Courville, and Touretzky 2006](https://ieeexplore.ieee.org/abstract/document/6796449), which I briefly describe in the next point.
	- The annoying part about this paper is that Sam claims that their formulation of the belief state model is mathematically equivalent to a model that has uncertainty in the dwell-time distributions of each, which (to me) is a highly misleading statement. It makes it seem like it considers the uncertainty in time as part of the belief state, which it might but only indirectly if at all.
2.  [Daw, Courville, and Touretzky. 2006.](https://ieeexplore.ieee.org/abstract/document/6796449) 
	- They operate in the framework where there are one of two possible true underlying states, ITI and ISI, which are:
		- ITI = Inter-Trial Interval: you didn't get a reward and hence are waiting for the next trial to start, and  
		- ISI = Inter-Stimulus Interval: you're yet to get a reward and hence must be in the period after the stimulus where the trial hasn't ended yet.  
	- So the belief is uncertain to the extent that the agent doesn't know whether it is in the ITI state or the ISI state. 
	- From the outset, I'll say that this seems quite wonky to me. Of course, this is just based on intuition, which is often wrong. But I do have a more concrete criticism, which I note in the last point.
	- Their approach is certainly an interesting way to construct the belief state, one that I hadn't considered at all. And I think it is also worth simulating as it is a very strong alternative model.  
	- My one concrete criticism of their framework is that temporal discounting, based on real-time, would definitely break if the states are arbitrarily discretized like this. For example, there could be trials where the ISI is 10s long, and others where it is 1s long, and that's going to elicit different magnitudes of RPE at the time of cue and reward. Same for ITI. However, if there are only two states, ISI and ITI, the transitioning between them skips over all the true temporal discounts.
	- They note this in their paper in section 4 in the paragraph after Equation 4.1: they use an average reward formulation instead of the temporal discounted value function. Mathematically, this means that there is no temporal discounting, i.e. $\gamma\rightarrow 1$.
	- One super interesting part of their paper is section 6.1, the first paragraph. The alternative model they mention there (just in words) is exactly the one that we discussed and want to construct. And that is based on the [Kaelbling et al. 1998](https://www.sciencedirect.com/science/article/pii/S000437029800023X) paper that Pablo sent us.

3. [Fiorillo, Newsome, and Schultz. 2008.](https://www.nature.com/articles/nn.2159)
	- They seem to have some very clean data that we could use to validate all belief state models.
	- What's interesting about their experiments is that the reward magnitude is always the same (across three experiments), but what changes is the ITI or how the cue predicts it and such.
	- They don't have a theory-driven model, but they have a phenomenological one to showcase the weber law in expected reward. I think this 100% confounded with the fact that the uncertainty of the belief state itself grows like this and hence the expected reward. This should pop out _exactly_ from the belief state model that we want to construct. Honestly, I'm very surprised that nobody has done this yet. Seems like low-hanging fruit.

### Formalizing our belief state model

I have noted some preliminary ideas below. But once we have a framework, I think we should simulate our model along with the ones in the previous section to look at the differences in predictions.

#### Belief state

- Rough ideas to construct our belief state representation:
	- Use the silly feature representation model from Sam's paper, where each feature is a time-bin.
	- In the fully observed case, say when you have an external clock, the feature of the state corresponding to the true time bin is 1 while the others are 0.
	- Define a belief state as a (discrete) probability distribution over the time-binned features. For example, I might have a 0.5 on the feature of one time bin and 0.25 each on the features representing the time bins immediately before and after.
	- The agent's belief states have an empirically known probability distribution, Gaussian with mean at true time t and standard deviation equal to t as well (Weber law, check). Just discretize this, that's all.
	- By doing this, the transition dynamics for belief states are Markovian and known in absence of observations (including reward, which we can consider as an observation). The only thing that changes with observations is the standard deviation of the belief state, which goes to zero, since the uncertainty is fully resolved.

#### Learning
Read Kaelbling et al. 1998 to figure out how learning should work. 