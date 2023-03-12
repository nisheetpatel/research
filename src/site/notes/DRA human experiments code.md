---
{"dg-publish":true,"permalink":"/dra-human-experiments-code/","created":"","updated":""}
---

[[DRA human experiments\|DRA human experiments]]
Tags: #science #DRA #research #project  #code 

# DRA human experiments code
## Code responsibilities
### Backbone
- All agents
- Task
- Simulator and Trainer

### Experiment design
- Task parameters dict
- Experiment and Experiment group

#### $\Delta_{PMT}$
- with p-value tests
- with stan

##### Adaptive design
- adaptive testing with one dedicated session for setting $\Delta_{PMT}$ for each subject

### Data analysis
- stan code to fit posterior
- tests for posteriors
- probability of model signature
- fake data generator
- data extractors

#### Additional stuff
- Bayesian model comparison

## Architecture and design
- [[DRA human experiments code v0\|DRA human experiments code v0]]
- [[DRA human experiments code v0.1\|DRA human experiments code v0.1]]

### Ideas
#### Adapt models as in DRA vs max entropy RL
- Current model has way too much responsibility
- Also all models have the same class. Super weird
- Make models minimal and introduce Simulator and Trainer classes

#### Generate fake data
- Could do with simulator and something else

#### Analyze data
- build data reader
- analyzer for regular and PMT trials
- Bayesian model comparison