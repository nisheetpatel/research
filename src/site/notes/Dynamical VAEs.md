---
{"dg-publish":true,"permalink":"/dynamical-va-es/","created":"","updated":""}
---


Considering as an approach to model our conjecture of [[The role of serotonin\|The role of serotonin]] in encoding State Prediction Errors (SPEs).

## Notes

#### Motivation for DVAEs

To mix the worlds of
1. Dynamical models aiming to model the dynamics of sequential data
2. VAEs aiming to models the latent factors of data variations

##### Alternatives
1. Temporal convolutional networks (Bai et al. 2018)
2. Transformers (Vaswani et al. 2017)
	1. Transformer VAE (Jiang et al 2020)

#### The class of Dynamical VAEs

Joint distribution of latent & observed sequences:

![Pasted image 20221212091928.png](/img/user/images/Pasted%20image%2020221212091928.png)

![Pasted image 20221212092520.png](/img/user/images/Pasted%20image%2020221212092520.png)

Two main sub-families:
1. State-space models
	- independent dynamical model + instantaneous observation model
	- DKF/DMM, KVAE, DSAE
2. Predictive or autoregressive models
	- intricate cross-dependencies b/n variables, particularly $x_{t-1}$ or $x_{1:t-1}$ is used for generating $x_{t}$ and/or $z_{t}$
	- STORN, SRNN, VRNN


## Resources

1. [Dynamical VAEs: a comprehensive review](https://arxiv.org/abs/2008.12595)
2. [YT tutorial on Dynamical VAEs](https://www.youtube.com/playlist?list=PLuZsCU0LDHDeEusitbdY6bNKOEhqh6DnS)