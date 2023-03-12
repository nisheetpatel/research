---
{"dg-publish":true,"permalink":"/myochallenge-implementation-ideas/","created":"","updated":""}
---

Ideas to try other than Pablo's main simple curriculum with SB3's RecurrentPPO.


> [!warning] Critical
> It is important to tune `vf_loss_coeff` to ensure that `vf_loss` and `policy_loss` are roughly of the same magnitude.


> [!NOTE] Hugely helpful for implementation
> [PPO implementation details](https://iclr-blog-track.github.io/2022/03/25/ppo-implementation-details/)
> 1. Use sde for continuous actions
> 2. 


## Current SB3 setup
1. Optimize hyperparameters ([everything including architecture search](https://github.com/araffin/rl-baselines-zoo/issues/29))
2. Framestack instead of using LSTMs
3. Add feedforward units to both value and policy networks
4. Disable critic LSTM

## RLlib

Refer to [this github issue](https://github.com/ray-project/ray/issues/20827) for examples of proper implementations of LSTM, attention, and framestack.

1. Tune hyperparameters
2. Try framestack instead of LSTM
3. Use LSTM
4. Use autoregressive action distributions
5. Use attention
6. Try IMPALA or APPO


# Order of execution

## Hyperparameter tuning

### SB3
- Clone rl-baselines3-zoo
- Register our custom env in `rl_zoo/import_envs`
- Use "ppo_lstm" as a the algorithm
- Choose hyperparameters as in 

### RLlib

## 
