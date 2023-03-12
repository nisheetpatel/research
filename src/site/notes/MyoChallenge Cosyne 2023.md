---
{"dg-publish":true,"permalink":"/myo-challenge-cosyne-2023/","created":"","updated":""}
---


# Cosyne Abstract

## Summary

There are two key challenges in building realistic models for complex and skillful motor tasks such as dexterous manipulation by muscles: the need for physiologically-detailed musculoskeletal models and powerful yet interpretable algorithms that they are able to tackle such high-dimensional problems. Fortunately, we now have such musculoskeletal environments and tasks in MyoSuite, which is 4000x faster than other simulators such as OpenSim.

With the confluence of ideas from the normative approach of stochastic optimal control and the general-purpose framework provided by reinforcement learning (RL), we are finally able to tackle such a problem. In this work, we introduce a procedure called Static to Dynamic Stability (SDS) which builds upon the principle of equilibrium-point control. As the name suggests, SDS learns stable solutions at several desired points in the trajectory and gradually relaxes these static stability constraints to yield dynamically stable movement motifs. We deploy it with a recurrent architecture of a popular RL algorithm, Proximal Policy Optimization (PPO), on the Baoding Balls task which involves rotating two balls in the hand. The task is extremely challenging since the environment is very noisy and partially observable, and furthermore, it involves a decision-making component to it. Our model beat all baselines by a landslide and won the MyoChallenge, a NeurIPS 2022 competition.

We believe that building such models will bring about new advents in studying motor control not only by allowing us to explore the core principles of motor learning and their scalability but also by exploring the emergent dynamical movement motifs. In future work, we plan to investigate what aspects of the world are encoded in memory throughout learning, for instance, by analyzing how the recurrent dynamics change over the course of curriculum learning and how they adapt to noise.

## Additional detail

