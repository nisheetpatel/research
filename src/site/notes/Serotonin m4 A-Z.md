---
{"dg-publish":true,"permalink":"/serotonin-m4-a-z/","created":"","updated":""}
---

[[Serotonin immersion grant\|Serotonin immersion grant]]

# Computational story
1-2 pm on 19 October, 2021.

---

## Background
- Concerns:
	- Can't change the existing story much
	- Would have to re-write the proposal

---
 
## Immersion story
- Immersion
	- State w no disruptions/prediction errors
	- Tennis example
	- Resources freed up for *higher-level* processes, e.g. strategy
- Serotonin
	- Reflects prediction errors
	- Crucially, draws attention to them
		- Resources are dedicated to correct this error

___

 ## Schematic
 <span class='centerImg'> ![[Pasted image 20211019121659.png\|Pasted image 20211019121659.png]] </span>
 
 ___
 
 ## A story of priors and evidence
 - Classic version:
	 - Serotonin strengthens/weakens prior
 - Data:
	 - Sensory/motor: ~strengthening
		 - (hallucinations/gross motor skills)
	 - DM/cognitive: ~weakening
		 - (opening the doors of perception)

___
## Attention
- Proposal: 
	- Serotonin merely draws attention to errors
	- Passive process
		- disruptive nonetheless
- Implications:
	- perceptual process... 
	- decision process: evidence vs belief

---
### Attention drawn in absence of prediction errors
- Attention is drawn to processes passively by Serotonin
	- Naive users' behavior impacted more
	- Experienced users may exert higher level control
- Experiments:
	- Humans in VR
	- Vary novelty in butterflies task

---

## Modeling ideas

---
### Priors and evidence
<span class='centerImg'> ![[Pasted image 20211019124300.png\|Pasted image 20211019124300.png]] </span>
- Collect and tag task for mice
- Use Bayesian model w up-/down-weighting priors
	- Pouget, Latham 2019

---
### Priors and evidence
<span class='centerImg'> ![[Pasted image 20211019125052.png\|Pasted image 20211019125052.png]] </span>
- Collect and tag task for humans
- Simple model may not work here

___
### Hierarchical RL
- Basic sketch:
	- Pre-train models of perception, control on a set of tasks
		- Hierarchical states & policies
- At test time, fix weights and perturb attention/weighting of diff. areas

---
### HRL ideas
- Latent space policies
- Option-critic w option discovery

---
### Outline
- non-hierarchical prior/evidence story
	- may work on mice; maybe too naive
- hierarchical version
	- attentional perturbation + control
	- humans
- go back to mice and check this model
