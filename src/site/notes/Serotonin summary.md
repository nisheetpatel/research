---
{"dg-publish":true,"permalink":"/serotonin-summary/","created":"","updated":""}
---


## Abstract v0

Neuroscience has established a concrete role for dopamine in the brain as a reward prediction error signal. Serotonin, however, has remained enigmatic, implicated in various functions but lacking a clear, unified explanation. We propose that serotoninergic neurons in the dorsal raphe nucleus (DRN) encode a low-dimensional latent state prediction error (SPE) signal, which reflects the surprise induced by unpredicted changes in the environment. This hypothesis suggests a more general role for serotonin in unsupervised learning processes. To investigate our conjecture, we develop a family of models called Dynamical Variational Autoencoders (DVAEs) that combine the strengths of dynamical models and Variational Autoencoders (VAEs) to capture the dynamics of sequential data and latent factors of data variations. We demonstrate the key ideas using DVAEs on a toy task with moving MNIST digits and a more realistic navigation task with a speed glitch. Our hypothesis is further supported by unpublished neural recordings from mice performing the navigation task, showing consistent responses in DRN serotoninergic neurons during speed glitches, regardless of the direction of change. This work provides a formal framework for understanding the role of serotonin in unsupervised learning and offers insights into the diverse functions of neuromodulatory systems in the brain.

#### Points to emphasize

Serotonin response captures:

1. a low-D (but not 1D) latent state prediction error signal
2. only to *attended* or *relevant* features of state
3. that is modulated by learning


## 1. Introduction

Neuromodulatory systems, such as dopamine and serotonin, are pivotal to the brain's learning processes. These biologically ancient systems act as privileged channels of communication, diffusing throughout the brain unlike point-to-point synaptic connections, and modulate neuronal functions through specialized receptors.  While there have been substantial advances in the dopaminergic system, which has been shown to encode a prediction error signal for reinforcement learning (RL), the role of other systems, particularly serotonin, remains enigmatic.

Early models proposed by researchers like Dayan and Doya posited that these systems broadcast scalar signals, mainly due to their release by small clusters of neurons in brainstem regions like the ventral tegmental area (VTA) for dopamine and the dorsal raphe nucleus (DRN) for serotonin. However, emerging evidence has begun to challenge this scalar signal assumption, highlighting the signal's heterogeneity (Uchida/Witten/Zeb-Kurth Nelson for DA, Ranade et al. 2009 for 5-HT). This leads to the question of what is captured by the heterogeneity in these signals.

In this study, we propose a novel conjecture: serotonergic neurons in the DRN encode a low-dimensional latent state prediction error (SPE) signal, serving as a surprise signal in response to unexpected changes in the environment. This conjecture suggests that neuromodulatory systems, including serotonin, can be mapped to vectors in a low-dimensional latent space, contributing a new perspective to the field.

To investigate this conjecture, we present a learning framework that gives rise to a low-dimensional latent SPE. We utilize a family of models called Dynamical Variational AutoEncoders (DVAEs), and simulate them on tasks such as moving MNIST digits to demonstrate the implications of our conjecture. We further validate our models with real-world data from mice performing a virtual reality navigation task.

Our key contributions include a formal conjecture for a plausible role of serotonin, a learning framework generating a low-dimensional latent SPE, and demonstrations of the key ideas using DVAEs on toy and realistic tasks. We also provide empirical evidence supporting our hypothesis through neural recordings in mice. Our study thus contributes to the rich history of neuromodulatory system research, offering new insights into the complex machinery of the brain and the principles governing learning and decision-making.


### 1.1. Related work

Needs to be refined in the following format:

1. Our work is related to mapping neuromodulatory systems to key signals used by learning systems:
	- RPE for RL (Schultz et al. 1997)
	- Diversity of responses in VTA DA neurons:
		- Uchida, Witten, Zeb-Kurth Nelson
	- It is different in that we focus on 5-HT neurons in DRN encoding the learning signal for unsupervised learning instead of VTA DA neurons reflecting the learning signal for RL
2. Our work is also related to studies investigating what DRN 5-HT neurons encode:
	- See all the references below
	- However, we have a framework whereas they did not.
3. Our work is also related to predictive coding and other ideas which are prominent models of how the brain, especially the cortex, performs general information processing. However, they don't map it to neuromodulatory systems, typically keep it to PFC. Our work maps it directly to a system in the brain.
4. Our work is related to models proposed by Schmidhuber, DVAE folks, VRNN, etc.
	- We adopt their models to show the implications of our conjecture, and eventually

Serotonin (5-HT) has been implicated in many functions and this has led to it being considered somewhat mysterious in function in contrast to dopamine, which is associated with reward. Classical studies with, mainly by Jacobs and colleagues found DRN 5-HT neurons responded to a variety of events, often those with some internal (e.g. chewing) or external salience (e.g. opening of a door, tail pinch), as well as being modulated by activity state (sleep vs. wake). Early studies in tasks (Ranade et al.) showed a surprisingly heterogeneous population of responses in DRN, with neurons responding to sensory stimuli, motor actions and rewards and absence of reward. Moreover, the identified 5-HT neurons also showed heterogeneous responses (Cohen et al.), including both rewards and punishments).

Matias et al. studied DRN 5-HT neurons using fiber photometry, which records a bulk signal, in a reversal learning task in which odor cues were associated with different good or bad outcomes and then those associations were abruptly changed. They found prominent responses to reversals which were independent of the sign of reversal and tended to adapt or habituate (except to aversive air puffs). They suggested a 5-HT to signal unsigned error or surprise signal. This could still be reward related, but hints at a more general prediction error. However, their experiment was done with a reward, which makes it hard to make claims about the surprise signal being related to non-reward related state features.

