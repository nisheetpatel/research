---
{"dg-publish":true,"permalink":"/trajectory-vs-time-step/","created":"","updated":""}
---

[[DRA vs Max Ent RL\|DRA vs Max Ent RL]]
#project #research #RL #max-entropy #DRA 

# Trajectory vs time-step

In their original formulation, Ziebart et al. 2008 proposed the maximum entropy principle as a normative solution to the problem of resolving degeneracy in the [[Inverse RL\|Inverse RL]] problem, where the goal is to find a reward function to rationalize observed behavior. Here, the entropy in question is over the trajectories observed.

The max entropy principle applied to the RL problem, on the other hand, tries to learn a sufficiently varied distribution of policies and not just the optimal policy, which is often impossible to find in a POMDP setting. Here, the entropy in question is over the immediate action space for a given state, which seems behaviorally equivalent to DRA.

Crucially, it seems that as long as the agent has access to the q-values or advantage-values for each state action pair, there shouldn't be a difference whether the max entropy principle applied to the RL problem is local or over trajectories. Verify it on this problem:

A 
|--> B --> C
|--> Y --> Z
|--> X --> Y --> Z

(Update: this task doesn't make so much sense to me. Maybe I was trying to use the Ziebart task?)

If the rewards on each of the paths are equal, a local max-entropy agent would choose the path A-X-Y-Z half the time, and the other two only a quarter of the times. You can imagine scaling up the other side the probability of choosing each path there goes down much lower. However, if the action-values for the two paths are different, depending on the trade off with reward, the agent will learn to choose the path with max reward with higher probability.