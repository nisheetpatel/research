---
{"dg-publish":true,"permalink":"/serotonin-model-meeting-with-zach/","created":"","updated":""}
---


Felix, Laurena, Nisheet, Zach

## Meeting summary and notes

Zach thinks this that our formalization is very close to the ideas he had in his head. Of course, he doesn't think that it captures all the aspects of what serotonin does in the brain, but he does think that it's really cool and novel in its formalization. He was wondering if we'd be down to write a NeurIPS paper (deadline is May 17th), which would be submitted to the neuroscience track of course. Alternatively, we could write a review or a perspective article with this and try to submit it to a neuroscience journal, but we both thought that NeurIPS might be better - it's short, has a deadline, and probably hits the right audience. If it doesn't get accepted, we could resubmit it as a perspective article to a journal. We definitely do not have enough at the moment, so there is work to be done, but he does have unpublished data from mice that we can use for the paper!

He also said that he's happy to start writing up a sketch on Overleaf already which is good because he's a very good writer.

## NeurIPS paper

The goal of this paper would *not* be to say that we have a new theory of Serotonin. Rather, we propose a framework to formalize our intuitions about one of the possible roles of Serotonin without making any specific claims about the model/implementation. Here's a rough sketch for the paper based on the discussions with Zach.

#### Figures

1. Schematic for the system and our model
2. Moving MNIST experiments
	- Analysis of the latent space: do separate dimensions represent position/id?
3. Mouse experiment with [miniworld](https://github.com/Farama-Foundation/Miniworld/blob/master/images/maze_top_view.jpg) + real experimental data

Of course, we will rearrange and restructure all of it up depending on the flow

#### Simulations

Not quite sure what would be the best way to perform the experimental manipulations, but we might want to train the network on all the different manipulations while making the timing unpredictable. Here's the intuition behind the reason:
if we train the network with statistical irregularities, e.g. video switch at frame 10,  it might go away (or not like bounce). But if the frame at which the video switches is randomized during training, we might still observe a small SPE. This would be similar to how DA neurons signal RPE if the timing is uncertain.

###### Moving MNIST

- change the speed of the video: make it faster or slower
	- include this during training?
- Analyze the latent dimensions to see if the position and digit identity coded in separate units
	- plot the latent space in an interactive manner like Ha & Schmidhuber 2018

###### Navigation task

The big question here is whether we want to include self-motion in the observations, or even better, give the agent control over its actions. Perhaps it is too complicated and we should prioritize other things leaving this as a thread for the future. But if we have time and/or if we think that it is not so complicated, then we could give it a shot.

- Construct a corridor as in the mouse experiment with [miniworld](https://github.com/Farama-Foundation/Miniworld/blob/master/images/maze_top_view.jpg)
- Change the gain (speed of the video) as an experimental manipulation
- Change visual patterns on the wall as the agent goes through the corridor
	- ABABAB... $\rightarrow$ ABABAC...

#### Real experimental data

![Pasted image 20230209212640.png|200](/img/user/images/Pasted%20image%2020230209212640.png)

The data in the figure is recorded in mice with fiber photometry. With photometry, you kind of pick up an average signal of the area you're recording from. So to compare to the data, you would have to average across the latent dimensions to compare.

An example I liked was that there was a HUGE signal when you turn on the VR. We should check with Zach whether we can get our hands on this data to include it as a figure in the paper because it would very much go with our story and it is an easy manipulation - switch from videos of natural images to the maze environment.

Zach will think of other/more data that he has which we can try to simulate and get back to us.