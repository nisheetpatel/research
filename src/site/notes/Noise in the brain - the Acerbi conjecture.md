---
{"dg-publish":true,"permalink":"/noise-in-the-brain-the-acerbi-conjecture/","created":"","updated":""}
---

# Noise in the brain: the Acerbi conjecture


> [!tip] Placeholder for title
> A conjecture of functionally equivalent roles of noise in the brain: from variational Bayesian inference to robustness and generalization


## Premise

> [!abstract] TL;DR
> - Noise typically seen as nuisance: sth that corrupts sensing, computing, acting
> - Structure in noise largely ignored despite there being rich structure
> - There are some speculations of how (structured) noise might be beneficial, but they are rare and rather specific

#### Useful details

##### Buildup 
- common approach in neuroscience is to average over trials & subjects to determine relationships b/n task-relevant variables & behavior. This stems from the perspective of noise being a nuisance variance
- many models of cognition based on producing response $R$ from a stimulus $S$ with a deterministic function $R = f(S) + \epsilon$, where $\epsilon$ is additive, independent Gaussian noise
- Choices between options also modelled like this, e.g. 2AFC typically done with noisy estimates (again additive Gaussian noise or Gumbel noise) from which evidence is accumulated
	- argument: Gaussian distribution naturally comes out of summing many arbitrary (i.e. potentially non-Gaussian) distributions

However, there is at least some evidence that points to noise being highly structured and not independent:
- heavy tailed (Sanborn et al. 2022)
- Weber noise
- 1/f noise (Gilden et al., Farell et al.) or long-range autocorrelations