Our work is also related to predictive coding and other ideas which are prominent models of how the brain, especially the cortex, performs general information processing. We formalise this idea in the context of network models which perform unsupervised learning by learning to predict their inputs. Our work is thus similar in that it is an important non-RL class of models. Our models are derived from the ones proposed by Ha & Schmidhuber 2018, Cheng et al. 2016, and (cite DVAE paper).

### References

(Change to internal markdown citation later)

- [Ranade & Mainen 2009](https://journals.physiology.org/doi/full/10.1152/jn.00507.2009)
	- 2AFC odor discrimination task in rats
	- Tetrode recordings from DRN
	- *DRN neurons respond to diverse sensory, motor, and reward-related events*
		- odor port entry & exit
		- odor valve on
		- water port entry & exit
		- solenoid valve click
		- auditory tone
	- spanned the entire range of observed behavioral correlates, including odor selectivity, valve-click response, odor-port–specific inhibition, reward timing, and reward omission
	- Not a 1D signal, but low-D
- [Matias et al 2017](https://elifesciences.org/articles/20552)
	- Reversal learning task in mice
	- Fiber photometry recordings from DRN
	- *DRN 5-HT neurons are activated by both positive and negative reward prediction errors*
	- *responses to unexpected uncertainty*, i.e. out-of-context presentation of the US
		- not so clear whether they respond to expected uncertainty as well
- [Li et al 2016](https://www.nature.com/articles/ncomms10503)
	- classic conditioning task in mice to link an olfactory cue with the expected delivery of liquid reward
	- photometry and single unit recordings of DRN
	- *Both expected and unexpected rewards activate 5-HT neurons*
		- unexpected rewards activate 5-HT neurons *at* the time of delivery
		- expected rewards activate 5-HT neurons *before* the time of delivery, during waiting
			- only in trained mice
- [Cazettes et al 2021](https://www.sciencedirect.com/science/article/pii/S0960982220315037)
	- bunch of stuff with optogenetic activation of DRN 5-HT neurons, mostly not relevant (since we don't speculate about how the signal affects control)
	- but results are *consistent with the idea that 5-HT might report a surprise signal*
- [Nakamura 2013](https://www.frontiersin.org/articles/10.3389/fnint.2013.00060/full)
	- interactions of 5-HT and other systems (esp DA)
		- how 5-HT signal is utilized at projection targets
	- results again consistent with a surprise signal
		- *provide a “reward context” signal to the targets of DRN projections, where the signal may be used differently depending on the type of 5-HT receptor present*
- [Luo et al](https://learnmem.cshlp.org/content/22/9/452.full)
	- Optogenetic manipulations
	- DRN neurons strongly drives reward behaviors & activating DRN 5-HT neurons promotes reward waiting
	- knocking out _SERT_ reduces mouse reward-seeking behaviors ([Sanders et al. 2007](https://learnmem.cshlp.org/content/22/9/452.full#ref-124)) but improves reversal learning ([Brigman et al. 2010](https://learnmem.cshlp.org/content/22/9/452.full#ref-13))
- [Boureau & Dayan 2011](https://www.nature.com/articles/npp2010151)
	- in-depth review of 5-HT & DA "opponency"
- [Fischer & Ullsperger 2017](https://www.frontiersin.org/articles/10.3389/fnhum.2017.00484/full)
	- serotonergic system mainly signals unsigned PE
- [Paquelet et al 2022](https://www.sciencedirect.com/science/article/pii/S0896627322004561)
	- Individual DRN5-HT neurons responded to diverse combinations of salient stimuli, with some preference for valence and sensory modality
- [Cohen et al 2015](https://elifesciences.org/articles/6346)
	- Serotonergic neurons signal reward and punishment (unsigned) on multiple timescales

## Experiments

#### Moving MNIST

[[Roadmap for moving MNIST task\|Roadmap for moving MNIST task]]

![image (3).png](/img/user/images/image%20(3).png)

#### Hallway navigation task

![image.png](/img/user/images/image.png)

##### Mouse data

![Pasted image 20230209212640.png|200](/img/user/images/Pasted%20image%2020230209212640.png)

![Pasted image 20230421133124.png](/img/user/images/Pasted%20image%2020230421133124.png)

![Pasted image 20230421133150.png](/img/user/images/Pasted%20image%2020230421133150.png)

##### Simulations

![image (2).png](/img/user/images/image%20(2).png)![image (1).png](/img/user/images/image%20(1).png)



## Random notes

#### Meeting w Zach

One of the first pieces of data on serotonin:
- serotonin neurons inhibited during REM sleep
- more active during aroused/active/wake states

^^ cite this

- Josh may have data with one more mouse
	- both glitch & gain change
- Romain will help understand the structure of the data as well as give us the script to reproduce the figures

#### Meeting w Alex

- Emphasize that previously, people only looked at it in a reward context, never in a no-reward context. That is exactly what we do.
- Reversal learning results
	- could show how our model may be in line with it if we consider downstream effects of model-free vs model-based learning for reward
	- silencing DRN would disallow model-based learning, hence quick adaptation would no longer be possible
- Diversity of responses
	- current experimental data does not show this
	- MNIST does
	- previous results (Ranade & Mainen) do explicitly show this
	- we could simulate a task where there is such diversity and show that our model also captures it

##### Zach's take on these points

- We don't want to make wild claims that we know are not going to be true. 
- We should try to stick to the most general parts of theory, which would have some predictive power, but explicitly state what we do not make claims about:
	- the specific model
	- which features should be encoded
		- we can say that it is low-D not 1D
		- we can also say that it is very likely to be multi-modal
		- but cannot make claims about which features are important and which ones aren't
	- the functional role of the latent SPE
	- the exact functional form of the latent SPE
		- could be the norm
		- could be a likelihood for a probabilistic SPE