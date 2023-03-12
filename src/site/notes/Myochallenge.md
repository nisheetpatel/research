---
{"dg-publish":true,"permalink":"/myochallenge/","created":"","updated":""}
---

# Myochallenge

## Docs and blog
[[Myochallenge github doc\|Myochallenge github doc]]
[[Myochallenge blog\|Myochallenge blog]]
[[MyoChallenge Paper\|MyoChallenge Paper]]
[[MyoChallenge Cosyne 2023\|MyoChallenge Cosyne 2023]]

## Phase 2
[[Myochallenge Phase 2 task details\|Myochallenge Phase 2 task details]]
[[Myochallenge hand-crafted expert model\|Myochallenge hand-crafted expert model]]
[[Myochallenge phase 2 curriculum\|Myochallenge phase 2 curriculum]]

### Cluster deployment
This OpenAI project ([blog](https://openai.com/blog/learning-dexterity/), [article](https://arxiv.org/pdf/1808.00177.pdf)) uses 8 x V100 GPUs with 6144 CPUs for an extremely similar die reorientation task with almost the same kind of physics noise as ours. Here are the hyperparameters they used. Funnily enough, their fully connected ReLU layer is *before* the LSTM layer, which makes sense to me and how I initially thought the network was wired. 

![Pasted image 20221030022456.png](/img/user/images/Pasted%20image%2020221030022456.png)

- Google Cloud Program [create a VM](https://console.cloud.google.com/compute/instancesAdd?project=disco-abacus-367013)
- Unige HPC cluster ([user docs](https://doc.eresearch.unige.ch/hpc/start))
	- [CPUs and GPUs available on yggdrasil and baobab](https://doc.eresearch.unige.ch/hpc/hpc_clusters#for_advanced_users)
	- [Can check which ones are free from here](https://monitor.hpc.unige.ch/dashboards)
	- Use on Yggdrasil
		- CPU 116-119 or 123-150 (they have 128 cores)
		- GPU 008 (it is a V100)
	- [How to use](https://doc.eresearch.unige.ch/hpc/hpc_clusters#for_advanced_users)

## Team name
[[Myochallenge team name\|Myochallenge team name]]

## Curriculum learning
- [[Myochallenge standardize curriculum\|Myochallenge standardize curriculum]]
- [[Finger movement sub-task for Baoding\|Finger movement sub-task for Baoding]]

## Other ideas
- [[Myochallenge implementation ideas\|Myochallenge implementation ideas]]
- [[Myochallenge imitation learning\|Myochallenge imitation learning]]
- [[Myochallenge priorities 2022.10.14\|Myochallenge priorities 2022.10.14]]
- [[Myochallenge classifier for MoE\|Myochallenge classifier for MoE]]

## Initial notes
- [[Myochallenge notes 2022.08.19\|Myochallenge notes 2022.08.19]]
- [[Myochallenge notes 2022.09.19\|Myochallenge notes 2022.09.19]]

## Inspiration
<iframe width="560" height="315" src="https://www.youtube.com/embed/iyMXsvBwHKQ" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
<iframe width="560" height="315" src="https://www.youtube.com/embed/DpKao7kWU40" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
