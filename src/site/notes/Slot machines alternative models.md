---
{"dg-publish":true,"permalink":"/slot-machines-alternative-models/","created":"","updated":""}
---


#DRA #human-memory #slot-machines 


# Alternative models

We already have the following families of models to compare DRA against:

- Frequency-based resource allocation
- Stakes-based resource allocaiton
- Equal precision

However, these are all just heuristics for resource allocation that are intuitive and human-interpretable. This doesn't necessarily make them good models.

We should try and construct really solid alternative models using state-of-the-art methods to hammer the point that the effect we predicted and are showing is *really* something entirely new, which we would have never gotten to if we did not follow the normative approach. Here are the models we should construct:

#### Standard RL model

We should try to show that a standard RL model doesn't fit this data


#### Bayesian ideal observer model

The ideal observer would observe the values of each of the slot machines and update its beliefs about the value of the slot machine. However, it has no memory resource allocation built into it, meaning that if it sees something less frequently or if the stakes are not as high (on average), then the only thing constraining the precision with which it encodes the values of the slot machines is lack of data. In the limit of infinite data, these effects should go away, and we would expect this model to look like an equal precision model. With finite data, it may look like a frequency-based model because of the seriously low number of trials for the infrequent slot machines collected per subject.

The standard ones have something like a Gaussian inverse-Gamma prior with a softmax choice.