Moreover, sensory and motor noise not all there is. 
- And even when present, it is not a limiting factor except when taken to the extreme (our retina is crap, but we don't need ultra high-res images to classify objects). 
In fact, Drugowitsch et al 2016 showed that two-thirds of the noise related to inference, i.e. cognitive noise.

##### Noise in cognition

Recently, people have suggested how noise in cognition might be beneficial or even a core aspect of computation:

- Findling et al. 2021:
	- computational noise that scales with value is useful in volatile envs
- Dasgupta, ..., Gershman 2017: where do hypotheses come from?
	- Humans inferences are sometimes close to the Bayesian ideal but other times systematically biased. They propose a stochastic hypothesis sampling model and show that when the number of possibilities is large, humans display consistent biases.
	- They make no explicit claims that humans use MCMC sampling 
- Sanborn et al. 2022: noise as a feature not a bug
	- Metropolis-coupled MCMC (MC$^3$) sampling directly related to noise
- Friston?
	- Ask Luigi for references/relations
- Ma, Beck at al. 2006 / Lengyel??

^^ This is the wave we intend to ride <--> transition to the goal of the paper



## Goal of this paper

In this article, we do not consider noise as a handicap, but in fact, try to show how structured noise may underlie some core aspects of computation. In order to do this, we first conjecture that the following statements are functionally equivalent:

*The Acerbi conjecture*
1. (Multiplicative) noise in the neural network units
2. Bayesian inference over (a subset of) network parameters
3. “Data augmentation”
4. Robustness of the network to (structured) input corruptions
5. Ability to generalize to different settings (“tackling covariate shift”)

Next, we demonstrate this at work with variational inference in a node-based Bayesian deep neural network as a proof-of-concept/toy model. Finally, we view current neuroscience literature in light of this observation to highlight the paradigm shift that would be needed to build models of behavior, representation, and computation in the brain that would be required to test these ideas formally. 

We would like to make it clear to the reader that the goal of this article is not to claim that "noise in the brain comes from a specific form of inference, there you go, brain (noise) solved". Such claims have been made for variational inference (Friston?) or MCMC (Dasgupta 2017, Griffiths 2012, Sanborn 2022). We simply want to showcase multiple, functionally-equivalent roles of "noise" in a learning system. Note that perhaps none of these statements taken individually might be novel, but that is not the purpose of this article. This, again, is in stark contrast to our work, which merely shows it for a toy model without making strong claims about whether or not that is happening in the brain.


## Proof-by-(toy)-example for the conjecture

Source: Luigi's slides

### 1 = 2 or Noisy units = Bayesian inference
![Pasted image 20221215133255.png](/img/user/images/Pasted%20image%2020221215133255.png)![Pasted image 20221215133306.png](/img/user/images/Pasted%20image%2020221215133306.png)![Pasted image 20221215133313.png](/img/user/images/Pasted%20image%2020221215133313.png)![Pasted image 20221215133321.png](/img/user/images/Pasted%20image%2020221215133321.png)

### 1 = 3 or Noisy units = data augmentation
![Pasted image 20221215134233.png](/img/user/images/Pasted%20image%2020221215134233.png)

### 1 = 4 or Noisy units = robustness to input corruptions
![Pasted image 20221215133407.png](/img/user/images/Pasted%20image%2020221215133407.png)

### 1 = 5 or Noisy units = generalzation
![Pasted image 20221215133436.png](/img/user/images/Pasted%20image%2020221215133436.png)

QED.

*The Acerbi conjecture*
1. (Multiplicative) noise in the neural network units
2. Bayesian inference over (a subset of) network parameters
3. “Data augmentation”
4. Robustness of the network to (structured) input corruptions
5. Ability to generalize to different settings (“tackling covariate shift”)


## Takeaways and tests for computation in the brain

??

---

## Appendix


> [!todo] To do
> - Relations to Noise correlation stuff
> - Any relations with Ted Moskovitz + Matt Botvinick's paper?

#### Node-based BNNs 
Source: [Trinh et al. 2022](https://arxiv.org/abs/2206.02435), Luigi's slides

- Standard NNs highly fragile and brittle to noise
- Bayesian NNs (BNNs) worse than MAP models under corruptions
	- unclear why, but they think standard Gaussian prior not good inductive bias
- Proposal: Node-based BNNs
	- better than MAP under corruptions (more robust to noise)
	- way more efficient than weight-based BNNs
	- latent variable nodes multiplicative noise
- Training node-BNNs
	- find MAP for $\theta=\{(W^{(l)}, b^{(l)})\}_{l=1}^L$ with prior $p(\theta)$
	- infer posterior for $\mathcal{Z}=\{z^{(l)}\}_{l=1}^L$ with prior $p(\mathcal{Z})$
		- posterior approximated with variational inference (param $\phi$)
	- Training objective: ELBO maximized with SGD

Hypothesis for why node-BNNs generalize well?
- distribution of latent variables induces a distribution of implicit input corruptions
- increasing entropy of latent variables increase the diversity of implicit corruptions
	- they provide a modified ELBO to train higher-entropy posteriors
	- show that these are more robust to input corruptions

#### Meeting notes

##### 15 Dec 2022
- Moreno-Bote, Pouget, Knill
	- sampling hypotheses in a multi-stable perception task
	- links to PPC and/or noise in the brain?
- One and done? Vul, Goodman, Griffiths, Tenenbaum
	- connecting sampling-based or probability matching behavior with the hypothesis that human cognition can be understood in Bayesian terms
- Note: our conjecture is very high-level about abstract stuff
	- link between neural noise and noise in our toy model may be left as future work or might need quite some thinking
- Berkes, Orban, Lengyel, Fiser
	- spontaneous changes in neural activity reflect sampling of prior expectations
	- this is not abstract, latent noise
	- this style of work is very different from that of Sanborn, Griffiths, Tenenbaum, etc.

Two claims have been made to varying degrees:
- Noise as Bayesian inference
- Noise as Robustness
But these are largely separate threads. Our goal is to show that it could be doing both at the same time in the same system + more (other statements in the conjecture).

Thought experiment:
- If we tinker with activity in the IT cortex, does it create structured patterns in V1/V2?

Connect low level theories of neural variability (PPCs, Lengyel's sampling) to high-level roles of structured noise. Could we link these formally? Could we design experiments to test this?