---
{"dg-publish":true,"permalink":"/simulations-dra-vs-max-entropy-rl/","created":"","updated":""}
---


[[DRA vs Max Ent RL\|DRA vs Max Ent RL]]

## Max-entropy RL refresher

Quick refresher on max-entropy RL since we use the intuitions for the some of the results below.

${J}_\text{MaxEnt}(\pi;p,r) = \mathbb{E}_{a_t\sim\pi(a_t|s_t),\ s_{t+1}\sim p(s_{t+1}|s_t,a_t)}\left[\sum_t r(s_t,a_t) + \alpha\mathcal{H}_\pi\left(a_t|s_t\right)\right]$

$q_\text{soft}(s,a)=r(s,a)+\gamma V_\text{soft}(s')$

$V_\text{soft}(s)=\alpha\log \mathbb{E}_\pi \left[\exp\left(\frac{q_\text{soft}(s,\cdot)}{\alpha}\right)\right]$

$\pi(a|s) = \frac{1}{Z}\exp\left(\frac{q_\text{soft}(s,a)-V_\text{soft}(s)}{\alpha}\right)$



> [!warning] Caution
> Ensure that the exploration problem of q-learning (positive bias) hasn't crept into the results. To do so, re-run the simulations with 



## Simulation results

- [x] Ziebart's task
- [x] Mattar & Daw's maze
- [/] Memory 2AFC task
- [/] Bottleneck task
- [ ] Slot machines task
- [ ] Huys task

#### Ziebart's task

[Ziebart's proposition for inverse RL](https://www.aaai.org/Papers/AAAI/2008/AAAI08-227.pdf) was to penalize the entropy of trajectories sampled from the policy, since that would be intractable for large MDPs. We want to see if this differs from the approach by Haarnoja et al., who penalize the entropy of the policy $\pi(a_t|s_t)$ at time $t$.

##### Task schematic

![[Pasted image 20230214105722.png\|Pasted image 20230214105722.png]]

We will refer to $s_0\rightarrow s_1 \rightarrow\ ... \rightarrow s_5$ as the top-branch, and $s_0\rightarrow s_2 \rightarrow s_4 \rightarrow s_5$ as the bottom branch. Notably, only the final state has a reward, i.e.

- $r(s_i)=0\quad \forall\ i\in\{1,...,4\}$
- $r(s_5)=1$

##### Results

Both models were simulated with $\gamma = 0.98$. For both models, we varied the noise parameters: $\lambda>0$, and $\sigma_\text{base}>0$ for DRA, and $\alpha>0$ for max-entropy RL. Note that if a model does not show any preference between two choices, it's entropy should be $\mathcal{H}(s)=\ln(2)\approx0.693$.

| Model  | $\mathcal{H}(s_0)$ | $\mathcal{H}(s_1)$ | Noise                                    |
| ------ | ------------------ | ------------------ | ---------------------------------------- |
| DRA    | 0.693              | 0.693              | $\forall\ (\lambda, \sigma_\text{base})$ |
| maxEnt | 0.64               | 0.69               | $\alpha=10$                              |
| maxEnt | 0.64               | 0.58               | $\alpha=1$                               |
| maxEnt | 0.5                | 0.24               | $\alpha=0.5$                             |
| maxEnt | 0                  | 0                  | $\alpha=0.1$                             |

As expected, DRA doesn't show a preference for either branch. This is because it encodes the true action-values which are equivalent for both branches. Max-entropy RL, on the other hand, shows a distinct preference for the top branch because it doesn't encode the true action-values, but instead "soft" action-values, which are different for the top and bottom branches. The intuition of why the soft action-values for the top branch is higher than that of the bottom branch is hard to get just from the equations I have listed at the top of this page, but if you work out the terms of the value, it is the normal RL value + a term that corresponds to the entropy of the agent's choices in that state.

We can also see the preference in this figure showing the difference in the learned action-values for each of the two choice states $(s_0, s_1)$ for both models:

<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<svg
   width="706.14pt"
   height="432pt"
   viewBox="0 0 706.14 432"
   version="1.1"
   id="svg935"
   sodipodi:docname="stakes.svg"
   inkscape:version="1.1.2 (0a00cf5339, 2022-02-04)"
   xmlns:inkscape="http://www.inkscape.org/namespaces/inkscape"
   xmlns:sodipodi="http://sodipodi.sourceforge.net/DTD/sodipodi-0.dtd"
   xmlns:xlink="http://www.w3.org/1999/xlink"
   xmlns="http://www.w3.org/2000/svg"
   xmlns:svg="http://www.w3.org/2000/svg"
   xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#"
   xmlns:cc="http://creativecommons.org/ns#"
   xmlns:dc="http://purl.org/dc/elements/1.1/">
  <sodipodi:namedview
     id="namedview937"
     pagecolor="#ffffff"
     bordercolor="#666666"
     borderopacity="1.0"
     inkscape:pageshadow="2"
     inkscape:pageopacity="0.0"
     inkscape:pagecheckerboard="0"
     inkscape:document-units="pt"
     showgrid="false"
     inkscape:zoom="1.4211063"
     inkscape:cx="540.07221"
     inkscape:cy="293.78521"
     inkscape:window-width="3366"
     inkscape:window-height="1376"
     inkscape:window-x="74"
     inkscape:window-y="27"
     inkscape:window-maximized="1"
     inkscape:current-layer="axes_2" />
  <metadata
     id="metadata2">
    <rdf:RDF>
      <cc:Work>
        <dc:type
           rdf:resource="http://purl.org/dc/dcmitype/StillImage" />
        <dc:date>2022-05-12T10:34:33.561815</dc:date>
        <dc:format>image/svg+xml</dc:format>
        <dc:creator>
          <cc:Agent>
            <dc:title>Matplotlib v3.5.1, https://matplotlib.org/</dc:title>
          </cc:Agent>
        </dc:creator>
      </cc:Work>
    </rdf:RDF>
  </metadata>
  <defs
     id="defs6">
    <style
       type="text/css"
       id="style4">*{stroke-linejoin: round; stroke-linecap: butt}</style>
  </defs>
  <g
     id="figure_1">
    <g
       id="patch_1">
      <path
         d="M 0 432  L 706.14 432  L 706.14 0  L 0 0  z "
         style="fill: #ffffff"
         id="path8" />
    </g>
    <g
       id="axes_1">
      <g
         id="patch_2">
        <path
           d="M 50.824644 390.04  L 338.737023 390.04  L 338.737023 24.72  L 50.824644 24.72  z "
           style="fill: #ffffff"
           id="path11" />
      </g>
      <g
         id="line2d_1">
        <path
           d="M 122.802739 333.235223  L 266.758928 333.522808  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #79c1b8; stroke-width: 2.7; stroke-linecap: square"
           id="path14" />
      </g>
      <g
         id="line2d_2">
        <path
           d="M 122.802739 333.289311  L 122.802739 333.194497  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #79c1b8; stroke-width: 2.7; stroke-linecap: square"
           id="path17" />
      </g>
      <g
         id="line2d_3">
        <path
           d="M 108.40712 333.289311  L 137.198358 333.289311  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #79c1b8; stroke-width: 2.7; stroke-linecap: square"
           id="path20" />
      </g>
      <g
         id="line2d_4">
        <path
           d="M 108.40712 333.194497  L 137.198358 333.194497  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #79c1b8; stroke-width: 2.7; stroke-linecap: square"
           id="path23" />
      </g>
      <g
         id="line2d_5">
        <path
           d="M 266.758928 333.691474  L 266.758928 333.353663  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #79c1b8; stroke-width: 2.7; stroke-linecap: square"
           id="path26" />
      </g>
      <g
         id="line2d_6">
        <path
           d="M 252.363309 333.691474  L 281.154547 333.691474  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #79c1b8; stroke-width: 2.7; stroke-linecap: square"
           id="path29" />
      </g>
      <g
         id="line2d_7">
        <path
           d="M 252.363309 333.353663  L 281.154547 333.353663  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #79c1b8; stroke-width: 2.7; stroke-linecap: square"
           id="path32" />
      </g>
      <g
         id="PathCollection_1">
        <defs
           id="defs36">
          <path
             id="m67a9cd3a9b"
             d="M 0 3.383948  C 0.897434 3.383948 1.758231 3.027394 2.392813 2.392813  C 3.027394 1.758231 3.383948 0.897434 3.383948 0  C 3.383948 -0.897434 3.027394 -1.758231 2.392813 -2.392813  C 1.758231 -3.027394 0.897434 -3.383948 0 -3.383948  C -0.897434 -3.383948 -1.758231 -3.027394 -2.392813 -2.392813  C -3.027394 -1.758231 -3.383948 -0.897434 -3.383948 0  C -3.383948 0.897434 -3.027394 1.758231 -2.392813 2.392813  C -1.758231 3.027394 -0.897434 3.383948 0 3.383948  z "
             style="stroke: #79c1b8; stroke-width: 2.025" />
        </defs>
        <g
           clip-path="url(#p611c9b1e85)"
           id="g42">
          <use
             xlink:href="#m67a9cd3a9b"
             x="122.802739"
             y="333.235223"
             style="fill: #79c1b8; stroke: #79c1b8; stroke-width: 2.025"
             id="use38" />
          <use
             xlink:href="#m67a9cd3a9b"
             x="266.758928"
             y="333.522808"
             style="fill: #79c1b8; stroke: #79c1b8; stroke-width: 2.025"
             id="use40" />
        </g>
      </g>
      <g
         id="xtick_1">
        <g
           id="line2d_8">
          <defs
             id="defs46">
            <path
               id="mfe2ed4cf68"
               d="M 0,0 V 3.5"
               style="stroke:#000000;stroke-width:0.8" />
          </defs>
          <g
             id="g50">
            <use
               xlink:href="#mfe2ed4cf68"
               x="122.80274"
               y="390.04001"
               style="stroke:#000000;stroke-width:0.8"
               id="use48"
               width="100%"
               height="100%" />
          </g>
        </g>
        <g
           id="text_1">
          <!-- 0 -->
          <g
             transform="matrix(0.1,0,0,-0.1,119.62149,404.63844)"
             id="g58">
            <defs
               id="defs54">
              <path
                 id="DejaVuSans-30"
                 d="m 2034,4250 q -487,0 -733,-480 -245,-479 -245,-1442 0,-959 245,-1439 246,-480 733,-480 491,0 736,480 246,480 246,1439 0,963 -246,1442 -245,480 -736,480 z m 0,500 q 785,0 1199,-621 414,-620 414,-1801 0,-1178 -414,-1799 -414,-620 -1199,-620 -784,0 -1198,620 -414,621 -414,1799 0,1181 414,1801 414,621 1198,621 z"
                 transform="scale(0.015625)" />
            </defs>
            <use
               xlink:href="#DejaVuSans-30"
               id="use56"
               x="0"
               y="0"
               width="100%"
               height="100%" />
          </g>
        </g>
      </g>
      <g
         id="xtick_2">
        <g
           id="line2d_9">
          <g
             id="g64">
            <use
               xlink:href="#mfe2ed4cf68"
               x="266.75894"
               y="390.04001"
               style="stroke:#000000;stroke-width:0.8"
               id="use62"
               width="100%"
               height="100%" />
          </g>
        </g>
      </g>
      <g
         id="ytick_1">
        <g
           id="line2d_10">
          <defs
             id="defs97">
            <path
               id="m9ff2572f5d"
               d="M 0,0 H -3.5"
               style="stroke:#000000;stroke-width:0.8" />
          </defs>
          <g
             id="g101">
            <use
               xlink:href="#m9ff2572f5d"
               x="50.824642"
               y="376.13672"
               style="stroke:#000000;stroke-width:0.8"
               id="use99"
               width="100%"
               height="100%" />
          </g>
        </g>
        <g
           id="text_4">
          <!-- −1 -->
          <g
             transform="matrix(0.1,0,0,-0.1,29.082457,379.93594)"
             id="g112">
            <defs
               id="defs106">
              <path
                 id="DejaVuSans-2212"
                 d="M 678,2272 H 4684 V 1741 H 678 Z"
                 transform="scale(0.015625)" />
              <path
                 id="DejaVuSans-31"
                 d="M 794,531 H 1825 V 4091 L 703,3866 v 575 l 1116,225 h 631 V 531 H 3481 V 0 H 794 Z"
                 transform="scale(0.015625)" />
            </defs>
            <use
               xlink:href="#DejaVuSans-2212"
               id="use108"
               x="0"
               y="0"
               width="100%"
               height="100%" />
            <use
               xlink:href="#DejaVuSans-31"
               x="83.789062"
               id="use110"
               y="0"
               width="100%"
               height="100%" />
          </g>
        </g>
      </g>
      <g
         id="ytick_2">
        <g
           id="line2d_11">
          <g
             id="g118">
            <use
               xlink:href="#m9ff2572f5d"
               x="50.824642"
               y="333.1752"
               style="stroke:#000000;stroke-width:0.8"
               id="use116"
               width="100%"
               height="100%" />
          </g>
        </g>
        <g
           id="text_5">
          <!-- 0 -->
          <g
             transform="matrix(0.1,0,0,-0.1,37.462144,336.97444)"
             id="g123">
            <use
               xlink:href="#DejaVuSans-30"
               id="use121"
               x="0"
               y="0"
               width="100%"
               height="100%" />
          </g>
        </g>
      </g>
      <g
         id="ytick_3">
        <g
           id="line2d_12">
          <g
             id="g129">
            <use
               xlink:href="#m9ff2572f5d"
               x="50.824642"
               y="290.21371"
               style="stroke:#000000;stroke-width:0.8"
               id="use127"
               width="100%"
               height="100%" />
          </g>
        </g>
        <g
           id="text_6">
          <!-- 1 -->
          <g
             transform="matrix(0.1,0,0,-0.1,37.462144,294.01293)"
             id="g134">
            <use
               xlink:href="#DejaVuSans-31"
               id="use132"
               x="0"
               y="0"
               width="100%"
               height="100%" />
          </g>
        </g>
      </g>
      <g
         id="text_6-3"
         transform="translate(226.43751,109.76575)">
        <!-- 1 -->
        <g
           transform="matrix(0.1,0,0,-0.1,37.462144,294.01293)"
           id="g134-5">
          <use
             xlink:href="#DejaVuSans-31"
             id="use132-6"
             x="0"
             y="0"
             width="100%"
             height="100%" />
        </g>
      </g>
      <g
         id="ytick_4">
        <g
           id="line2d_13">
          <g
             id="g140">
            <use
               xlink:href="#m9ff2572f5d"
               x="50.824642"
               y="247.25221"
               style="stroke:#000000;stroke-width:0.8"
               id="use138"
               width="100%"
               height="100%" />
          </g>
        </g>
        <g
           id="text_7">
          <!-- 2 -->
          <g
             transform="matrix(0.1,0,0,-0.1,37.462144,251.05143)"
             id="g145">
            <path
               id="use143"
               d="M 19.1875,8.296875 H 53.609375 V 0 H 7.328125 v 8.296875 q 5.609375,5.8125 15.296875,15.59375 9.703125,9.796875 12.1875,12.640625 4.734375,5.3125 6.609375,9 1.890625,3.6875 1.890625,7.25 0,5.8125 -4.078125,9.46875 -4.078125,3.671875 -10.625,3.671875 -4.640625,0 -9.796875,-1.609375 -5.140625,-1.609375 -11,-4.890625 v 9.96875 Q 13.765625,71.78125 18.9375,73 q 5.1875,1.21875 9.484375,1.21875 11.328125,0 18.0625,-5.671875 6.734375,-5.65625 6.734375,-15.125 0,-4.5 -1.6875,-8.53125 Q 49.859375,40.875 45.40625,35.40625 44.1875,33.984375 37.640625,27.21875 31.109375,20.453125 19.1875,8.296875 Z"
               style="stroke-width:0.015625" />
          </g>
        </g>
      </g>
      <g
         id="ytick_5">
        <g
           id="line2d_14">
          <g
             id="g151">
            <use
               xlink:href="#m9ff2572f5d"
               x="50.824642"
               y="204.29071"
               style="stroke:#000000;stroke-width:0.8"
               id="use149"
               width="100%"
               height="100%" />
          </g>
        </g>
        <g
           id="text_8">
          <!-- 3 -->
          <g
             transform="matrix(0.1,0,0,-0.1,37.462144,208.08993)"
             id="g159">
            <defs
               id="defs155">
              <path
                 id="DejaVuSans-33"
                 d="m 2597,2516 q 453,-97 707,-404 255,-306 255,-756 0,-690 -475,-1069 -475,-378 -1350,-378 -293,0 -604,58 -311,58 -642,174 v 609 q 262,-153 574,-231 313,-78 654,-78 593,0 904,234 311,234 311,681 0,413 -289,645 -289,233 -804,233 h -544 v 519 h 569 q 465,0 712,186 247,186 247,536 0,359 -255,551 -254,193 -729,193 -260,0 -557,-57 -297,-56 -653,-174 v 562 q 360,100 674,150 314,50 592,50 719,0 1137,-327 419,-326 419,-882 0,-388 -222,-655 -222,-267 -631,-370 z"
                 transform="scale(0.015625)" />
            </defs>
            <use
               xlink:href="#DejaVuSans-33"
               id="use157"
               x="0"
               y="0"
               width="100%"
               height="100%" />
          </g>
        </g>
      </g>
      <g
         id="ytick_6">
        <g
           id="line2d_15">
          <g
             id="g165">
            <use
               xlink:href="#m9ff2572f5d"
               x="50.824642"
               y="161.32921"
               style="stroke:#000000;stroke-width:0.8"
               id="use163"
               width="100%"
               height="100%" />
          </g>
        </g>
        <g
           id="text_9">
          <!-- 4 -->
          <g
             transform="matrix(0.1,0,0,-0.1,37.462144,165.12842)"
             id="g173">
            <defs
               id="defs169">
              <path
                 id="DejaVuSans-34"
                 d="M 2419,4116 825,1625 h 1594 z m -166,550 h 794 V 1625 h 666 V 1100 H 3047 V 0 H 2419 V 1100 H 313 v 609 z"
                 transform="scale(0.015625)" />
            </defs>
            <use
               xlink:href="#DejaVuSans-34"
               id="use171"
               x="0"
               y="0"
               width="100%"
               height="100%" />
          </g>
        </g>
      </g>
      <g
         id="ytick_7">
        <g
           id="line2d_16">
          <g
             id="g179">
            <use
               xlink:href="#m9ff2572f5d"
               x="50.824642"
               y="118.36771"
               style="stroke:#000000;stroke-width:0.8"
               id="use177"
               width="100%"
               height="100%" />
          </g>
        </g>
        <g
           id="text_10">
          <!-- 5 -->
          <g
             transform="matrix(0.1,0,0,-0.1,37.462144,122.16692)"
             id="g187">
            <defs
               id="defs183">
              <path
                 id="DejaVuSans-35"
                 d="M 691,4666 H 3169 V 4134 H 1269 V 2991 q 137,47 274,70 138,23 276,23 781,0 1237,-428 457,-428 457,-1159 Q 3513,744 3044,326 2575,-91 1722,-91 1428,-91 1123,-41 819,9 494,109 v 635 q 281,-153 581,-228 300,-75 634,-75 541,0 856,284 316,284 316,772 0,487 -316,771 -315,285 -856,285 -253,0 -505,-56 -251,-56 -513,-175 z"
                 transform="scale(0.015625)" />
            </defs>
            <use
               xlink:href="#DejaVuSans-35"
               id="use185"
               x="0"
               y="0"
               width="100%"
               height="100%" />
          </g>
        </g>
      </g>
      <g
         id="ytick_8">
        <g
           id="line2d_17">
          <g
             id="g193">
            <use
               xlink:href="#m9ff2572f5d"
               x="50.824642"
               y="75.406204"
               style="stroke:#000000;stroke-width:0.8"
               id="use191"
               width="100%"
               height="100%" />
          </g>
        </g>
        <g
           id="text_11">
          <!-- 6 -->
          <g
             transform="matrix(0.1,0,0,-0.1,37.462144,79.20542)"
             id="g201">
            <defs
               id="defs197">
              <path
                 id="DejaVuSans-36"
                 d="m 2113,2584 q -425,0 -674,-291 -248,-290 -248,-796 0,-503 248,-796 249,-292 674,-292 425,0 673,292 248,293 248,796 0,506 -248,796 -248,291 -673,291 z m 1253,1979 v -575 q -238,112 -480,171 -242,60 -480,60 -625,0 -955,-422 -329,-422 -376,-1275 184,272 462,417 279,145 613,145 703,0 1111,-427 408,-426 408,-1160 0,-719 -425,-1154 -425,-434 -1131,-434 -810,0 -1238,620 -428,621 -428,1799 0,1106 525,1764 525,658 1409,658 238,0 480,-47 242,-47 505,-140 z"
                 transform="scale(0.015625)" />
            </defs>
            <use
               xlink:href="#DejaVuSans-36"
               id="use199"
               x="0"
               y="0"
               width="100%"
               height="100%" />
          </g>
        </g>
      </g>
      <g
         id="ytick_9">
        <g
           id="line2d_18">
          <g
             id="g207">
            <use
               xlink:href="#m9ff2572f5d"
               x="50.824642"
               y="32.444698"
               style="stroke:#000000;stroke-width:0.8"
               id="use205"
               width="100%"
               height="100%" />
          </g>
        </g>
        <g
           id="text_12">
          <!-- 7 -->
          <g
             transform="matrix(0.1,0,0,-0.1,37.462144,36.243918)"
             id="g215">
            <defs
               id="defs211">
              <path
                 id="DejaVuSans-37"
                 d="M 525,4666 H 3525 V 4397 L 1831,0 H 1172 L 2766,4134 H 525 Z"
                 transform="scale(0.015625)" />
            </defs>
            <use
               xlink:href="#DejaVuSans-37"
               id="use213"
               x="0"
               y="0"
               width="100%"
               height="100%" />
          </g>
        </g>
      </g>
      <g
         id="line2d_19">
        <path
           d="M 122.802739 333.236699  L 266.758928 333.385028  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #5babb8; stroke-width: 2.7; stroke-linecap: square"
           id="path239" />
      </g>
      <g
         id="line2d_20">
        <path
           d="M 122.802739 333.259728  L 122.802739 333.212606  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #5babb8; stroke-width: 2.7; stroke-linecap: square"
           id="path242" />
      </g>
      <g
         id="line2d_21">
        <path
           d="M 108.40712 333.259728  L 137.198358 333.259728  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #5babb8; stroke-width: 2.7; stroke-linecap: square"
           id="path245" />
      </g>
      <g
         id="line2d_22">
        <path
           d="M 108.40712 333.212606  L 137.198358 333.212606  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #5babb8; stroke-width: 2.7; stroke-linecap: square"
           id="path248" />
      </g>
      <g
         id="line2d_23">
        <path
           d="M 266.758928 333.60429  L 266.758928 333.209183  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #5babb8; stroke-width: 2.7; stroke-linecap: square"
           id="path251" />
      </g>
      <g
         id="line2d_24">
        <path
           d="M 252.363309 333.60429  L 281.154547 333.60429  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #5babb8; stroke-width: 2.7; stroke-linecap: square"
           id="path254" />
      </g>
      <g
         id="line2d_25">
        <path
           d="M 252.363309 333.209183  L 281.154547 333.209183  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #5babb8; stroke-width: 2.7; stroke-linecap: square"
           id="path257" />
      </g>
      <g
         id="PathCollection_2">
        <defs
           id="defs261">
          <path
             id="m396209a82e"
             d="M 0 3.383948  C 0.897434 3.383948 1.758231 3.027394 2.392813 2.392813  C 3.027394 1.758231 3.383948 0.897434 3.383948 0  C 3.383948 -0.897434 3.027394 -1.758231 2.392813 -2.392813  C 1.758231 -3.027394 0.897434 -3.383948 0 -3.383948  C -0.897434 -3.383948 -1.758231 -3.027394 -2.392813 -2.392813  C -3.027394 -1.758231 -3.383948 -0.897434 -3.383948 0  C -3.383948 0.897434 -3.027394 1.758231 -2.392813 2.392813  C -1.758231 3.027394 -0.897434 3.383948 0 3.383948  z "
             style="stroke: #5babb8; stroke-width: 2.025" />
        </defs>
        <g
           clip-path="url(#p611c9b1e85)"
           id="g267">
          <use
             xlink:href="#m396209a82e"
             x="122.802739"
             y="333.236699"
             style="fill: #5babb8; stroke: #5babb8; stroke-width: 2.025"
             id="use263" />
          <use
             xlink:href="#m396209a82e"
             x="266.758928"
             y="333.385028"
             style="fill: #5babb8; stroke: #5babb8; stroke-width: 2.025"
             id="use265" />
        </g>
      </g>
      <g
         id="patch_3">
        <path
           d="M 50.824644 390.04  L 50.824644 24.72  "
           style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"
           id="path270" />
      </g>
      <g
         id="patch_4">
        <path
           d="M 50.824644 390.04  L 338.737023 390.04  "
           style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"
           id="path273" />
      </g>
      <g
         id="line2d_26">
        <path
           d="M 122.802739 333.220054  L 266.758928 333.354113  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #3c95b8; stroke-width: 2.7; stroke-linecap: square"
           id="path276" />
      </g>
      <g
         id="line2d_27">
        <path
           d="M 122.802739 333.244915  L 122.802739 333.200876  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #3c95b8; stroke-width: 2.7; stroke-linecap: square"
           id="path279" />
      </g>
      <g
         id="line2d_28">
        <path
           d="M 108.40712 333.244915  L 137.198358 333.244915  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #3c95b8; stroke-width: 2.7; stroke-linecap: square"
           id="path282" />
      </g>
      <g
         id="line2d_29">
        <path
           d="M 108.40712 333.200876  L 137.198358 333.200876  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #3c95b8; stroke-width: 2.7; stroke-linecap: square"
           id="path285" />
      </g>
      <g
         id="line2d_30">
        <path
           d="M 266.758928 333.449475  L 266.758928 333.271623  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #3c95b8; stroke-width: 2.7; stroke-linecap: square"
           id="path288" />
      </g>
      <g
         id="line2d_31">
        <path
           d="M 252.363309 333.449475  L 281.154547 333.449475  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #3c95b8; stroke-width: 2.7; stroke-linecap: square"
           id="path291" />
      </g>
      <g
         id="line2d_32">
        <path
           d="M 252.363309 333.271623  L 281.154547 333.271623  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #3c95b8; stroke-width: 2.7; stroke-linecap: square"
           id="path294" />
      </g>
      <g
         id="PathCollection_3">
        <defs
           id="defs298">
          <path
             id="mb5648846fd"
             d="M 0 3.383948  C 0.897434 3.383948 1.758231 3.027394 2.392813 2.392813  C 3.027394 1.758231 3.383948 0.897434 3.383948 0  C 3.383948 -0.897434 3.027394 -1.758231 2.392813 -2.392813  C 1.758231 -3.027394 0.897434 -3.383948 0 -3.383948  C -0.897434 -3.383948 -1.758231 -3.027394 -2.392813 -2.392813  C -3.027394 -1.758231 -3.383948 -0.897434 -3.383948 0  C -3.383948 0.897434 -3.027394 1.758231 -2.392813 2.392813  C -1.758231 3.027394 -0.897434 3.383948 0 3.383948  z "
             style="stroke: #3c95b8; stroke-width: 2.025" />
        </defs>
        <g
           clip-path="url(#p611c9b1e85)"
           id="g304">
          <use
             xlink:href="#mb5648846fd"
             x="122.802739"
             y="333.220054"
             style="fill: #3c95b8; stroke: #3c95b8; stroke-width: 2.025"
             id="use300" />
          <use
             xlink:href="#mb5648846fd"
             x="266.758928"
             y="333.354113"
             style="fill: #3c95b8; stroke: #3c95b8; stroke-width: 2.025"
             id="use302" />
        </g>
      </g>
      <g
         id="line2d_33">
        <path
           d="M 122.802739 333.225737  L 266.758928 333.35439  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #1f80b7; stroke-width: 2.7; stroke-linecap: square"
           id="path343" />
      </g>
      <g
         id="line2d_34">
        <path
           d="M 122.802739 333.261172  L 122.802739 333.198117  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #1f80b7; stroke-width: 2.7; stroke-linecap: square"
           id="path346" />
      </g>
      <g
         id="line2d_35">
        <path
           d="M 108.40712 333.261172  L 137.198358 333.261172  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #1f80b7; stroke-width: 2.7; stroke-linecap: square"
           id="path349" />
      </g>
      <g
         id="line2d_36">
        <path
           d="M 108.40712 333.198117  L 137.198358 333.198117  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #1f80b7; stroke-width: 2.7; stroke-linecap: square"
           id="path352" />
      </g>
      <g
         id="line2d_37">
        <path
           d="M 266.758928 333.441544  L 266.758928 333.26601  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #1f80b7; stroke-width: 2.7; stroke-linecap: square"
           id="path355" />
      </g>
      <g
         id="line2d_38">
        <path
           d="M 252.363309 333.441544  L 281.154547 333.441544  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #1f80b7; stroke-width: 2.7; stroke-linecap: square"
           id="path358" />
      </g>
      <g
         id="line2d_39">
        <path
           d="M 252.363309 333.26601  L 281.154547 333.26601  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #1f80b7; stroke-width: 2.7; stroke-linecap: square"
           id="path361" />
      </g>
      <g
         id="PathCollection_4">
        <defs
           id="defs365">
          <path
             id="m5cc345a50f"
             d="M 0 3.383948  C 0.897434 3.383948 1.758231 3.027394 2.392813 2.392813  C 3.027394 1.758231 3.383948 0.897434 3.383948 0  C 3.383948 -0.897434 3.027394 -1.758231 2.392813 -2.392813  C 1.758231 -3.027394 0.897434 -3.383948 0 -3.383948  C -0.897434 -3.383948 -1.758231 -3.027394 -2.392813 -2.392813  C -3.027394 -1.758231 -3.383948 -0.897434 -3.383948 0  C -3.383948 0.897434 -3.027394 1.758231 -2.392813 2.392813  C -1.758231 3.027394 -0.897434 3.383948 0 3.383948  z "
             style="stroke: #1f80b7; stroke-width: 2.025" />
        </defs>
        <g
           clip-path="url(#p611c9b1e85)"
           id="g371">
          <use
             xlink:href="#m5cc345a50f"
             x="122.802739"
             y="333.225737"
             style="fill: #1f80b7; stroke: #1f80b7; stroke-width: 2.025"
             id="use367" />
          <use
             xlink:href="#m5cc345a50f"
             x="266.758928"
             y="333.35439"
             style="fill: #1f80b7; stroke: #1f80b7; stroke-width: 2.025"
             id="use369" />
        </g>
      </g>
      <g
         id="line2d_40">
        <path
           d="M 122.802739 333.219879  L 266.758928 333.424611  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #246c96; stroke-width: 2.7; stroke-linecap: square"
           id="path374" />
      </g>
      <g
         id="line2d_41">
        <path
           d="M 122.802739 333.247104  L 122.802739 333.19804  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #246c96; stroke-width: 2.7; stroke-linecap: square"
           id="path377" />
      </g>
      <g
         id="line2d_42">
        <path
           d="M 108.40712 333.247104  L 137.198358 333.247104  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #246c96; stroke-width: 2.7; stroke-linecap: square"
           id="path380" />
      </g>
      <g
         id="line2d_43">
        <path
           d="M 108.40712 333.19804  L 137.198358 333.19804  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #246c96; stroke-width: 2.7; stroke-linecap: square"
           id="path383" />
      </g>
      <g
         id="line2d_44">
        <path
           d="M 266.758928 333.71055  L 266.758928 333.25991  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #246c96; stroke-width: 2.7; stroke-linecap: square"
           id="path386" />
      </g>
      <g
         id="line2d_45">
        <path
           d="M 252.363309 333.71055  L 281.154547 333.71055  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #246c96; stroke-width: 2.7; stroke-linecap: square"
           id="path389" />
      </g>
      <g
         id="line2d_46">
        <path
           d="M 252.363309 333.25991  L 281.154547 333.25991  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #246c96; stroke-width: 2.7; stroke-linecap: square"
           id="path392" />
      </g>
      <g
         id="PathCollection_5">
        <defs
           id="defs396">
          <path
             id="macfee0750b"
             d="M 0 3.383948  C 0.897434 3.383948 1.758231 3.027394 2.392813 2.392813  C 3.027394 1.758231 3.383948 0.897434 3.383948 0  C 3.383948 -0.897434 3.027394 -1.758231 2.392813 -2.392813  C 1.758231 -3.027394 0.897434 -3.383948 0 -3.383948  C -0.897434 -3.383948 -1.758231 -3.027394 -2.392813 -2.392813  C -3.027394 -1.758231 -3.383948 -0.897434 -3.383948 0  C -3.383948 0.897434 -3.027394 1.758231 -2.392813 2.392813  C -1.758231 3.027394 -0.897434 3.383948 0 3.383948  z "
             style="stroke: #246c96; stroke-width: 2.025" />
        </defs>
        <g
           clip-path="url(#p611c9b1e85)"
           id="g402">
          <use
             xlink:href="#macfee0750b"
             x="122.802739"
             y="333.219879"
             style="fill: #246c96; stroke: #246c96; stroke-width: 2.025"
             id="use398" />
          <use
             xlink:href="#macfee0750b"
             x="266.758928"
             y="333.424611"
             style="fill: #246c96; stroke: #246c96; stroke-width: 2.025"
             id="use400" />
        </g>
      </g>
      <g
         id="line2d_47">
        <path
           d="M 122.802739 333.238266  L 266.758928 333.604554  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #295975; stroke-width: 2.7; stroke-linecap: square"
           id="path405" />
      </g>
      <g
         id="line2d_48">
        <path
           d="M 122.802739 333.269937  L 122.802739 333.20791  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #295975; stroke-width: 2.7; stroke-linecap: square"
           id="path408" />
      </g>
      <g
         id="line2d_49">
        <path
           d="M 108.40712 333.269937  L 137.198358 333.269937  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #295975; stroke-width: 2.7; stroke-linecap: square"
           id="path411" />
      </g>
      <g
         id="line2d_50">
        <path
           d="M 108.40712 333.20791  L 137.198358 333.20791  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #295975; stroke-width: 2.7; stroke-linecap: square"
           id="path414" />
      </g>
      <g
         id="line2d_51">
        <path
           d="M 266.758928 333.918623  L 266.758928 333.317971  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #295975; stroke-width: 2.7; stroke-linecap: square"
           id="path417" />
      </g>
      <g
         id="line2d_52">
        <path
           d="M 252.363309 333.918623  L 281.154547 333.918623  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #295975; stroke-width: 2.7; stroke-linecap: square"
           id="path420" />
      </g>
      <g
         id="line2d_53">
        <path
           d="M 252.363309 333.317971  L 281.154547 333.317971  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #295975; stroke-width: 2.7; stroke-linecap: square"
           id="path423" />
      </g>
      <g
         id="PathCollection_6">
        <defs
           id="defs427">
          <path
             id="m5b74f1468f"
             d="M 0 3.383948  C 0.897434 3.383948 1.758231 3.027394 2.392813 2.392813  C 3.027394 1.758231 3.383948 0.897434 3.383948 0  C 3.383948 -0.897434 3.027394 -1.758231 2.392813 -2.392813  C 1.758231 -3.027394 0.897434 -3.383948 0 -3.383948  C -0.897434 -3.383948 -1.758231 -3.027394 -2.392813 -2.392813  C -3.027394 -1.758231 -3.383948 -0.897434 -3.383948 0  C -3.383948 0.897434 -3.027394 1.758231 -2.392813 2.392813  C -1.758231 3.027394 -0.897434 3.383948 0 3.383948  z "
             style="stroke: #295975; stroke-width: 2.025" />
        </defs>
        <g
           clip-path="url(#p611c9b1e85)"
           id="g433">
          <use
             xlink:href="#m5b74f1468f"
             x="122.802739"
             y="333.238266"
             style="fill: #295975; stroke: #295975; stroke-width: 2.025"
             id="use429" />
          <use
             xlink:href="#m5b74f1468f"
             x="266.758928"
             y="333.604554"
             style="fill: #295975; stroke: #295975; stroke-width: 2.025"
             id="use431" />
        </g>
      </g>
      <g
         id="line2d_54">
        <path
           d="M 122.802739 333.236312  L 266.758928 333.256407  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #2e4653; stroke-width: 2.7; stroke-linecap: square"
           id="path436" />
      </g>
      <g
         id="line2d_55">
        <path
           d="M 122.802739 333.276298  L 122.802739 333.196326  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #2e4653; stroke-width: 2.7; stroke-linecap: square"
           id="path439" />
      </g>
      <g
         id="line2d_56">
        <path
           d="M 108.40712 333.276298  L 137.198358 333.276298  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #2e4653; stroke-width: 2.7; stroke-linecap: square"
           id="path442" />
      </g>
      <g
         id="line2d_57">
        <path
           d="M 108.40712 333.196326  L 137.198358 333.196326  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #2e4653; stroke-width: 2.7; stroke-linecap: square"
           id="path445" />
      </g>
      <g
         id="line2d_58">
        <path
           d="M 266.758928 333.336082  L 266.758928 333.192861  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #2e4653; stroke-width: 2.7; stroke-linecap: square"
           id="path448" />
      </g>
      <g
         id="line2d_59">
        <path
           d="M 252.363309 333.336082  L 281.154547 333.336082  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #2e4653; stroke-width: 2.7; stroke-linecap: square"
           id="path451" />
      </g>
      <g
         id="line2d_60">
        <path
           d="M 252.363309 333.192861  L 281.154547 333.192861  "
           clip-path="url(#p611c9b1e85)"
           style="fill: none; stroke: #2e4653; stroke-width: 2.7; stroke-linecap: square"
           id="path454" />
      </g>
      <g
         id="PathCollection_7">
        <defs
           id="defs458">
          <path
             id="me8a9bf0cd3"
             d="M 0 3.383948  C 0.897434 3.383948 1.758231 3.027394 2.392813 2.392813  C 3.027394 1.758231 3.383948 0.897434 3.383948 0  C 3.383948 -0.897434 3.027394 -1.758231 2.392813 -2.392813  C 1.758231 -3.027394 0.897434 -3.383948 0 -3.383948  C -0.897434 -3.383948 -1.758231 -3.027394 -2.392813 -2.392813  C -3.027394 -1.758231 -3.383948 -0.897434 -3.383948 0  C -3.383948 0.897434 -3.027394 1.758231 -2.392813 2.392813  C -1.758231 3.027394 -0.897434 3.383948 0 3.383948  z "
             style="stroke: #2e4653; stroke-width: 2.025" />
        </defs>
        <g
           clip-path="url(#p611c9b1e85)"
           id="g464">
          <use
             xlink:href="#me8a9bf0cd3"
             x="122.802739"
             y="333.236312"
             style="fill: #2e4653; stroke: #2e4653; stroke-width: 2.025"
             id="use460" />
          <use
             xlink:href="#me8a9bf0cd3"
             x="266.758928"
             y="333.256407"
             style="fill: #2e4653; stroke: #2e4653; stroke-width: 2.025"
             id="use462" />
        </g>
      </g>
      <g
         id="xtick_2-2"
         style="stroke-linecap:butt;stroke-linejoin:round"
         transform="translate(302.34497,-0.7831204)">
        <g
           id="line2d_9-7">
          <g
             id="g64-0">
            <use
               xlink:href="#mfe2ed4cf68"
               x="266.75894"
               y="390.04001"
               style="stroke:#000000;stroke-width:0.8"
               id="use62-9"
               width="100%"
               height="100%" />
          </g>
        </g>
      </g>
      <g
         id="text_6-3-3"
         transform="translate(528.78248,108.98263)"
         style="stroke-linecap:butt;stroke-linejoin:round">
        <!-- 1 -->
        <g
           transform="matrix(0.1,0,0,-0.1,37.462144,294.01293)"
           id="g134-5-6">
          <use
             xlink:href="#DejaVuSans-31"
             id="use132-6-0"
             x="0"
             y="0"
             width="100%"
             height="100%" />
        </g>
      </g>
      <text
         xml:space="preserve"
         style="font-style:normal;font-weight:normal;font-size:15px;line-height:1.25;font-family:sans-serif;letter-spacing:0px;word-spacing:0px;fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.75"
         x="-332.39844"
         y="24.599716"
         id="text4404"
         transform="rotate(-90)"><tspan
           sodipodi:role="line"
           id="tspan4402"
           style="font-size:15px;stroke-width:0.75"
           x="-332.39844"
           y="24.599716">Difference in learned action-values</tspan></text>
      <text
         xml:space="preserve"
         style="font-style:normal;font-weight:normal;font-size:15px;line-height:1.25;font-family:sans-serif;letter-spacing:0px;word-spacing:0px;fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.75"
         x="178.55733"
         y="415.82516"
         id="text46299"><tspan
           sodipodi:role="line"
           id="tspan46297"
           style="font-size:15px;stroke-width:0.75"
           x="178.55733"
           y="415.82516">State</tspan></text>
      <text
         xml:space="preserve"
         style="font-style:normal;font-weight:normal;font-size:15px;line-height:1.25;font-family:sans-serif;letter-spacing:0px;word-spacing:0px;fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.75;stroke-linecap:butt;stroke-linejoin:round"
         x="484.57419"
         y="414.68759"
         id="text46299-6"><tspan
           sodipodi:role="line"
           id="tspan46297-2"
           style="font-size:15px;stroke-width:0.75"
           x="484.57419"
           y="414.68759">State</tspan></text>
    </g>
    <g
       id="axes_2">
      <g
         id="patch_5">
        <path
           d="M 353.026222 390.04  L 640.9386 390.04  L 640.9386 24.72  L 353.026222 24.72  z "
           style="fill: #ffffff"
           id="path468" />
      </g>
      <g
         id="line2d_61">
        <path
           d="M 425.004316 326.368031  L 568.960505 360.648946  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #79c1b8; stroke-width: 2.7; stroke-linecap: square"
           id="path471" />
      </g>
      <g
         id="line2d_62">
        <path
           d="M 425.004316 356.926885  L 425.004316 295.951556  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #79c1b8; stroke-width: 2.7; stroke-linecap: square"
           id="path474" />
      </g>
      <g
         id="line2d_63">
        <path
           d="M 410.608698 356.926885  L 439.399935 356.926885  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #79c1b8; stroke-width: 2.7; stroke-linecap: square"
           id="path477" />
      </g>
      <g
         id="line2d_64">
        <path
           d="M 410.608698 295.951556  L 439.399935 295.951556  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #79c1b8; stroke-width: 2.7; stroke-linecap: square"
           id="path480" />
      </g>
      <g
         id="line2d_65">
        <path
           d="M 568.960505 373.434545  L 568.960505 346.842662  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #79c1b8; stroke-width: 2.7; stroke-linecap: square"
           id="path483" />
      </g>
      <g
         id="line2d_66">
        <path
           d="M 554.564887 373.434545  L 583.356124 373.434545  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #79c1b8; stroke-width: 2.7; stroke-linecap: square"
           id="path486" />
      </g>
      <g
         id="line2d_67">
        <path
           d="M 554.564887 346.842662  L 583.356124 346.842662  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #79c1b8; stroke-width: 2.7; stroke-linecap: square"
           id="path489" />
      </g>
      <g
         id="PathCollection_8">
        <g
           clip-path="url(#pf85bd4df92)"
           id="g496">
          <use
             xlink:href="#m67a9cd3a9b"
             x="425.004316"
             y="326.368031"
             style="fill: #79c1b8; stroke: #79c1b8; stroke-width: 2.025"
             id="use492" />
          <use
             xlink:href="#m67a9cd3a9b"
             x="568.960505"
             y="360.648946"
             style="fill: #79c1b8; stroke: #79c1b8; stroke-width: 2.025"
             id="use494" />
        </g>
      </g>
      <g
         id="matplotlib.axis_3">
        <g
           id="xtick_3">
          <g
             id="line2d_68">
            <g
               id="g501">
              <use
                 xlink:href="#mfe2ed4cf68"
                 x="425.004316"
                 y="390.04"
                 style="stroke: #000000; stroke-width: 0.8"
                 id="use499" />
            </g>
          </g>
          <g
             id="text_15">
            <!-- 0 -->
            <g
               transform="translate(421.823066 404.638438)scale(0.1 -0.1)"
               id="g506">
              <use
                 xlink:href="#DejaVuSans-30"
                 id="use504" />
            </g>
          </g>
        </g>
      </g>
      <g
         id="matplotlib.axis_4">
        <g
           id="ytick_10">
          <g
             id="line2d_70">
            <g
               id="g537">
              <use
                 xlink:href="#m9ff2572f5d"
                 x="353.026222"
                 y="376.136718"
                 style="stroke: #000000; stroke-width: 0.8"
                 id="use535" />
            </g>
          </g>
        </g>
        <g
           id="ytick_11">
          <g
             id="line2d_71">
            <g
               id="g543">
              <use
                 xlink:href="#m9ff2572f5d"
                 x="353.026222"
                 y="333.175216"
                 style="stroke: #000000; stroke-width: 0.8"
                 id="use541" />
            </g>
          </g>
        </g>
        <g
           id="ytick_12">
          <g
             id="line2d_72">
            <g
               id="g549">
              <use
                 xlink:href="#m9ff2572f5d"
                 x="353.026222"
                 y="290.213713"
                 style="stroke: #000000; stroke-width: 0.8"
                 id="use547" />
            </g>
          </g>
        </g>
        <g
           id="ytick_13">
          <g
             id="line2d_73">
            <g
               id="g555">
              <use
                 xlink:href="#m9ff2572f5d"
                 x="353.026222"
                 y="247.252211"
                 style="stroke: #000000; stroke-width: 0.8"
                 id="use553" />
            </g>
          </g>
        </g>
        <g
           id="ytick_14">
          <g
             id="line2d_74">
            <g
               id="g561">
              <use
                 xlink:href="#m9ff2572f5d"
                 x="353.026222"
                 y="204.290709"
                 style="stroke: #000000; stroke-width: 0.8"
                 id="use559" />
            </g>
          </g>
        </g>
        <g
           id="ytick_15">
          <g
             id="line2d_75">
            <g
               id="g567">
              <use
                 xlink:href="#m9ff2572f5d"
                 x="353.026222"
                 y="161.329206"
                 style="stroke: #000000; stroke-width: 0.8"
                 id="use565" />
            </g>
          </g>
        </g>
        <g
           id="ytick_16">
          <g
             id="line2d_76">
            <g
               id="g573">
              <use
                 xlink:href="#m9ff2572f5d"
                 x="353.026222"
                 y="118.367704"
                 style="stroke: #000000; stroke-width: 0.8"
                 id="use571" />
            </g>
          </g>
        </g>
        <g
           id="ytick_17">
          <g
             id="line2d_77">
            <g
               id="g579">
              <use
                 xlink:href="#m9ff2572f5d"
                 x="353.026222"
                 y="75.406201"
                 style="stroke: #000000; stroke-width: 0.8"
                 id="use577" />
            </g>
          </g>
        </g>
        <g
           id="ytick_18">
          <g
             id="line2d_78">
            <g
               id="g585">
              <use
                 xlink:href="#m9ff2572f5d"
                 x="353.026222"
                 y="32.444699"
                 style="stroke: #000000; stroke-width: 0.8"
                 id="use583" />
            </g>
          </g>
        </g>
      </g>
      <g
         id="line2d_79">
        <path
           d="M 425.004316 326.896068  L 568.960505 341.741271  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #5babb8; stroke-width: 2.7; stroke-linecap: square"
           id="path590" />
      </g>
      <g
         id="line2d_80">
        <path
           d="M 425.004316 330.00474  L 425.004316 324.633243  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #5babb8; stroke-width: 2.7; stroke-linecap: square"
           id="path593" />
      </g>
      <g
         id="line2d_81">
        <path
           d="M 410.608698 330.00474  L 439.399935 330.00474  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #5babb8; stroke-width: 2.7; stroke-linecap: square"
           id="path596" />
      </g>
      <g
         id="line2d_82">
        <path
           d="M 410.608698 324.633243  L 439.399935 324.633243  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #5babb8; stroke-width: 2.7; stroke-linecap: square"
           id="path599" />
      </g>
      <g
         id="line2d_83">
        <path
           d="M 568.960505 356.353354  L 568.960505 333.652696  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #5babb8; stroke-width: 2.7; stroke-linecap: square"
           id="path602" />
      </g>
      <g
         id="line2d_84">
        <path
           d="M 554.564887 356.353354  L 583.356124 356.353354  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #5babb8; stroke-width: 2.7; stroke-linecap: square"
           id="path605" />
      </g>
      <g
         id="line2d_85">
        <path
           d="M 554.564887 333.652696  L 583.356124 333.652696  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #5babb8; stroke-width: 2.7; stroke-linecap: square"
           id="path608" />
      </g>
      <g
         id="PathCollection_9">
        <g
           clip-path="url(#pf85bd4df92)"
           id="g615">
          <use
             xlink:href="#m396209a82e"
             x="425.004316"
             y="326.896068"
             style="fill: #5babb8; stroke: #5babb8; stroke-width: 2.025"
             id="use611" />
          <use
             xlink:href="#m396209a82e"
             x="568.960505"
             y="341.741271"
             style="fill: #5babb8; stroke: #5babb8; stroke-width: 2.025"
             id="use613" />
        </g>
      </g>
      <g
         id="patch_6">
        <path
           d="M 353.026222 390.04  L 353.026222 24.72  "
           style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"
           id="path618" />
      </g>
      <g
         id="patch_7">
        <path
           d="M 353.026222 390.04  L 640.9386 390.04  "
           style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"
           id="path621" />
      </g>
      <g
         id="line2d_86">
        <path
           d="M 425.004316 318.628128  L 568.960505 333.286363  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #3c95b8; stroke-width: 2.7; stroke-linecap: square"
           id="path624" />
      </g>
      <g
         id="line2d_87">
        <path
           d="M 425.004316 318.765767  L 425.004316 318.501054  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #3c95b8; stroke-width: 2.7; stroke-linecap: square"
           id="path627" />
      </g>
      <g
         id="line2d_88">
        <path
           d="M 410.608698 318.765767  L 439.399935 318.765767  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #3c95b8; stroke-width: 2.7; stroke-linecap: square"
           id="path630" />
      </g>
      <g
         id="line2d_89">
        <path
           d="M 410.608698 318.501054  L 439.399935 318.501054  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #3c95b8; stroke-width: 2.7; stroke-linecap: square"
           id="path633" />
      </g>
      <g
         id="line2d_90">
        <path
           d="M 568.960505 333.402341  L 568.960505 333.201811  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #3c95b8; stroke-width: 2.7; stroke-linecap: square"
           id="path636" />
      </g>
      <g
         id="line2d_91">
        <path
           d="M 554.564887 333.402341  L 583.356124 333.402341  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #3c95b8; stroke-width: 2.7; stroke-linecap: square"
           id="path639" />
      </g>
      <g
         id="line2d_92">
        <path
           d="M 554.564887 333.201811  L 583.356124 333.201811  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #3c95b8; stroke-width: 2.7; stroke-linecap: square"
           id="path642" />
      </g>
      <g
         id="PathCollection_10">
        <g
           clip-path="url(#pf85bd4df92)"
           id="g649">
          <use
             xlink:href="#mb5648846fd"
             x="425.004316"
             y="318.628128"
             style="fill: #3c95b8; stroke: #3c95b8; stroke-width: 2.025"
             id="use645" />
          <use
             xlink:href="#mb5648846fd"
             x="568.960505"
             y="333.286363"
             style="fill: #3c95b8; stroke: #3c95b8; stroke-width: 2.025"
             id="use647" />
        </g>
      </g>
      <g
         id="line2d_93">
        <path
           d="M 425.004316 304.012521  L 568.960505 333.237285  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #1f80b7; stroke-width: 2.7; stroke-linecap: square"
           id="path693" />
      </g>
      <g
         id="line2d_94">
        <path
           d="M 425.004316 304.074457  L 425.004316 303.969599  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #1f80b7; stroke-width: 2.7; stroke-linecap: square"
           id="path696" />
      </g>
      <g
         id="line2d_95">
        <path
           d="M 410.608698 304.074457  L 439.399935 304.074457  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #1f80b7; stroke-width: 2.7; stroke-linecap: square"
           id="path699" />
      </g>
      <g
         id="line2d_96">
        <path
           d="M 410.608698 303.969599  L 439.399935 303.969599  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #1f80b7; stroke-width: 2.7; stroke-linecap: square"
           id="path702" />
      </g>
      <g
         id="line2d_97">
        <path
           d="M 568.960505 333.312567  L 568.960505 333.195421  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #1f80b7; stroke-width: 2.7; stroke-linecap: square"
           id="path705" />
      </g>
      <g
         id="line2d_98">
        <path
           d="M 554.564887 333.312567  L 583.356124 333.312567  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #1f80b7; stroke-width: 2.7; stroke-linecap: square"
           id="path708" />
      </g>
      <g
         id="line2d_99">
        <path
           d="M 554.564887 333.195421  L 583.356124 333.195421  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #1f80b7; stroke-width: 2.7; stroke-linecap: square"
           id="path711" />
      </g>
      <g
         id="PathCollection_11">
        <g
           clip-path="url(#pf85bd4df92)"
           id="g718">
          <use
             xlink:href="#m5cc345a50f"
             x="425.004316"
             y="304.012521"
             style="fill: #1f80b7; stroke: #1f80b7; stroke-width: 2.025"
             id="use714" />
          <use
             xlink:href="#m5cc345a50f"
             x="568.960505"
             y="333.237285"
             style="fill: #1f80b7; stroke: #1f80b7; stroke-width: 2.025"
             id="use716" />
        </g>
      </g>
      <g
         id="line2d_100">
        <path
           d="M 425.004316 274.8156  L 568.960505 333.210642  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #246c96; stroke-width: 2.7; stroke-linecap: square"
           id="path721" />
      </g>
      <g
         id="line2d_101">
        <path
           d="M 425.004316 274.856461  L 425.004316 274.781211  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #246c96; stroke-width: 2.7; stroke-linecap: square"
           id="path724" />
      </g>
      <g
         id="line2d_102">
        <path
           d="M 410.608698 274.856461  L 439.399935 274.856461  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #246c96; stroke-width: 2.7; stroke-linecap: square"
           id="path727" />
      </g>
      <g
         id="line2d_103">
        <path
           d="M 410.608698 274.781211  L 439.399935 274.781211  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #246c96; stroke-width: 2.7; stroke-linecap: square"
           id="path730" />
      </g>
      <g
         id="line2d_104">
        <path
           d="M 568.960505 333.247799  L 568.960505 333.179861  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #246c96; stroke-width: 2.7; stroke-linecap: square"
           id="path733" />
      </g>
      <g
         id="line2d_105">
        <path
           d="M 554.564887 333.247799  L 583.356124 333.247799  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #246c96; stroke-width: 2.7; stroke-linecap: square"
           id="path736" />
      </g>
      <g
         id="line2d_106">
        <path
           d="M 554.564887 333.179861  L 583.356124 333.179861  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #246c96; stroke-width: 2.7; stroke-linecap: square"
           id="path739" />
      </g>
      <g
         id="PathCollection_12">
        <g
           clip-path="url(#pf85bd4df92)"
           id="g746">
          <use
             xlink:href="#macfee0750b"
             x="425.004316"
             y="274.8156"
             style="fill: #246c96; stroke: #246c96; stroke-width: 2.025"
             id="use742" />
          <use
             xlink:href="#macfee0750b"
             x="568.960505"
             y="333.210642"
             style="fill: #246c96; stroke: #246c96; stroke-width: 2.025"
             id="use744" />
        </g>
      </g>
      <g
         id="line2d_107">
        <path
           d="M 425.004316 187.259239  L 568.960505 333.202816  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #295975; stroke-width: 2.7; stroke-linecap: square"
           id="path749" />
      </g>
      <g
         id="line2d_108">
        <path
           d="M 425.004316 187.275354  L 425.004316 187.232799  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #295975; stroke-width: 2.7; stroke-linecap: square"
           id="path752" />
      </g>
      <g
         id="line2d_109">
        <path
           d="M 410.608698 187.275354  L 439.399935 187.275354  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #295975; stroke-width: 2.7; stroke-linecap: square"
           id="path755" />
      </g>
      <g
         id="line2d_110">
        <path
           d="M 410.608698 187.232799  L 439.399935 187.232799  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #295975; stroke-width: 2.7; stroke-linecap: square"
           id="path758" />
      </g>
      <g
         id="line2d_111">
        <path
           d="M 568.960505 333.215381  L 568.960505 333.189018  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #295975; stroke-width: 2.7; stroke-linecap: square"
           id="path761" />
      </g>
      <g
         id="line2d_112">
        <path
           d="M 554.564887 333.215381  L 583.356124 333.215381  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #295975; stroke-width: 2.7; stroke-linecap: square"
           id="path764" />
      </g>
      <g
         id="line2d_113">
        <path
           d="M 554.564887 333.189018  L 583.356124 333.189018  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #295975; stroke-width: 2.7; stroke-linecap: square"
           id="path767" />
      </g>
      <g
         id="PathCollection_13">
        <g
           clip-path="url(#pf85bd4df92)"
           id="g774">
          <use
             xlink:href="#m5b74f1468f"
             x="425.004316"
             y="187.259239"
             style="fill: #295975; stroke: #295975; stroke-width: 2.025"
             id="use770" />
          <use
             xlink:href="#m5b74f1468f"
             x="568.960505"
             y="333.202816"
             style="fill: #295975; stroke: #295975; stroke-width: 2.025"
             id="use772" />
        </g>
      </g>
      <g
         id="line2d_114">
        <path
           d="M 425.004316 41.340802  L 568.960505 333.194529  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #2e4653; stroke-width: 2.7; stroke-linecap: square"
           id="path777" />
      </g>
      <g
         id="line2d_115">
        <path
           d="M 425.004316 41.35615  L 425.004316 41.325455  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #2e4653; stroke-width: 2.7; stroke-linecap: square"
           id="path780" />
      </g>
      <g
         id="line2d_116">
        <path
           d="M 410.608698 41.35615  L 439.399935 41.35615  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #2e4653; stroke-width: 2.7; stroke-linecap: square"
           id="path783" />
      </g>
      <g
         id="line2d_117">
        <path
           d="M 410.608698 41.325455  L 439.399935 41.325455  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #2e4653; stroke-width: 2.7; stroke-linecap: square"
           id="path786" />
      </g>
      <g
         id="line2d_118">
        <path
           d="M 568.960505 333.200035  L 568.960505 333.188466  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #2e4653; stroke-width: 2.7; stroke-linecap: square"
           id="path789" />
      </g>
      <g
         id="line2d_119">
        <path
           d="M 554.564887 333.200035  L 583.356124 333.200035  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #2e4653; stroke-width: 2.7; stroke-linecap: square"
           id="path792" />
      </g>
      <g
         id="line2d_120">
        <path
           d="M 554.564887 333.188466  L 583.356124 333.188466  "
           clip-path="url(#pf85bd4df92)"
           style="fill: none; stroke: #2e4653; stroke-width: 2.7; stroke-linecap: square"
           id="path795" />
      </g>
      <g
         id="PathCollection_14">
        <g
           clip-path="url(#pf85bd4df92)"
           id="g802">
          <use
             xlink:href="#me8a9bf0cd3"
             x="425.004316"
             y="41.340802"
             style="fill: #2e4653; stroke: #2e4653; stroke-width: 2.025"
             id="use798" />
          <use
             xlink:href="#me8a9bf0cd3"
             x="568.960505"
             y="333.194529"
             style="fill: #2e4653; stroke: #2e4653; stroke-width: 2.025"
             id="use800" />
        </g>
      </g>
      <text
         xml:space="preserve"
         style="font-style:normal;font-weight:normal;font-size:20px;line-height:1.25;font-family:sans-serif;letter-spacing:0px;word-spacing:0px;fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.75"
         x="166.16837"
         y="18.899153"
         id="text57992"><tspan
           sodipodi:role="line"
           id="tspan57990"
           style="font-size:20px;stroke-width:0.75"
           x="166.16837"
           y="18.899153">DRA</tspan></text>
      <text
         xml:space="preserve"
         style="font-style:normal;font-weight:normal;font-size:20px;line-height:1.25;font-family:sans-serif;letter-spacing:0px;word-spacing:0px;fill:#000000;fill-opacity:1;stroke:none;stroke-width:0.75;stroke-linecap:butt;stroke-linejoin:round"
         x="446.94852"
         y="21.515116"
         id="text57992-6"><tspan
           sodipodi:role="line"
           id="tspan57990-1"
           style="font-size:20px;stroke-width:0.75"
           x="446.94852"
           y="21.515116">Max-entropy RL</tspan></text>
    </g>
    <g
       id="legend_1">
      <g
         id="text_19">
        <!-- Cost -->
        <g
           transform="translate(660.891562 167.385938)scale(0.1 -0.1)"
           id="g817">
          <defs
             id="defs807">
            <path
               id="DejaVuSans-43"
               d="M 4122 4306  L 4122 3641  Q 3803 3938 3442 4084  Q 3081 4231 2675 4231  Q 1875 4231 1450 3742  Q 1025 3253 1025 2328  Q 1025 1406 1450 917  Q 1875 428 2675 428  Q 3081 428 3442 575  Q 3803 722 4122 1019  L 4122 359  Q 3791 134 3420 21  Q 3050 -91 2638 -91  Q 1578 -91 968 557  Q 359 1206 359 2328  Q 359 3453 968 4101  Q 1578 4750 2638 4750  Q 3056 4750 3426 4639  Q 3797 4528 4122 4306  z "
               transform="scale(0.015625)" />
          </defs>
          <use
             xlink:href="#DejaVuSans-43"
             id="use809" />
          <path
             id="use811"
             d="m 100.43359,48.390625 q -7.218746,0 -11.421871,-5.640625 -4.203125,-5.640625 -4.203125,-15.453125 0,-9.8125 4.171875,-15.453125 4.1875,-5.640625 11.453121,-5.640625 7.1875,0 11.375,5.65625 4.20313,5.671875 4.20313,15.4375 0,9.71875 -4.20313,15.40625 -4.1875,5.6875 -11.375,5.6875 z m 0,7.609375 q 11.71875,0 18.40625,-7.625 6.70313,-7.609375 6.70313,-21.078125 0,-13.421875 -6.70313,-21.078125 -6.6875,-7.640625 -18.40625,-7.640625 -11.765621,0 -18.437496,7.640625 -6.65625,7.65625 -6.65625,21.078125 0,13.46875 6.65625,21.078125 Q 88.667969,56 100.43359,56 Z"
             style="stroke-width:0.015625" />
          <path
             id="use813"
             d="m 175.28711,53.078125 v -8.5 q -3.79688,1.953125 -7.90625,2.921875 -4.09375,0.984375 -8.5,0.984375 -6.6875,0 -10.03125,-2.046875 -3.34375,-2.046875 -3.34375,-6.15625 0,-3.125 2.39062,-4.90625 2.39063,-1.78125 9.625,-3.390625 l 3.07813,-0.6875 q 9.5625,-2.046875 13.59375,-5.78125 4.03125,-3.734375 4.03125,-10.421875 0,-7.625 -6.03125,-12.078125 -6.03125,-4.4375 -16.57813,-4.4375 -4.39062,0 -9.15625,0.859375 Q 141.69336,0.296875 136.42773,2 v 9.28125 q 4.98438,-2.59375 9.8125,-3.890625 4.82813,-1.28125 9.57813,-1.28125 6.34375,0 9.75,2.171875 3.42187,2.171875 3.42187,6.125 0,3.65625 -2.46875,5.609375 -2.45312,1.953125 -10.8125,3.765625 l -3.125,0.734375 q -8.34375,1.75 -12.0625,5.390625 -3.70312,3.640625 -3.70312,9.984375 0,7.71875 5.46875,11.90625 Q 147.75586,56 157.81836,56 q 4.96875,0 9.35937,-0.734375 4.40625,-0.71875 8.10938,-2.1875 z"
             style="stroke-width:0.015625" />
          <path
             id="use815"
             d="M 201.41797,70.21875 V 54.6875 h 18.5 v -6.984375 h -18.5 v -29.6875 q 0,-6.6875 1.82812,-8.59375 1.82813,-1.90625 7.45313,-1.90625 h 9.21875 V 0 h -9.21875 q -10.40625,0 -14.35938,3.875 -3.95312,3.890625 -3.95312,14.140625 v 29.6875 h -6.59375 V 54.6875 h 6.59375 v 15.53125 z"
             style="stroke-width:0.015625" />
        </g>
      </g>
      <g
         id="PathCollection_15">
        <g
           id="g822">
          <use
             xlink:href="#m67a9cd3a9b"
             x="656.874375"
             y="179.439063"
             style="fill: #79c1b8; stroke: #79c1b8; stroke-width: 2.025"
             id="use820" />
        </g>
      </g>
      <g
         id="text_20">
        <!-- 0.1 -->
        <g
           transform="translate(674.874375 182.064063)scale(0.1 -0.1)"
           id="g834">
          <defs
             id="defs826">
            <path
               id="DejaVuSans-2e"
               d="M 684 794  L 1344 794  L 1344 0  L 684 0  L 684 794  z "
               transform="scale(0.015625)" />
          </defs>
          <use
             xlink:href="#DejaVuSans-30"
             id="use828" />
          <use
             xlink:href="#DejaVuSans-2e"
             x="63.623047"
             id="use830" />
          <use
             xlink:href="#DejaVuSans-31"
             x="95.410156"
             id="use832" />
        </g>
      </g>
      <g
         id="PathCollection_16">
        <g
           id="g839">
          <use
             xlink:href="#m396209a82e"
             x="656.874375"
             y="194.117188"
             style="fill: #5babb8; stroke: #5babb8; stroke-width: 2.025"
             id="use837" />
        </g>
      </g>
      <g
         id="text_21">
        <!-- 0.25 -->
        <g
           transform="translate(674.874375 196.742188)scale(0.1 -0.1)"
           id="g850">
          <use
             xlink:href="#DejaVuSans-30"
             id="use842" />
          <use
             xlink:href="#DejaVuSans-2e"
             x="63.623047"
             id="use844" />
          <path
             id="use846"
             d="m 114.59766,8.296875 h 34.42187 V 0 h -46.28125 v 8.296875 q 5.60938,5.8125 15.29688,15.59375 9.70312,9.796875 12.1875,12.640625 4.73437,5.3125 6.60937,9 1.89063,3.6875 1.89063,7.25 0,5.8125 -4.07813,9.46875 -4.07812,3.671875 -10.625,3.671875 -4.64062,0 -9.79687,-1.609375 -5.14063,-1.609375 -11,-4.890625 v 9.96875 q 5.95312,2.390625 11.125,3.609375 5.1875,1.21875 9.48437,1.21875 11.32813,0 18.0625,-5.671875 6.73438,-5.65625 6.73438,-15.125 0,-4.5 -1.6875,-8.53125 -1.67188,-4.015625 -6.125,-9.484375 -1.21875,-1.421875 -7.76563,-8.1875 -6.53125,-6.765625 -18.45312,-18.921875 z"
             style="stroke-width:0.015625" />
          <use
             xlink:href="#DejaVuSans-35"
             x="159.033203"
             id="use848" />
        </g>
      </g>
      <g
         id="PathCollection_17">
        <g
           id="g855">
          <use
             xlink:href="#mb5648846fd"
             x="656.874375"
             y="208.795313"
             style="fill: #3c95b8; stroke: #3c95b8; stroke-width: 2.025"
             id="use853" />
        </g>
      </g>
      <g
         id="text_22">
        <!-- 0.5 -->
        <g
           transform="translate(674.874375 211.420313)scale(0.1 -0.1)"
           id="g864">
          <use
             xlink:href="#DejaVuSans-30"
             id="use858" />
          <use
             xlink:href="#DejaVuSans-2e"
             x="63.623047"
             id="use860" />
          <use
             xlink:href="#DejaVuSans-35"
             x="95.410156"
             id="use862" />
        </g>
      </g>
      <g
         id="PathCollection_18">
        <g
           id="g869">
          <use
             xlink:href="#m5cc345a50f"
             x="656.874375"
             y="223.473438"
             style="fill: #1f80b7; stroke: #1f80b7; stroke-width: 2.025"
             id="use867" />
        </g>
      </g>
      <g
         id="text_23">
        <!-- 1.0 -->
        <g
           transform="translate(674.874375 226.098438)scale(0.1 -0.1)"
           id="g878">
          <use
             xlink:href="#DejaVuSans-31"
             id="use872" />
          <use
             xlink:href="#DejaVuSans-2e"
             x="63.623047"
             id="use874" />
          <use
             xlink:href="#DejaVuSans-30"
             x="95.410156"
             id="use876" />
        </g>
      </g>
      <g
         id="PathCollection_19">
        <g
           id="g883">
          <use
             xlink:href="#macfee0750b"
             x="656.874375"
             y="238.151563"
             style="fill: #246c96; stroke: #246c96; stroke-width: 2.025"
             id="use881" />
        </g>
      </g>
      <g
         id="text_24">
        <!-- 2.0 -->
        <g
           transform="translate(674.874375 240.776563)scale(0.1 -0.1)"
           id="g892">
          <path
             id="use886"
             d="M 19.1875,8.296875 H 53.609375 V 0 H 7.328125 v 8.296875 q 5.609375,5.8125 15.296875,15.59375 9.703125,9.796875 12.1875,12.640625 4.734375,5.3125 6.609375,9 1.890625,3.6875 1.890625,7.25 0,5.8125 -4.078125,9.46875 -4.078125,3.671875 -10.625,3.671875 -4.640625,0 -9.796875,-1.609375 -5.140625,-1.609375 -11,-4.890625 v 9.96875 Q 13.765625,71.78125 18.9375,73 q 5.1875,1.21875 9.484375,1.21875 11.328125,0 18.0625,-5.671875 6.734375,-5.65625 6.734375,-15.125 0,-4.5 -1.6875,-8.53125 Q 49.859375,40.875 45.40625,35.40625 44.1875,33.984375 37.640625,27.21875 31.109375,20.453125 19.1875,8.296875 Z"
             style="stroke-width:0.015625" />
          <use
             xlink:href="#DejaVuSans-2e"
             x="63.623047"
             id="use888" />
          <use
             xlink:href="#DejaVuSans-30"
             x="95.410156"
             id="use890" />
        </g>
      </g>
      <g
         id="PathCollection_20">
        <g
           id="g897">
          <use
             xlink:href="#m5b74f1468f"
             x="656.874375"
             y="252.829688"
             style="fill: #295975; stroke: #295975; stroke-width: 2.025"
             id="use895" />
        </g>
      </g>
      <g
         id="text_25">
        <!-- 5.0 -->
        <g
           transform="translate(674.874375 255.454688)scale(0.1 -0.1)"
           id="g906">
          <use
             xlink:href="#DejaVuSans-35"
             id="use900" />
          <use
             xlink:href="#DejaVuSans-2e"
             x="63.623047"
             id="use902" />
          <use
             xlink:href="#DejaVuSans-30"
             x="95.410156"
             id="use904" />
        </g>
      </g>
      <g
         id="PathCollection_21">
        <g
           id="g911">
          <use
             xlink:href="#me8a9bf0cd3"
             x="656.874375"
             y="267.507812"
             style="fill: #2e4653; stroke: #2e4653; stroke-width: 2.025"
             id="use909" />
        </g>
      </g>
      <g
         id="text_26">
        <!-- 10.0 -->
        <g
           transform="translate(674.874375 270.132812)scale(0.1 -0.1)"
           id="g922">
          <use
             xlink:href="#DejaVuSans-31"
             id="use914" />
          <use
             xlink:href="#DejaVuSans-30"
             x="63.623047"
             id="use916" />
          <use
             xlink:href="#DejaVuSans-2e"
             x="127.246094"
             id="use918" />
          <use
             xlink:href="#DejaVuSans-30"
             x="159.033203"
             id="use920" />
        </g>
      </g>
    </g>
  </g>
  <defs
     id="defs933">
    <clipPath
       id="p611c9b1e85">
      <rect
         x="50.824644"
         y="24.72"
         width="287.912378"
         height="365.32"
         id="rect927" />
    </clipPath>
    <clipPath
       id="pf85bd4df92">
      <rect
         x="353.026222"
         y="24.72"
         width="287.912378"
         height="365.32"
         id="rect930" />
    </clipPath>
  </defs>
</svg>


#### Mattar & Daw's maze

Not entirely sure how to quantify the differences/similarities, but here they are. For both models, varying the cost parameter gives a whole range of behavior. The color scale is missing but dark purple indicates low entropy or greedy behavior and yellow indicates high entropy or random behavior.

##### Entropy

<?xml version="1.0" encoding="utf-8" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN"
  "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns:xlink="http://www.w3.org/1999/xlink" width="460.8pt" height="345.6pt" viewBox="0 0 460.8 345.6" xmlns="http://www.w3.org/2000/svg" version="1.1">
 <metadata>
  <rdf:RDF xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:cc="http://creativecommons.org/ns#" xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
   <cc:Work>
    <dc:type rdf:resource="http://purl.org/dc/dcmitype/StillImage"/>
    <dc:date>2022-05-11T14:26:16.699664</dc:date>
    <dc:format>image/svg+xml</dc:format>
    <dc:creator>
     <cc:Agent>
      <dc:title>Matplotlib v3.5.1, https://matplotlib.org/</dc:title>
     </cc:Agent>
    </dc:creator>
   </cc:Work>
  </rdf:RDF>
 </metadata>
 <defs>
  <style type="text/css">*{stroke-linejoin: round; stroke-linecap: butt}</style>
 </defs>
 <g id="figure_1">
  <g id="patch_1">
   <path d="M 0 345.6 
L 460.8 345.6 
L 460.8 0 
L 0 0 
z
" style="fill: #ffffff"/>
  </g>
  <g id="axes_1">
   <g id="patch_2">
    <path d="M 57.6 228.637091 
L 219.927273 228.637091 
L 219.927273 120.418909 
L 57.6 120.418909 
z
" style="fill: #ffffff"/>
   </g>
   <g clip-path="url(#pf5e8c66b18)">
    <image xlink:href="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAOIAAACXCAYAAAAI/4elAAAC9ElEQVR4nO3cMavVdRzH8d/xSmIQTeqgiENza9TQJA7WWKtLU9BULs1Ba0v4DFrExSUQLoqDjS4NgWBTF4UykIgL6vX0EHL5Ht7o6/UEPvzP4c1v+27+Pji7XcNebscn1tGa3/j9xVvjG3s7+I611vrz6J3xjS/vXBnf2L/0w/jGvcML4xvHxheA/yVECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIWBz8djn8xdtd3Bg+HWxv72x2cXOd79+Mv6n3H3/5PTEevLFh+Mbp28+GN/wIkKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoSAzcXNZ+OHZvfOnJ6eWLceXxs/zLuL34o3kxcRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAo7vZOT6Dnr/eH7i0dcfjW+cuvzH+MZaaz2+fW5849z3v4xv7G9vvBYX3r2IECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIeD4zwf3x0cOt8/GN94dX1jrx6+ujW9cf/LB+MZaa/317/yBYV6dFxEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIEbP45OL+dHvnt+fTCWm9vXoxvPH15YnzjzN7h+MZaaz1fm/GNT3+6Or7x8Ntvxj/k6NF74414ESFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIULAf1WUSvbH0+WvAAAAAElFTkSuQmCC" id="image93b0b3ae56" transform="scale(1 -1)translate(0 -108.72)" x="57.6" y="-119.917091" width="162.72" height="108.72"/>
   </g>
   <g id="matplotlib.axis_1">
    <g id="xtick_1">
     <g id="line2d_1">
      <defs>
       <path id="m27a1f004cc" d="M 0 0 
L 0 3.5 
" style="stroke: #000000; stroke-width: 0.8"/>
      </defs>
      <g>
       <use xlink:href="#m27a1f004cc" x="66.618182" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_1">
      <!-- 0 -->
      <g transform="translate(63.436932 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-30" d="M 2034 4250 
Q 1547 4250 1301 3770 
Q 1056 3291 1056 2328 
Q 1056 1369 1301 889 
Q 1547 409 2034 409 
Q 2525 409 2770 889 
Q 3016 1369 3016 2328 
Q 3016 3291 2770 3770 
Q 2525 4250 2034 4250 
z
M 2034 4750 
Q 2819 4750 3233 4129 
Q 3647 3509 3647 2328 
Q 3647 1150 3233 529 
Q 2819 -91 2034 -91 
Q 1250 -91 836 529 
Q 422 1150 422 2328 
Q 422 3509 836 4129 
Q 1250 4750 2034 4750 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="xtick_2">
     <g id="line2d_2">
      <g>
       <use xlink:href="#m27a1f004cc" x="102.690909" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_2">
      <!-- 2 -->
      <g transform="translate(99.509659 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-32" d="M 1228 531 
L 3431 531 
L 3431 0 
L 469 0 
L 469 531 
Q 828 903 1448 1529 
Q 2069 2156 2228 2338 
Q 2531 2678 2651 2914 
Q 2772 3150 2772 3378 
Q 2772 3750 2511 3984 
Q 2250 4219 1831 4219 
Q 1534 4219 1204 4116 
Q 875 4013 500 3803 
L 500 4441 
Q 881 4594 1212 4672 
Q 1544 4750 1819 4750 
Q 2544 4750 2975 4387 
Q 3406 4025 3406 3419 
Q 3406 3131 3298 2873 
Q 3191 2616 2906 2266 
Q 2828 2175 2409 1742 
Q 1991 1309 1228 531 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="xtick_3">
     <g id="line2d_3">
      <g>
       <use xlink:href="#m27a1f004cc" x="138.763636" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_3">
      <!-- 4 -->
      <g transform="translate(135.582386 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-34" d="M 2419 4116 
L 825 1625 
L 2419 1625 
L 2419 4116 
z
M 2253 4666 
L 3047 4666 
L 3047 1625 
L 3713 1625 
L 3713 1100 
L 3047 1100 
L 3047 0 
L 2419 0 
L 2419 1100 
L 313 1100 
L 313 1709 
L 2253 4666 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
    <g id="xtick_4">
     <g id="line2d_4">
      <g>
       <use xlink:href="#m27a1f004cc" x="174.836364" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_4">
      <!-- 6 -->
      <g transform="translate(171.655114 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-36" d="M 2113 2584 
Q 1688 2584 1439 2293 
Q 1191 2003 1191 1497 
Q 1191 994 1439 701 
Q 1688 409 2113 409 
Q 2538 409 2786 701 
Q 3034 994 3034 1497 
Q 3034 2003 2786 2293 
Q 2538 2584 2113 2584 
z
M 3366 4563 
L 3366 3988 
Q 3128 4100 2886 4159 
Q 2644 4219 2406 4219 
Q 1781 4219 1451 3797 
Q 1122 3375 1075 2522 
Q 1259 2794 1537 2939 
Q 1816 3084 2150 3084 
Q 2853 3084 3261 2657 
Q 3669 2231 3669 1497 
Q 3669 778 3244 343 
Q 2819 -91 2113 -91 
Q 1303 -91 875 529 
Q 447 1150 447 2328 
Q 447 3434 972 4092 
Q 1497 4750 2381 4750 
Q 2619 4750 2861 4703 
Q 3103 4656 3366 4563 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-36"/>
      </g>
     </g>
    </g>
    <g id="xtick_5">
     <g id="line2d_5">
      <g>
       <use xlink:href="#m27a1f004cc" x="210.909091" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_5">
      <!-- 8 -->
      <g transform="translate(207.727841 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-38" d="M 2034 2216 
Q 1584 2216 1326 1975 
Q 1069 1734 1069 1313 
Q 1069 891 1326 650 
Q 1584 409 2034 409 
Q 2484 409 2743 651 
Q 3003 894 3003 1313 
Q 3003 1734 2745 1975 
Q 2488 2216 2034 2216 
z
M 1403 2484 
Q 997 2584 770 2862 
Q 544 3141 544 3541 
Q 544 4100 942 4425 
Q 1341 4750 2034 4750 
Q 2731 4750 3128 4425 
Q 3525 4100 3525 3541 
Q 3525 3141 3298 2862 
Q 3072 2584 2669 2484 
Q 3125 2378 3379 2068 
Q 3634 1759 3634 1313 
Q 3634 634 3220 271 
Q 2806 -91 2034 -91 
Q 1263 -91 848 271 
Q 434 634 434 1313 
Q 434 1759 690 2068 
Q 947 2378 1403 2484 
z
M 1172 3481 
Q 1172 3119 1398 2916 
Q 1625 2713 2034 2713 
Q 2441 2713 2670 2916 
Q 2900 3119 2900 3481 
Q 2900 3844 2670 4047 
Q 2441 4250 2034 4250 
Q 1625 4250 1398 4047 
Q 1172 3844 1172 3481 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-38"/>
      </g>
     </g>
    </g>
   </g>
   <g id="matplotlib.axis_2">
    <g id="ytick_1">
     <g id="line2d_6">
      <defs>
       <path id="m582d1633ad" d="M 0 0 
L -3.5 0 
" style="stroke: #000000; stroke-width: 0.8"/>
      </defs>
      <g>
       <use xlink:href="#m582d1633ad" x="57.6" y="129.437091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_6">
      <!-- 0 -->
      <g transform="translate(44.2375 133.23631)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="ytick_2">
     <g id="line2d_7">
      <g>
       <use xlink:href="#m582d1633ad" x="57.6" y="165.509818" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_7">
      <!-- 2 -->
      <g transform="translate(44.2375 169.309037)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="ytick_3">
     <g id="line2d_8">
      <g>
       <use xlink:href="#m582d1633ad" x="57.6" y="201.582545" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_8">
      <!-- 4 -->
      <g transform="translate(44.2375 205.381764)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
   </g>
   <g id="patch_3">
    <path d="M 57.6 228.637091 
L 57.6 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_4">
    <path d="M 219.927273 228.637091 
L 219.927273 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_5">
    <path d="M 57.6 228.637091 
L 219.927273 228.637091 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_6">
    <path d="M 57.6 120.418909 
L 219.927273 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="text_9">
    <!-- DRA -->
    <g transform="translate(126.110199 114.418909)scale(0.12 -0.12)">
     <defs>
      <path id="DejaVuSans-44" d="M 1259 4147 
L 1259 519 
L 2022 519 
Q 2988 519 3436 956 
Q 3884 1394 3884 2338 
Q 3884 3275 3436 3711 
Q 2988 4147 2022 4147 
L 1259 4147 
z
M 628 4666 
L 1925 4666 
Q 3281 4666 3915 4102 
Q 4550 3538 4550 2338 
Q 4550 1131 3912 565 
Q 3275 0 1925 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-52" d="M 2841 2188 
Q 3044 2119 3236 1894 
Q 3428 1669 3622 1275 
L 4263 0 
L 3584 0 
L 2988 1197 
Q 2756 1666 2539 1819 
Q 2322 1972 1947 1972 
L 1259 1972 
L 1259 0 
L 628 0 
L 628 4666 
L 2053 4666 
Q 2853 4666 3247 4331 
Q 3641 3997 3641 3322 
Q 3641 2881 3436 2590 
Q 3231 2300 2841 2188 
z
M 1259 4147 
L 1259 2491 
L 2053 2491 
Q 2509 2491 2742 2702 
Q 2975 2913 2975 3322 
Q 2975 3731 2742 3939 
Q 2509 4147 2053 4147 
L 1259 4147 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-41" d="M 2188 4044 
L 1331 1722 
L 3047 1722 
L 2188 4044 
z
M 1831 4666 
L 2547 4666 
L 4325 0 
L 3669 0 
L 3244 1197 
L 1141 1197 
L 716 0 
L 50 0 
L 1831 4666 
z
" transform="scale(0.015625)"/>
     </defs>
     <use xlink:href="#DejaVuSans-44"/>
     <use xlink:href="#DejaVuSans-52" x="77.001953"/>
     <use xlink:href="#DejaVuSans-41" x="142.484375"/>
    </g>
   </g>
  </g>
  <g id="axes_2">
   <g id="patch_7">
    <path d="M 252.392727 228.637091 
L 414.72 228.637091 
L 414.72 120.418909 
L 252.392727 120.418909 
z
" style="fill: #ffffff"/>
   </g>
   <g clip-path="url(#p6c2ddcf71c)">
    <image xlink:href="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAOIAAACXCAYAAAAI/4elAAADLUlEQVR4nO3cvYqeVRhG4f2F8W8khUiUyEhQIlhoIRYRrIQpPABLC7VVCGJl4wEIFtqLGKzEgJVBUUEQAwEhhVgIQkAlBoeACpkUmYyFB6DNM6ziuk7ghhfWu7tnc/XXk4dr2Nc3dqYn1mfXnxzfuPTFE+MbF156e3xjrbW+2X90fOO9n54b37j49EfjG9/evHt849j4AvCfhAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIEbL2z9+z4yPmvnhnfONi+Pb7x+Pnr4xun3/p9Mz6y1jr14dnxw9KPvXJ5emKdefXs+MbJ9y+Pb3gRIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQsDm+ysPjx+afffa7vTEOnfmg/HDvM8/9Nr4t7p17Y/piX/dPjiaHf4XLyIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIEbG0fuzU+snvfj+Mb58YX1trbfeQIVo5iY627/p6/9H3Pp5fGN748/GT8wvvu5oXxC+9eRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBCwdWrrzvGRE9u/jG8chb2nxu/MrsOt+Y211nrw4vw/+N7jx8c31l/zE0fBiwgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBECNn/+tjN+0fbzGw9MT6wf9nfGN67s3z++8eKJ78Y31lrr5uEd4xuvf/zy+MbPb76xmd44uHp6vBEvIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAj4BzNAUlXe0wh7AAAAAElFTkSuQmCC" id="imagec391479499" transform="scale(1 -1)translate(0 -108.72)" x="252.392727" y="-119.917091" width="162.72" height="108.72"/>
   </g>
   <g id="matplotlib.axis_3">
    <g id="xtick_6">
     <g id="line2d_9">
      <g>
       <use xlink:href="#m27a1f004cc" x="261.410909" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_10">
      <!-- 0 -->
      <g transform="translate(258.229659 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="xtick_7">
     <g id="line2d_10">
      <g>
       <use xlink:href="#m27a1f004cc" x="297.483636" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_11">
      <!-- 2 -->
      <g transform="translate(294.302386 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="xtick_8">
     <g id="line2d_11">
      <g>
       <use xlink:href="#m27a1f004cc" x="333.556364" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_12">
      <!-- 4 -->
      <g transform="translate(330.375114 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
    <g id="xtick_9">
     <g id="line2d_12">
      <g>
       <use xlink:href="#m27a1f004cc" x="369.629091" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_13">
      <!-- 6 -->
      <g transform="translate(366.447841 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-36"/>
      </g>
     </g>
    </g>
    <g id="xtick_10">
     <g id="line2d_13">
      <g>
       <use xlink:href="#m27a1f004cc" x="405.701818" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_14">
      <!-- 8 -->
      <g transform="translate(402.520568 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-38"/>
      </g>
     </g>
    </g>
   </g>
   <g id="matplotlib.axis_4">
    <g id="ytick_4">
     <g id="line2d_14">
      <g>
       <use xlink:href="#m582d1633ad" x="252.392727" y="129.437091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_15">
      <!-- 0 -->
      <g transform="translate(239.030227 133.23631)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="ytick_5">
     <g id="line2d_15">
      <g>
       <use xlink:href="#m582d1633ad" x="252.392727" y="165.509818" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_16">
      <!-- 2 -->
      <g transform="translate(239.030227 169.309037)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="ytick_6">
     <g id="line2d_16">
      <g>
       <use xlink:href="#m582d1633ad" x="252.392727" y="201.582545" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_17">
      <!-- 4 -->
      <g transform="translate(239.030227 205.381764)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
   </g>
   <g id="patch_8">
    <path d="M 252.392727 228.637091 
L 252.392727 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_9">
    <path d="M 414.72 228.637091 
L 414.72 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_10">
    <path d="M 252.392727 228.637091 
L 414.72 228.637091 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_11">
    <path d="M 252.392727 120.418909 
L 414.72 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="text_18">
    <!-- MaxEntRL -->
    <g transform="translate(303.693239 114.418909)scale(0.12 -0.12)">
     <defs>
      <path id="DejaVuSans-4d" d="M 628 4666 
L 1569 4666 
L 2759 1491 
L 3956 4666 
L 4897 4666 
L 4897 0 
L 4281 0 
L 4281 4097 
L 3078 897 
L 2444 897 
L 1241 4097 
L 1241 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-61" d="M 2194 1759 
Q 1497 1759 1228 1600 
Q 959 1441 959 1056 
Q 959 750 1161 570 
Q 1363 391 1709 391 
Q 2188 391 2477 730 
Q 2766 1069 2766 1631 
L 2766 1759 
L 2194 1759 
z
M 3341 1997 
L 3341 0 
L 2766 0 
L 2766 531 
Q 2569 213 2275 61 
Q 1981 -91 1556 -91 
Q 1019 -91 701 211 
Q 384 513 384 1019 
Q 384 1609 779 1909 
Q 1175 2209 1959 2209 
L 2766 2209 
L 2766 2266 
Q 2766 2663 2505 2880 
Q 2244 3097 1772 3097 
Q 1472 3097 1187 3025 
Q 903 2953 641 2809 
L 641 3341 
Q 956 3463 1253 3523 
Q 1550 3584 1831 3584 
Q 2591 3584 2966 3190 
Q 3341 2797 3341 1997 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-78" d="M 3513 3500 
L 2247 1797 
L 3578 0 
L 2900 0 
L 1881 1375 
L 863 0 
L 184 0 
L 1544 1831 
L 300 3500 
L 978 3500 
L 1906 2253 
L 2834 3500 
L 3513 3500 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-45" d="M 628 4666 
L 3578 4666 
L 3578 4134 
L 1259 4134 
L 1259 2753 
L 3481 2753 
L 3481 2222 
L 1259 2222 
L 1259 531 
L 3634 531 
L 3634 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-6e" d="M 3513 2113 
L 3513 0 
L 2938 0 
L 2938 2094 
Q 2938 2591 2744 2837 
Q 2550 3084 2163 3084 
Q 1697 3084 1428 2787 
Q 1159 2491 1159 1978 
L 1159 0 
L 581 0 
L 581 3500 
L 1159 3500 
L 1159 2956 
Q 1366 3272 1645 3428 
Q 1925 3584 2291 3584 
Q 2894 3584 3203 3211 
Q 3513 2838 3513 2113 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-74" d="M 1172 4494 
L 1172 3500 
L 2356 3500 
L 2356 3053 
L 1172 3053 
L 1172 1153 
Q 1172 725 1289 603 
Q 1406 481 1766 481 
L 2356 481 
L 2356 0 
L 1766 0 
Q 1100 0 847 248 
Q 594 497 594 1153 
L 594 3053 
L 172 3053 
L 172 3500 
L 594 3500 
L 594 4494 
L 1172 4494 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-4c" d="M 628 4666 
L 1259 4666 
L 1259 531 
L 3531 531 
L 3531 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
     </defs>
     <use xlink:href="#DejaVuSans-4d"/>
     <use xlink:href="#DejaVuSans-61" x="86.279297"/>
     <use xlink:href="#DejaVuSans-78" x="147.558594"/>
     <use xlink:href="#DejaVuSans-45" x="206.738281"/>
     <use xlink:href="#DejaVuSans-6e" x="269.921875"/>
     <use xlink:href="#DejaVuSans-74" x="333.300781"/>
     <use xlink:href="#DejaVuSans-52" x="372.509766"/>
     <use xlink:href="#DejaVuSans-4c" x="441.992188"/>
    </g>
   </g>
  </g>
 </g>
 <defs>
  <clipPath id="pf5e8c66b18">
   <rect x="57.6" y="120.418909" width="162.327273" height="108.218182"/>
  </clipPath>
  <clipPath id="p6c2ddcf71c">
   <rect x="252.392727" y="120.418909" width="162.327273" height="108.218182"/>
  </clipPath>
 </defs>
</svg>
<?xml version="1.0" encoding="utf-8" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN"
  "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns:xlink="http://www.w3.org/1999/xlink" width="460.8pt" height="345.6pt" viewBox="0 0 460.8 345.6" xmlns="http://www.w3.org/2000/svg" version="1.1">
 <metadata>
  <rdf:RDF xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:cc="http://creativecommons.org/ns#" xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
   <cc:Work>
    <dc:type rdf:resource="http://purl.org/dc/dcmitype/StillImage"/>
    <dc:date>2022-05-11T14:23:01.428424</dc:date>
    <dc:format>image/svg+xml</dc:format>
    <dc:creator>
     <cc:Agent>
      <dc:title>Matplotlib v3.5.1, https://matplotlib.org/</dc:title>
     </cc:Agent>
    </dc:creator>
   </cc:Work>
  </rdf:RDF>
 </metadata>
 <defs>
  <style type="text/css">*{stroke-linejoin: round; stroke-linecap: butt}</style>
 </defs>
 <g id="figure_1">
  <g id="patch_1">
   <path d="M 0 345.6 
L 460.8 345.6 
L 460.8 0 
L 0 0 
z
" style="fill: #ffffff"/>
  </g>
  <g id="axes_1">
   <g id="patch_2">
    <path d="M 57.6 228.637091 
L 219.927273 228.637091 
L 219.927273 120.418909 
L 57.6 120.418909 
z
" style="fill: #ffffff"/>
   </g>
   <g clip-path="url(#pf569ac5824)">
    <image xlink:href="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAOIAAACXCAYAAAAI/4elAAAC30lEQVR4nO3cLevVdxjH8d9RsXpTFBEUUUFRMIjB6gw2w5JpeyQrPoH1lRWDuLZgEKOIIAgqapiwOZXZXPgbvDkGH4DpOrz583o9gQ+/c3jzbddq682R9TJs68vH6Ynl9/9Pj2/8vOfZ+Mb1dxfHN5ZlWa7ufTi+8frTvvGNNx/nN359cGl8Y8f4AvBdQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBECVpfP/zJ+YHj98On0xLZxZ31rtYmdH1Y/jv/vy2oDn7Ke/4xN8CJCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEgNVGDs1uwCYO826X34oeLyIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIE7NrIyMED8yNv5yde3jg3vnHs2qPxje1ku1x49yJCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEgF1//Ht/fOTVpy/jG7ePjE8sV04+Hd94Mb7wzc5TJ8Y33p/ZP76x3Lw1v7EBXkQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQsHry96H19Mi9D8emJ5bHW4fHN/58fnZ84/hP80eMl2VZfvvr7vjGf593j29cOPrPanrj89vj4414ESFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIULAVzGMR8SMk9BfAAAAAElFTkSuQmCC" id="imaged5576c7537" transform="scale(1 -1)translate(0 -108.72)" x="57.6" y="-119.917091" width="162.72" height="108.72"/>
   </g>
   <g id="matplotlib.axis_1">
    <g id="xtick_1">
     <g id="line2d_1">
      <defs>
       <path id="m2737b24559" d="M 0 0 
L 0 3.5 
" style="stroke: #000000; stroke-width: 0.8"/>
      </defs>
      <g>
       <use xlink:href="#m2737b24559" x="66.618182" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_1">
      <!-- 0 -->
      <g transform="translate(63.436932 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-30" d="M 2034 4250 
Q 1547 4250 1301 3770 
Q 1056 3291 1056 2328 
Q 1056 1369 1301 889 
Q 1547 409 2034 409 
Q 2525 409 2770 889 
Q 3016 1369 3016 2328 
Q 3016 3291 2770 3770 
Q 2525 4250 2034 4250 
z
M 2034 4750 
Q 2819 4750 3233 4129 
Q 3647 3509 3647 2328 
Q 3647 1150 3233 529 
Q 2819 -91 2034 -91 
Q 1250 -91 836 529 
Q 422 1150 422 2328 
Q 422 3509 836 4129 
Q 1250 4750 2034 4750 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="xtick_2">
     <g id="line2d_2">
      <g>
       <use xlink:href="#m2737b24559" x="102.690909" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_2">
      <!-- 2 -->
      <g transform="translate(99.509659 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-32" d="M 1228 531 
L 3431 531 
L 3431 0 
L 469 0 
L 469 531 
Q 828 903 1448 1529 
Q 2069 2156 2228 2338 
Q 2531 2678 2651 2914 
Q 2772 3150 2772 3378 
Q 2772 3750 2511 3984 
Q 2250 4219 1831 4219 
Q 1534 4219 1204 4116 
Q 875 4013 500 3803 
L 500 4441 
Q 881 4594 1212 4672 
Q 1544 4750 1819 4750 
Q 2544 4750 2975 4387 
Q 3406 4025 3406 3419 
Q 3406 3131 3298 2873 
Q 3191 2616 2906 2266 
Q 2828 2175 2409 1742 
Q 1991 1309 1228 531 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="xtick_3">
     <g id="line2d_3">
      <g>
       <use xlink:href="#m2737b24559" x="138.763636" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_3">
      <!-- 4 -->
      <g transform="translate(135.582386 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-34" d="M 2419 4116 
L 825 1625 
L 2419 1625 
L 2419 4116 
z
M 2253 4666 
L 3047 4666 
L 3047 1625 
L 3713 1625 
L 3713 1100 
L 3047 1100 
L 3047 0 
L 2419 0 
L 2419 1100 
L 313 1100 
L 313 1709 
L 2253 4666 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
    <g id="xtick_4">
     <g id="line2d_4">
      <g>
       <use xlink:href="#m2737b24559" x="174.836364" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_4">
      <!-- 6 -->
      <g transform="translate(171.655114 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-36" d="M 2113 2584 
Q 1688 2584 1439 2293 
Q 1191 2003 1191 1497 
Q 1191 994 1439 701 
Q 1688 409 2113 409 
Q 2538 409 2786 701 
Q 3034 994 3034 1497 
Q 3034 2003 2786 2293 
Q 2538 2584 2113 2584 
z
M 3366 4563 
L 3366 3988 
Q 3128 4100 2886 4159 
Q 2644 4219 2406 4219 
Q 1781 4219 1451 3797 
Q 1122 3375 1075 2522 
Q 1259 2794 1537 2939 
Q 1816 3084 2150 3084 
Q 2853 3084 3261 2657 
Q 3669 2231 3669 1497 
Q 3669 778 3244 343 
Q 2819 -91 2113 -91 
Q 1303 -91 875 529 
Q 447 1150 447 2328 
Q 447 3434 972 4092 
Q 1497 4750 2381 4750 
Q 2619 4750 2861 4703 
Q 3103 4656 3366 4563 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-36"/>
      </g>
     </g>
    </g>
    <g id="xtick_5">
     <g id="line2d_5">
      <g>
       <use xlink:href="#m2737b24559" x="210.909091" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_5">
      <!-- 8 -->
      <g transform="translate(207.727841 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-38" d="M 2034 2216 
Q 1584 2216 1326 1975 
Q 1069 1734 1069 1313 
Q 1069 891 1326 650 
Q 1584 409 2034 409 
Q 2484 409 2743 651 
Q 3003 894 3003 1313 
Q 3003 1734 2745 1975 
Q 2488 2216 2034 2216 
z
M 1403 2484 
Q 997 2584 770 2862 
Q 544 3141 544 3541 
Q 544 4100 942 4425 
Q 1341 4750 2034 4750 
Q 2731 4750 3128 4425 
Q 3525 4100 3525 3541 
Q 3525 3141 3298 2862 
Q 3072 2584 2669 2484 
Q 3125 2378 3379 2068 
Q 3634 1759 3634 1313 
Q 3634 634 3220 271 
Q 2806 -91 2034 -91 
Q 1263 -91 848 271 
Q 434 634 434 1313 
Q 434 1759 690 2068 
Q 947 2378 1403 2484 
z
M 1172 3481 
Q 1172 3119 1398 2916 
Q 1625 2713 2034 2713 
Q 2441 2713 2670 2916 
Q 2900 3119 2900 3481 
Q 2900 3844 2670 4047 
Q 2441 4250 2034 4250 
Q 1625 4250 1398 4047 
Q 1172 3844 1172 3481 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-38"/>
      </g>
     </g>
    </g>
   </g>
   <g id="matplotlib.axis_2">
    <g id="ytick_1">
     <g id="line2d_6">
      <defs>
       <path id="m474e3b04fe" d="M 0 0 
L -3.5 0 
" style="stroke: #000000; stroke-width: 0.8"/>
      </defs>
      <g>
       <use xlink:href="#m474e3b04fe" x="57.6" y="129.437091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_6">
      <!-- 0 -->
      <g transform="translate(44.2375 133.23631)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="ytick_2">
     <g id="line2d_7">
      <g>
       <use xlink:href="#m474e3b04fe" x="57.6" y="165.509818" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_7">
      <!-- 2 -->
      <g transform="translate(44.2375 169.309037)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="ytick_3">
     <g id="line2d_8">
      <g>
       <use xlink:href="#m474e3b04fe" x="57.6" y="201.582545" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_8">
      <!-- 4 -->
      <g transform="translate(44.2375 205.381764)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
   </g>
   <g id="patch_3">
    <path d="M 57.6 228.637091 
L 57.6 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_4">
    <path d="M 219.927273 228.637091 
L 219.927273 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_5">
    <path d="M 57.6 228.637091 
L 219.927273 228.637091 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_6">
    <path d="M 57.6 120.418909 
L 219.927273 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="text_9">
    <!-- DRA -->
    <g transform="translate(126.110199 114.418909)scale(0.12 -0.12)">
     <defs>
      <path id="DejaVuSans-44" d="M 1259 4147 
L 1259 519 
L 2022 519 
Q 2988 519 3436 956 
Q 3884 1394 3884 2338 
Q 3884 3275 3436 3711 
Q 2988 4147 2022 4147 
L 1259 4147 
z
M 628 4666 
L 1925 4666 
Q 3281 4666 3915 4102 
Q 4550 3538 4550 2338 
Q 4550 1131 3912 565 
Q 3275 0 1925 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-52" d="M 2841 2188 
Q 3044 2119 3236 1894 
Q 3428 1669 3622 1275 
L 4263 0 
L 3584 0 
L 2988 1197 
Q 2756 1666 2539 1819 
Q 2322 1972 1947 1972 
L 1259 1972 
L 1259 0 
L 628 0 
L 628 4666 
L 2053 4666 
Q 2853 4666 3247 4331 
Q 3641 3997 3641 3322 
Q 3641 2881 3436 2590 
Q 3231 2300 2841 2188 
z
M 1259 4147 
L 1259 2491 
L 2053 2491 
Q 2509 2491 2742 2702 
Q 2975 2913 2975 3322 
Q 2975 3731 2742 3939 
Q 2509 4147 2053 4147 
L 1259 4147 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-41" d="M 2188 4044 
L 1331 1722 
L 3047 1722 
L 2188 4044 
z
M 1831 4666 
L 2547 4666 
L 4325 0 
L 3669 0 
L 3244 1197 
L 1141 1197 
L 716 0 
L 50 0 
L 1831 4666 
z
" transform="scale(0.015625)"/>
     </defs>
     <use xlink:href="#DejaVuSans-44"/>
     <use xlink:href="#DejaVuSans-52" x="77.001953"/>
     <use xlink:href="#DejaVuSans-41" x="142.484375"/>
    </g>
   </g>
  </g>
  <g id="axes_2">
   <g id="patch_7">
    <path d="M 252.392727 228.637091 
L 414.72 228.637091 
L 414.72 120.418909 
L 252.392727 120.418909 
z
" style="fill: #ffffff"/>
   </g>
   <g clip-path="url(#p30af2ac36d)">
    <image xlink:href="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAOIAAACXCAYAAAAI/4elAAACsElEQVR4nO3coYpVURiG4XVkRgx2ERTEZBbEYhHsFoMY1DpBhClGL2CSN2CzeQPKJEW0eBAEmSQm0xgMThE8XoKWtXk5PM8NfLA3L6v9q9tv9zZjsvvn38+eGJd3j6dvPDjYn77x++z0iTHGGBefradvPP3ybvrGk8d70zeOH/6avnFq+gLwT0KEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAla3VnemHxjm/x1uXq6W2PHfW7yIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIWBrDgwvcZh3W74VPV5ECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECNhZZGU1/Qj3GAvc4F7tnl5gY5lf8ufkZJGd2bblwrsXEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgTsHHz7MH3k+Y8b0zcOr06fGPc+f52+8eLKhekb9HgRIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQsDOEiNHP89N33i0vruZvXHtzJvZE2P98dL0jTHGOLq+wMjr+f993Jw/8er7p+kbXkQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQ8BfxHzZPyHmffwAAAABJRU5ErkJggg==" id="image49d3da34de" transform="scale(1 -1)translate(0 -108.72)" x="252.392727" y="-119.917091" width="162.72" height="108.72"/>
   </g>
   <g id="matplotlib.axis_3">
    <g id="xtick_6">
     <g id="line2d_9">
      <g>
       <use xlink:href="#m2737b24559" x="261.410909" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_10">
      <!-- 0 -->
      <g transform="translate(258.229659 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="xtick_7">
     <g id="line2d_10">
      <g>
       <use xlink:href="#m2737b24559" x="297.483636" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_11">
      <!-- 2 -->
      <g transform="translate(294.302386 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="xtick_8">
     <g id="line2d_11">
      <g>
       <use xlink:href="#m2737b24559" x="333.556364" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_12">
      <!-- 4 -->
      <g transform="translate(330.375114 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
    <g id="xtick_9">
     <g id="line2d_12">
      <g>
       <use xlink:href="#m2737b24559" x="369.629091" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_13">
      <!-- 6 -->
      <g transform="translate(366.447841 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-36"/>
      </g>
     </g>
    </g>
    <g id="xtick_10">
     <g id="line2d_13">
      <g>
       <use xlink:href="#m2737b24559" x="405.701818" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_14">
      <!-- 8 -->
      <g transform="translate(402.520568 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-38"/>
      </g>
     </g>
    </g>
   </g>
   <g id="matplotlib.axis_4">
    <g id="ytick_4">
     <g id="line2d_14">
      <g>
       <use xlink:href="#m474e3b04fe" x="252.392727" y="129.437091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_15">
      <!-- 0 -->
      <g transform="translate(239.030227 133.23631)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="ytick_5">
     <g id="line2d_15">
      <g>
       <use xlink:href="#m474e3b04fe" x="252.392727" y="165.509818" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_16">
      <!-- 2 -->
      <g transform="translate(239.030227 169.309037)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="ytick_6">
     <g id="line2d_16">
      <g>
       <use xlink:href="#m474e3b04fe" x="252.392727" y="201.582545" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_17">
      <!-- 4 -->
      <g transform="translate(239.030227 205.381764)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
   </g>
   <g id="patch_8">
    <path d="M 252.392727 228.637091 
L 252.392727 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_9">
    <path d="M 414.72 228.637091 
L 414.72 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_10">
    <path d="M 252.392727 228.637091 
L 414.72 228.637091 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_11">
    <path d="M 252.392727 120.418909 
L 414.72 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="text_18">
    <!-- MaxEntRL -->
    <g transform="translate(303.693239 114.418909)scale(0.12 -0.12)">
     <defs>
      <path id="DejaVuSans-4d" d="M 628 4666 
L 1569 4666 
L 2759 1491 
L 3956 4666 
L 4897 4666 
L 4897 0 
L 4281 0 
L 4281 4097 
L 3078 897 
L 2444 897 
L 1241 4097 
L 1241 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-61" d="M 2194 1759 
Q 1497 1759 1228 1600 
Q 959 1441 959 1056 
Q 959 750 1161 570 
Q 1363 391 1709 391 
Q 2188 391 2477 730 
Q 2766 1069 2766 1631 
L 2766 1759 
L 2194 1759 
z
M 3341 1997 
L 3341 0 
L 2766 0 
L 2766 531 
Q 2569 213 2275 61 
Q 1981 -91 1556 -91 
Q 1019 -91 701 211 
Q 384 513 384 1019 
Q 384 1609 779 1909 
Q 1175 2209 1959 2209 
L 2766 2209 
L 2766 2266 
Q 2766 2663 2505 2880 
Q 2244 3097 1772 3097 
Q 1472 3097 1187 3025 
Q 903 2953 641 2809 
L 641 3341 
Q 956 3463 1253 3523 
Q 1550 3584 1831 3584 
Q 2591 3584 2966 3190 
Q 3341 2797 3341 1997 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-78" d="M 3513 3500 
L 2247 1797 
L 3578 0 
L 2900 0 
L 1881 1375 
L 863 0 
L 184 0 
L 1544 1831 
L 300 3500 
L 978 3500 
L 1906 2253 
L 2834 3500 
L 3513 3500 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-45" d="M 628 4666 
L 3578 4666 
L 3578 4134 
L 1259 4134 
L 1259 2753 
L 3481 2753 
L 3481 2222 
L 1259 2222 
L 1259 531 
L 3634 531 
L 3634 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-6e" d="M 3513 2113 
L 3513 0 
L 2938 0 
L 2938 2094 
Q 2938 2591 2744 2837 
Q 2550 3084 2163 3084 
Q 1697 3084 1428 2787 
Q 1159 2491 1159 1978 
L 1159 0 
L 581 0 
L 581 3500 
L 1159 3500 
L 1159 2956 
Q 1366 3272 1645 3428 
Q 1925 3584 2291 3584 
Q 2894 3584 3203 3211 
Q 3513 2838 3513 2113 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-74" d="M 1172 4494 
L 1172 3500 
L 2356 3500 
L 2356 3053 
L 1172 3053 
L 1172 1153 
Q 1172 725 1289 603 
Q 1406 481 1766 481 
L 2356 481 
L 2356 0 
L 1766 0 
Q 1100 0 847 248 
Q 594 497 594 1153 
L 594 3053 
L 172 3053 
L 172 3500 
L 594 3500 
L 594 4494 
L 1172 4494 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-4c" d="M 628 4666 
L 1259 4666 
L 1259 531 
L 3531 531 
L 3531 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
     </defs>
     <use xlink:href="#DejaVuSans-4d"/>
     <use xlink:href="#DejaVuSans-61" x="86.279297"/>
     <use xlink:href="#DejaVuSans-78" x="147.558594"/>
     <use xlink:href="#DejaVuSans-45" x="206.738281"/>
     <use xlink:href="#DejaVuSans-6e" x="269.921875"/>
     <use xlink:href="#DejaVuSans-74" x="333.300781"/>
     <use xlink:href="#DejaVuSans-52" x="372.509766"/>
     <use xlink:href="#DejaVuSans-4c" x="441.992188"/>
    </g>
   </g>
  </g>
 </g>
 <defs>
  <clipPath id="pf569ac5824">
   <rect x="57.6" y="120.418909" width="162.327273" height="108.218182"/>
  </clipPath>
  <clipPath id="p30af2ac36d">
   <rect x="252.392727" y="120.418909" width="162.327273" height="108.218182"/>
  </clipPath>
 </defs>
</svg>
<?xml version="1.0" encoding="utf-8" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN"
  "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns:xlink="http://www.w3.org/1999/xlink" width="460.8pt" height="345.6pt" viewBox="0 0 460.8 345.6" xmlns="http://www.w3.org/2000/svg" version="1.1">
 <metadata>
  <rdf:RDF xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:cc="http://creativecommons.org/ns#" xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
   <cc:Work>
    <dc:type rdf:resource="http://purl.org/dc/dcmitype/StillImage"/>
    <dc:date>2022-05-11T14:30:29.240965</dc:date>
    <dc:format>image/svg+xml</dc:format>
    <dc:creator>
     <cc:Agent>
      <dc:title>Matplotlib v3.5.1, https://matplotlib.org/</dc:title>
     </cc:Agent>
    </dc:creator>
   </cc:Work>
  </rdf:RDF>
 </metadata>
 <defs>
  <style type="text/css">*{stroke-linejoin: round; stroke-linecap: butt}</style>
 </defs>
 <g id="figure_1">
  <g id="patch_1">
   <path d="M 0 345.6 
L 460.8 345.6 
L 460.8 0 
L 0 0 
z
" style="fill: #ffffff"/>
  </g>
  <g id="axes_1">
   <g id="patch_2">
    <path d="M 57.6 228.637091 
L 219.927273 228.637091 
L 219.927273 120.418909 
L 57.6 120.418909 
z
" style="fill: #ffffff"/>
   </g>
   <g clip-path="url(#p849d3653ab)">
    <image xlink:href="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAOIAAACXCAYAAAAI/4elAAADQ0lEQVR4nO3cP4qddRiG4d/JRBOYkKRJIYqCisGIhaW9prCwEgQRiYULEAsbsRIU/FNkA5YuQBB0B3YiSIo4qZJoCGIyDCM6mRkLF2D1yg1e1wYeznfOfb7u3fx1+8njNWzv+M/piXXn8Gh845u958Y3Xt6+Nr6x1lr3jk6NbxwcnxzfePH0/G/r9oP5jRPjC8C/EiIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQsLn8wofjB4aPr+1MT6wT58+Nb/z87lPjGzvvv7cZH1lrXfn+yvj3fv3jS9MT6+6bf4xvHO6cGd/wRoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBmyeufjZ+aPbiBz9NT6zvdr8cP8x7efut8Wd1tL8/PbHWWmvr7NnxjcPd3fGNrQsXxjeO7t0f3/BGhAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChICTx9sPxkduvfP8+Mb6fH7i7R/mL5Z/8sUb4xtrrXX+xsH4xukbv41vfHv90/EL7y9tvT5+4d0bEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgRsDn95evx46s7B3vTEeubxX8cPzd65+cj4s/pq99L0xFprratfvzK+cebZ38c3fnz1o/kDw5vXHBiG/wMhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAGb+7ceGz+eun98OD2xTm3m/1P2j+Y/x8H4wj/uHj48vnHxoaPxjXOP3hw/MPxfHOH2RoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBfwMTYmd1yaVzqgAAAABJRU5ErkJggg==" id="image2c995a285c" transform="scale(1 -1)translate(0 -108.72)" x="57.6" y="-119.917091" width="162.72" height="108.72"/>
   </g>
   <g id="matplotlib.axis_1">
    <g id="xtick_1">
     <g id="line2d_1">
      <defs>
       <path id="m4fc69e083e" d="M 0 0 
L 0 3.5 
" style="stroke: #000000; stroke-width: 0.8"/>
      </defs>
      <g>
       <use xlink:href="#m4fc69e083e" x="66.618182" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_1">
      <!-- 0 -->
      <g transform="translate(63.436932 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-30" d="M 2034 4250 
Q 1547 4250 1301 3770 
Q 1056 3291 1056 2328 
Q 1056 1369 1301 889 
Q 1547 409 2034 409 
Q 2525 409 2770 889 
Q 3016 1369 3016 2328 
Q 3016 3291 2770 3770 
Q 2525 4250 2034 4250 
z
M 2034 4750 
Q 2819 4750 3233 4129 
Q 3647 3509 3647 2328 
Q 3647 1150 3233 529 
Q 2819 -91 2034 -91 
Q 1250 -91 836 529 
Q 422 1150 422 2328 
Q 422 3509 836 4129 
Q 1250 4750 2034 4750 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="xtick_2">
     <g id="line2d_2">
      <g>
       <use xlink:href="#m4fc69e083e" x="102.690909" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_2">
      <!-- 2 -->
      <g transform="translate(99.509659 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-32" d="M 1228 531 
L 3431 531 
L 3431 0 
L 469 0 
L 469 531 
Q 828 903 1448 1529 
Q 2069 2156 2228 2338 
Q 2531 2678 2651 2914 
Q 2772 3150 2772 3378 
Q 2772 3750 2511 3984 
Q 2250 4219 1831 4219 
Q 1534 4219 1204 4116 
Q 875 4013 500 3803 
L 500 4441 
Q 881 4594 1212 4672 
Q 1544 4750 1819 4750 
Q 2544 4750 2975 4387 
Q 3406 4025 3406 3419 
Q 3406 3131 3298 2873 
Q 3191 2616 2906 2266 
Q 2828 2175 2409 1742 
Q 1991 1309 1228 531 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="xtick_3">
     <g id="line2d_3">
      <g>
       <use xlink:href="#m4fc69e083e" x="138.763636" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_3">
      <!-- 4 -->
      <g transform="translate(135.582386 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-34" d="M 2419 4116 
L 825 1625 
L 2419 1625 
L 2419 4116 
z
M 2253 4666 
L 3047 4666 
L 3047 1625 
L 3713 1625 
L 3713 1100 
L 3047 1100 
L 3047 0 
L 2419 0 
L 2419 1100 
L 313 1100 
L 313 1709 
L 2253 4666 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
    <g id="xtick_4">
     <g id="line2d_4">
      <g>
       <use xlink:href="#m4fc69e083e" x="174.836364" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_4">
      <!-- 6 -->
      <g transform="translate(171.655114 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-36" d="M 2113 2584 
Q 1688 2584 1439 2293 
Q 1191 2003 1191 1497 
Q 1191 994 1439 701 
Q 1688 409 2113 409 
Q 2538 409 2786 701 
Q 3034 994 3034 1497 
Q 3034 2003 2786 2293 
Q 2538 2584 2113 2584 
z
M 3366 4563 
L 3366 3988 
Q 3128 4100 2886 4159 
Q 2644 4219 2406 4219 
Q 1781 4219 1451 3797 
Q 1122 3375 1075 2522 
Q 1259 2794 1537 2939 
Q 1816 3084 2150 3084 
Q 2853 3084 3261 2657 
Q 3669 2231 3669 1497 
Q 3669 778 3244 343 
Q 2819 -91 2113 -91 
Q 1303 -91 875 529 
Q 447 1150 447 2328 
Q 447 3434 972 4092 
Q 1497 4750 2381 4750 
Q 2619 4750 2861 4703 
Q 3103 4656 3366 4563 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-36"/>
      </g>
     </g>
    </g>
    <g id="xtick_5">
     <g id="line2d_5">
      <g>
       <use xlink:href="#m4fc69e083e" x="210.909091" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_5">
      <!-- 8 -->
      <g transform="translate(207.727841 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-38" d="M 2034 2216 
Q 1584 2216 1326 1975 
Q 1069 1734 1069 1313 
Q 1069 891 1326 650 
Q 1584 409 2034 409 
Q 2484 409 2743 651 
Q 3003 894 3003 1313 
Q 3003 1734 2745 1975 
Q 2488 2216 2034 2216 
z
M 1403 2484 
Q 997 2584 770 2862 
Q 544 3141 544 3541 
Q 544 4100 942 4425 
Q 1341 4750 2034 4750 
Q 2731 4750 3128 4425 
Q 3525 4100 3525 3541 
Q 3525 3141 3298 2862 
Q 3072 2584 2669 2484 
Q 3125 2378 3379 2068 
Q 3634 1759 3634 1313 
Q 3634 634 3220 271 
Q 2806 -91 2034 -91 
Q 1263 -91 848 271 
Q 434 634 434 1313 
Q 434 1759 690 2068 
Q 947 2378 1403 2484 
z
M 1172 3481 
Q 1172 3119 1398 2916 
Q 1625 2713 2034 2713 
Q 2441 2713 2670 2916 
Q 2900 3119 2900 3481 
Q 2900 3844 2670 4047 
Q 2441 4250 2034 4250 
Q 1625 4250 1398 4047 
Q 1172 3844 1172 3481 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-38"/>
      </g>
     </g>
    </g>
   </g>
   <g id="matplotlib.axis_2">
    <g id="ytick_1">
     <g id="line2d_6">
      <defs>
       <path id="m31fefc1a0b" d="M 0 0 
L -3.5 0 
" style="stroke: #000000; stroke-width: 0.8"/>
      </defs>
      <g>
       <use xlink:href="#m31fefc1a0b" x="57.6" y="129.437091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_6">
      <!-- 0 -->
      <g transform="translate(44.2375 133.23631)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="ytick_2">
     <g id="line2d_7">
      <g>
       <use xlink:href="#m31fefc1a0b" x="57.6" y="165.509818" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_7">
      <!-- 2 -->
      <g transform="translate(44.2375 169.309037)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="ytick_3">
     <g id="line2d_8">
      <g>
       <use xlink:href="#m31fefc1a0b" x="57.6" y="201.582545" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_8">
      <!-- 4 -->
      <g transform="translate(44.2375 205.381764)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
   </g>
   <g id="patch_3">
    <path d="M 57.6 228.637091 
L 57.6 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_4">
    <path d="M 219.927273 228.637091 
L 219.927273 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_5">
    <path d="M 57.6 228.637091 
L 219.927273 228.637091 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_6">
    <path d="M 57.6 120.418909 
L 219.927273 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="text_9">
    <!-- DRA -->
    <g transform="translate(126.110199 114.418909)scale(0.12 -0.12)">
     <defs>
      <path id="DejaVuSans-44" d="M 1259 4147 
L 1259 519 
L 2022 519 
Q 2988 519 3436 956 
Q 3884 1394 3884 2338 
Q 3884 3275 3436 3711 
Q 2988 4147 2022 4147 
L 1259 4147 
z
M 628 4666 
L 1925 4666 
Q 3281 4666 3915 4102 
Q 4550 3538 4550 2338 
Q 4550 1131 3912 565 
Q 3275 0 1925 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-52" d="M 2841 2188 
Q 3044 2119 3236 1894 
Q 3428 1669 3622 1275 
L 4263 0 
L 3584 0 
L 2988 1197 
Q 2756 1666 2539 1819 
Q 2322 1972 1947 1972 
L 1259 1972 
L 1259 0 
L 628 0 
L 628 4666 
L 2053 4666 
Q 2853 4666 3247 4331 
Q 3641 3997 3641 3322 
Q 3641 2881 3436 2590 
Q 3231 2300 2841 2188 
z
M 1259 4147 
L 1259 2491 
L 2053 2491 
Q 2509 2491 2742 2702 
Q 2975 2913 2975 3322 
Q 2975 3731 2742 3939 
Q 2509 4147 2053 4147 
L 1259 4147 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-41" d="M 2188 4044 
L 1331 1722 
L 3047 1722 
L 2188 4044 
z
M 1831 4666 
L 2547 4666 
L 4325 0 
L 3669 0 
L 3244 1197 
L 1141 1197 
L 716 0 
L 50 0 
L 1831 4666 
z
" transform="scale(0.015625)"/>
     </defs>
     <use xlink:href="#DejaVuSans-44"/>
     <use xlink:href="#DejaVuSans-52" x="77.001953"/>
     <use xlink:href="#DejaVuSans-41" x="142.484375"/>
    </g>
   </g>
  </g>
  <g id="axes_2">
   <g id="patch_7">
    <path d="M 252.392727 228.637091 
L 414.72 228.637091 
L 414.72 120.418909 
L 252.392727 120.418909 
z
" style="fill: #ffffff"/>
   </g>
   <g clip-path="url(#p9b948a1018)">
    <image xlink:href="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAOIAAACXCAYAAAAI/4elAAADEklEQVR4nO3cMYqdZRiG4e8kcbBQFDEpTEgxWBm0EYRgqY1gIRKLDIiuYRpxBRZC9iA2YixsRERXYIqIFkEhgmQGC4MgiOhk4nEJVu/hLq5rAw+c/9x83bv54/jSdg07uPvG9MR6+am74xufH70wvnHzykfjG2uttbfZ7GRn2sl2/O+7Du68Pb5xZnwB+F9ChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQI2V699OH6h9Ylvj6cn1jo9HZ94+Nv98Y2vH3yyk8u/z713Y/y7X/745+mJ9eeLl8c3Hv3i1viGFxEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIEbA5vvzV+aPbLT69OT6w7HxyOH+Z99cz8b7W28xP0eBEhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAh4NxjZ/8ZH/n7/L/jG7tw9sL58Y3N3t74xlprnd472snOtG+2n81feN9cGz+/7kWEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAeeuP3lrfOT1N78b33jpcHxi/fj+/vjG/vPH4xtrrfXIwYX5kZMH8xu/z0/sghcRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBGzuHz2znR756q+L0xM78cvJ0+Mbrz3+w/jGWmu9+/074xs3rtwc33hl/6fN9MbDX58db8SLCAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQL+A1yzTCxwizaeAAAAAElFTkSuQmCC" id="image2d06915dae" transform="scale(1 -1)translate(0 -108.72)" x="252.392727" y="-119.917091" width="162.72" height="108.72"/>
   </g>
   <g id="matplotlib.axis_3">
    <g id="xtick_6">
     <g id="line2d_9">
      <g>
       <use xlink:href="#m4fc69e083e" x="261.410909" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_10">
      <!-- 0 -->
      <g transform="translate(258.229659 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="xtick_7">
     <g id="line2d_10">
      <g>
       <use xlink:href="#m4fc69e083e" x="297.483636" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_11">
      <!-- 2 -->
      <g transform="translate(294.302386 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="xtick_8">
     <g id="line2d_11">
      <g>
       <use xlink:href="#m4fc69e083e" x="333.556364" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_12">
      <!-- 4 -->
      <g transform="translate(330.375114 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
    <g id="xtick_9">
     <g id="line2d_12">
      <g>
       <use xlink:href="#m4fc69e083e" x="369.629091" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_13">
      <!-- 6 -->
      <g transform="translate(366.447841 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-36"/>
      </g>
     </g>
    </g>
    <g id="xtick_10">
     <g id="line2d_13">
      <g>
       <use xlink:href="#m4fc69e083e" x="405.701818" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_14">
      <!-- 8 -->
      <g transform="translate(402.520568 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-38"/>
      </g>
     </g>
    </g>
   </g>
   <g id="matplotlib.axis_4">
    <g id="ytick_4">
     <g id="line2d_14">
      <g>
       <use xlink:href="#m31fefc1a0b" x="252.392727" y="129.437091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_15">
      <!-- 0 -->
      <g transform="translate(239.030227 133.23631)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="ytick_5">
     <g id="line2d_15">
      <g>
       <use xlink:href="#m31fefc1a0b" x="252.392727" y="165.509818" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_16">
      <!-- 2 -->
      <g transform="translate(239.030227 169.309037)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="ytick_6">
     <g id="line2d_16">
      <g>
       <use xlink:href="#m31fefc1a0b" x="252.392727" y="201.582545" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_17">
      <!-- 4 -->
      <g transform="translate(239.030227 205.381764)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
   </g>
   <g id="patch_8">
    <path d="M 252.392727 228.637091 
L 252.392727 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_9">
    <path d="M 414.72 228.637091 
L 414.72 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_10">
    <path d="M 252.392727 228.637091 
L 414.72 228.637091 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_11">
    <path d="M 252.392727 120.418909 
L 414.72 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="text_18">
    <!-- MaxEntRL -->
    <g transform="translate(303.693239 114.418909)scale(0.12 -0.12)">
     <defs>
      <path id="DejaVuSans-4d" d="M 628 4666 
L 1569 4666 
L 2759 1491 
L 3956 4666 
L 4897 4666 
L 4897 0 
L 4281 0 
L 4281 4097 
L 3078 897 
L 2444 897 
L 1241 4097 
L 1241 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-61" d="M 2194 1759 
Q 1497 1759 1228 1600 
Q 959 1441 959 1056 
Q 959 750 1161 570 
Q 1363 391 1709 391 
Q 2188 391 2477 730 
Q 2766 1069 2766 1631 
L 2766 1759 
L 2194 1759 
z
M 3341 1997 
L 3341 0 
L 2766 0 
L 2766 531 
Q 2569 213 2275 61 
Q 1981 -91 1556 -91 
Q 1019 -91 701 211 
Q 384 513 384 1019 
Q 384 1609 779 1909 
Q 1175 2209 1959 2209 
L 2766 2209 
L 2766 2266 
Q 2766 2663 2505 2880 
Q 2244 3097 1772 3097 
Q 1472 3097 1187 3025 
Q 903 2953 641 2809 
L 641 3341 
Q 956 3463 1253 3523 
Q 1550 3584 1831 3584 
Q 2591 3584 2966 3190 
Q 3341 2797 3341 1997 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-78" d="M 3513 3500 
L 2247 1797 
L 3578 0 
L 2900 0 
L 1881 1375 
L 863 0 
L 184 0 
L 1544 1831 
L 300 3500 
L 978 3500 
L 1906 2253 
L 2834 3500 
L 3513 3500 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-45" d="M 628 4666 
L 3578 4666 
L 3578 4134 
L 1259 4134 
L 1259 2753 
L 3481 2753 
L 3481 2222 
L 1259 2222 
L 1259 531 
L 3634 531 
L 3634 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-6e" d="M 3513 2113 
L 3513 0 
L 2938 0 
L 2938 2094 
Q 2938 2591 2744 2837 
Q 2550 3084 2163 3084 
Q 1697 3084 1428 2787 
Q 1159 2491 1159 1978 
L 1159 0 
L 581 0 
L 581 3500 
L 1159 3500 
L 1159 2956 
Q 1366 3272 1645 3428 
Q 1925 3584 2291 3584 
Q 2894 3584 3203 3211 
Q 3513 2838 3513 2113 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-74" d="M 1172 4494 
L 1172 3500 
L 2356 3500 
L 2356 3053 
L 1172 3053 
L 1172 1153 
Q 1172 725 1289 603 
Q 1406 481 1766 481 
L 2356 481 
L 2356 0 
L 1766 0 
Q 1100 0 847 248 
Q 594 497 594 1153 
L 594 3053 
L 172 3053 
L 172 3500 
L 594 3500 
L 594 4494 
L 1172 4494 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-4c" d="M 628 4666 
L 1259 4666 
L 1259 531 
L 3531 531 
L 3531 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
     </defs>
     <use xlink:href="#DejaVuSans-4d"/>
     <use xlink:href="#DejaVuSans-61" x="86.279297"/>
     <use xlink:href="#DejaVuSans-78" x="147.558594"/>
     <use xlink:href="#DejaVuSans-45" x="206.738281"/>
     <use xlink:href="#DejaVuSans-6e" x="269.921875"/>
     <use xlink:href="#DejaVuSans-74" x="333.300781"/>
     <use xlink:href="#DejaVuSans-52" x="372.509766"/>
     <use xlink:href="#DejaVuSans-4c" x="441.992188"/>
    </g>
   </g>
  </g>
 </g>
 <defs>
  <clipPath id="p849d3653ab">
   <rect x="57.6" y="120.418909" width="162.327273" height="108.218182"/>
  </clipPath>
  <clipPath id="p9b948a1018">
   <rect x="252.392727" y="120.418909" width="162.327273" height="108.218182"/>
  </clipPath>
 </defs>
</svg>


##### Action values

<?xml version="1.0" encoding="utf-8" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN"
  "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns:xlink="http://www.w3.org/1999/xlink" width="460.8pt" height="345.6pt" viewBox="0 0 460.8 345.6" xmlns="http://www.w3.org/2000/svg" version="1.1">
 <metadata>
  <rdf:RDF xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:cc="http://creativecommons.org/ns#" xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
   <cc:Work>
    <dc:type rdf:resource="http://purl.org/dc/dcmitype/StillImage"/>
    <dc:date>2022-05-11T14:26:13.857578</dc:date>
    <dc:format>image/svg+xml</dc:format>
    <dc:creator>
     <cc:Agent>
      <dc:title>Matplotlib v3.5.1, https://matplotlib.org/</dc:title>
     </cc:Agent>
    </dc:creator>
   </cc:Work>
  </rdf:RDF>
 </metadata>
 <defs>
  <style type="text/css">*{stroke-linejoin: round; stroke-linecap: butt}</style>
 </defs>
 <g id="figure_1">
  <g id="patch_1">
   <path d="M 0 345.6 
L 460.8 345.6 
L 460.8 0 
L 0 0 
z
" style="fill: #ffffff"/>
  </g>
  <g id="axes_1">
   <g id="patch_2">
    <path d="M 57.6 228.637091 
L 219.927273 228.637091 
L 219.927273 120.418909 
L 57.6 120.418909 
z
" style="fill: #ffffff"/>
   </g>
   <g clip-path="url(#p51cb418d61)">
    <image xlink:href="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAOIAAACXCAYAAAAI/4elAAADIklEQVR4nO3cP4rdZRiG4W9mTjIzzp8oisUogoWElFNYuIOULiC1IIhVwC1Y6A4USxcguAHBwsrCWogSDQZUNOB4JpO4BKt3uIvr2sDD+XFuvu7duXv2wfM17fBgfOLZ8fzG1cn8xuXxZnxjrbW2t+Z3npztjm9s/pn/+z493BnfmP9SwP8SIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBCw+fXdN8dHLk/mD7RuT+cPzV6ezG88+PD+/Mdaa52/9+n4j/n7/GJ6Yu092h/fePrKdnzDiwgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBECdt6+98n4odmD36+mJ9Y3X380fpj3jS8+Hv9Wh6fzR3nXWuvV0yfjG68f/zm+8dbRb+Mbdw4fjm94ESFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFgs//Xs/GRo+9/Ht+4Di9/e2N8Y/dyfmOttX65fTK+8e/53vjGl+98Nn7h/YcHZ+MX3r2IECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIWBz8NV34yNX+/vjG9fh1o/b8Y3ti5vxjbXWOnw8f/z30cOXxjeuw52bL4xveBEhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCwGb34GB8ZOf4aHzj7mv3n09vXP1xMT2xdi9vjm+stdbu9sb4xt7F/MZP4wtr3f78/fENLyIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQI+A8A2UcyBoV8+QAAAABJRU5ErkJggg==" id="image47f73a4ad1" transform="scale(1 -1)translate(0 -108.72)" x="57.6" y="-119.917091" width="162.72" height="108.72"/>
   </g>
   <g id="matplotlib.axis_1">
    <g id="xtick_1">
     <g id="line2d_1">
      <defs>
       <path id="mbab493bbce" d="M 0 0 
L 0 3.5 
" style="stroke: #000000; stroke-width: 0.8"/>
      </defs>
      <g>
       <use xlink:href="#mbab493bbce" x="66.618182" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_1">
      <!-- 0 -->
      <g transform="translate(63.436932 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-30" d="M 2034 4250 
Q 1547 4250 1301 3770 
Q 1056 3291 1056 2328 
Q 1056 1369 1301 889 
Q 1547 409 2034 409 
Q 2525 409 2770 889 
Q 3016 1369 3016 2328 
Q 3016 3291 2770 3770 
Q 2525 4250 2034 4250 
z
M 2034 4750 
Q 2819 4750 3233 4129 
Q 3647 3509 3647 2328 
Q 3647 1150 3233 529 
Q 2819 -91 2034 -91 
Q 1250 -91 836 529 
Q 422 1150 422 2328 
Q 422 3509 836 4129 
Q 1250 4750 2034 4750 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="xtick_2">
     <g id="line2d_2">
      <g>
       <use xlink:href="#mbab493bbce" x="102.690909" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_2">
      <!-- 2 -->
      <g transform="translate(99.509659 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-32" d="M 1228 531 
L 3431 531 
L 3431 0 
L 469 0 
L 469 531 
Q 828 903 1448 1529 
Q 2069 2156 2228 2338 
Q 2531 2678 2651 2914 
Q 2772 3150 2772 3378 
Q 2772 3750 2511 3984 
Q 2250 4219 1831 4219 
Q 1534 4219 1204 4116 
Q 875 4013 500 3803 
L 500 4441 
Q 881 4594 1212 4672 
Q 1544 4750 1819 4750 
Q 2544 4750 2975 4387 
Q 3406 4025 3406 3419 
Q 3406 3131 3298 2873 
Q 3191 2616 2906 2266 
Q 2828 2175 2409 1742 
Q 1991 1309 1228 531 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="xtick_3">
     <g id="line2d_3">
      <g>
       <use xlink:href="#mbab493bbce" x="138.763636" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_3">
      <!-- 4 -->
      <g transform="translate(135.582386 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-34" d="M 2419 4116 
L 825 1625 
L 2419 1625 
L 2419 4116 
z
M 2253 4666 
L 3047 4666 
L 3047 1625 
L 3713 1625 
L 3713 1100 
L 3047 1100 
L 3047 0 
L 2419 0 
L 2419 1100 
L 313 1100 
L 313 1709 
L 2253 4666 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
    <g id="xtick_4">
     <g id="line2d_4">
      <g>
       <use xlink:href="#mbab493bbce" x="174.836364" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_4">
      <!-- 6 -->
      <g transform="translate(171.655114 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-36" d="M 2113 2584 
Q 1688 2584 1439 2293 
Q 1191 2003 1191 1497 
Q 1191 994 1439 701 
Q 1688 409 2113 409 
Q 2538 409 2786 701 
Q 3034 994 3034 1497 
Q 3034 2003 2786 2293 
Q 2538 2584 2113 2584 
z
M 3366 4563 
L 3366 3988 
Q 3128 4100 2886 4159 
Q 2644 4219 2406 4219 
Q 1781 4219 1451 3797 
Q 1122 3375 1075 2522 
Q 1259 2794 1537 2939 
Q 1816 3084 2150 3084 
Q 2853 3084 3261 2657 
Q 3669 2231 3669 1497 
Q 3669 778 3244 343 
Q 2819 -91 2113 -91 
Q 1303 -91 875 529 
Q 447 1150 447 2328 
Q 447 3434 972 4092 
Q 1497 4750 2381 4750 
Q 2619 4750 2861 4703 
Q 3103 4656 3366 4563 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-36"/>
      </g>
     </g>
    </g>
    <g id="xtick_5">
     <g id="line2d_5">
      <g>
       <use xlink:href="#mbab493bbce" x="210.909091" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_5">
      <!-- 8 -->
      <g transform="translate(207.727841 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-38" d="M 2034 2216 
Q 1584 2216 1326 1975 
Q 1069 1734 1069 1313 
Q 1069 891 1326 650 
Q 1584 409 2034 409 
Q 2484 409 2743 651 
Q 3003 894 3003 1313 
Q 3003 1734 2745 1975 
Q 2488 2216 2034 2216 
z
M 1403 2484 
Q 997 2584 770 2862 
Q 544 3141 544 3541 
Q 544 4100 942 4425 
Q 1341 4750 2034 4750 
Q 2731 4750 3128 4425 
Q 3525 4100 3525 3541 
Q 3525 3141 3298 2862 
Q 3072 2584 2669 2484 
Q 3125 2378 3379 2068 
Q 3634 1759 3634 1313 
Q 3634 634 3220 271 
Q 2806 -91 2034 -91 
Q 1263 -91 848 271 
Q 434 634 434 1313 
Q 434 1759 690 2068 
Q 947 2378 1403 2484 
z
M 1172 3481 
Q 1172 3119 1398 2916 
Q 1625 2713 2034 2713 
Q 2441 2713 2670 2916 
Q 2900 3119 2900 3481 
Q 2900 3844 2670 4047 
Q 2441 4250 2034 4250 
Q 1625 4250 1398 4047 
Q 1172 3844 1172 3481 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-38"/>
      </g>
     </g>
    </g>
   </g>
   <g id="matplotlib.axis_2">
    <g id="ytick_1">
     <g id="line2d_6">
      <defs>
       <path id="mab8508062e" d="M 0 0 
L -3.5 0 
" style="stroke: #000000; stroke-width: 0.8"/>
      </defs>
      <g>
       <use xlink:href="#mab8508062e" x="57.6" y="129.437091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_6">
      <!-- 0 -->
      <g transform="translate(44.2375 133.23631)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="ytick_2">
     <g id="line2d_7">
      <g>
       <use xlink:href="#mab8508062e" x="57.6" y="165.509818" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_7">
      <!-- 2 -->
      <g transform="translate(44.2375 169.309037)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="ytick_3">
     <g id="line2d_8">
      <g>
       <use xlink:href="#mab8508062e" x="57.6" y="201.582545" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_8">
      <!-- 4 -->
      <g transform="translate(44.2375 205.381764)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
   </g>
   <g id="patch_3">
    <path d="M 57.6 228.637091 
L 57.6 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_4">
    <path d="M 219.927273 228.637091 
L 219.927273 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_5">
    <path d="M 57.6 228.637091 
L 219.927273 228.637091 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_6">
    <path d="M 57.6 120.418909 
L 219.927273 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="text_9">
    <!-- DRA q-values -->
    <g transform="translate(98.872074 114.418909)scale(0.12 -0.12)">
     <defs>
      <path id="DejaVuSans-44" d="M 1259 4147 
L 1259 519 
L 2022 519 
Q 2988 519 3436 956 
Q 3884 1394 3884 2338 
Q 3884 3275 3436 3711 
Q 2988 4147 2022 4147 
L 1259 4147 
z
M 628 4666 
L 1925 4666 
Q 3281 4666 3915 4102 
Q 4550 3538 4550 2338 
Q 4550 1131 3912 565 
Q 3275 0 1925 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-52" d="M 2841 2188 
Q 3044 2119 3236 1894 
Q 3428 1669 3622 1275 
L 4263 0 
L 3584 0 
L 2988 1197 
Q 2756 1666 2539 1819 
Q 2322 1972 1947 1972 
L 1259 1972 
L 1259 0 
L 628 0 
L 628 4666 
L 2053 4666 
Q 2853 4666 3247 4331 
Q 3641 3997 3641 3322 
Q 3641 2881 3436 2590 
Q 3231 2300 2841 2188 
z
M 1259 4147 
L 1259 2491 
L 2053 2491 
Q 2509 2491 2742 2702 
Q 2975 2913 2975 3322 
Q 2975 3731 2742 3939 
Q 2509 4147 2053 4147 
L 1259 4147 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-41" d="M 2188 4044 
L 1331 1722 
L 3047 1722 
L 2188 4044 
z
M 1831 4666 
L 2547 4666 
L 4325 0 
L 3669 0 
L 3244 1197 
L 1141 1197 
L 716 0 
L 50 0 
L 1831 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-20" transform="scale(0.015625)"/>
      <path id="DejaVuSans-71" d="M 947 1747 
Q 947 1113 1208 752 
Q 1469 391 1925 391 
Q 2381 391 2643 752 
Q 2906 1113 2906 1747 
Q 2906 2381 2643 2742 
Q 2381 3103 1925 3103 
Q 1469 3103 1208 2742 
Q 947 2381 947 1747 
z
M 2906 525 
Q 2725 213 2448 61 
Q 2172 -91 1784 -91 
Q 1150 -91 751 415 
Q 353 922 353 1747 
Q 353 2572 751 3078 
Q 1150 3584 1784 3584 
Q 2172 3584 2448 3432 
Q 2725 3281 2906 2969 
L 2906 3500 
L 3481 3500 
L 3481 -1331 
L 2906 -1331 
L 2906 525 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-2d" d="M 313 2009 
L 1997 2009 
L 1997 1497 
L 313 1497 
L 313 2009 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-76" d="M 191 3500 
L 800 3500 
L 1894 563 
L 2988 3500 
L 3597 3500 
L 2284 0 
L 1503 0 
L 191 3500 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-61" d="M 2194 1759 
Q 1497 1759 1228 1600 
Q 959 1441 959 1056 
Q 959 750 1161 570 
Q 1363 391 1709 391 
Q 2188 391 2477 730 
Q 2766 1069 2766 1631 
L 2766 1759 
L 2194 1759 
z
M 3341 1997 
L 3341 0 
L 2766 0 
L 2766 531 
Q 2569 213 2275 61 
Q 1981 -91 1556 -91 
Q 1019 -91 701 211 
Q 384 513 384 1019 
Q 384 1609 779 1909 
Q 1175 2209 1959 2209 
L 2766 2209 
L 2766 2266 
Q 2766 2663 2505 2880 
Q 2244 3097 1772 3097 
Q 1472 3097 1187 3025 
Q 903 2953 641 2809 
L 641 3341 
Q 956 3463 1253 3523 
Q 1550 3584 1831 3584 
Q 2591 3584 2966 3190 
Q 3341 2797 3341 1997 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-6c" d="M 603 4863 
L 1178 4863 
L 1178 0 
L 603 0 
L 603 4863 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-75" d="M 544 1381 
L 544 3500 
L 1119 3500 
L 1119 1403 
Q 1119 906 1312 657 
Q 1506 409 1894 409 
Q 2359 409 2629 706 
Q 2900 1003 2900 1516 
L 2900 3500 
L 3475 3500 
L 3475 0 
L 2900 0 
L 2900 538 
Q 2691 219 2414 64 
Q 2138 -91 1772 -91 
Q 1169 -91 856 284 
Q 544 659 544 1381 
z
M 1991 3584 
L 1991 3584 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-65" d="M 3597 1894 
L 3597 1613 
L 953 1613 
Q 991 1019 1311 708 
Q 1631 397 2203 397 
Q 2534 397 2845 478 
Q 3156 559 3463 722 
L 3463 178 
Q 3153 47 2828 -22 
Q 2503 -91 2169 -91 
Q 1331 -91 842 396 
Q 353 884 353 1716 
Q 353 2575 817 3079 
Q 1281 3584 2069 3584 
Q 2775 3584 3186 3129 
Q 3597 2675 3597 1894 
z
M 3022 2063 
Q 3016 2534 2758 2815 
Q 2500 3097 2075 3097 
Q 1594 3097 1305 2825 
Q 1016 2553 972 2059 
L 3022 2063 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-73" d="M 2834 3397 
L 2834 2853 
Q 2591 2978 2328 3040 
Q 2066 3103 1784 3103 
Q 1356 3103 1142 2972 
Q 928 2841 928 2578 
Q 928 2378 1081 2264 
Q 1234 2150 1697 2047 
L 1894 2003 
Q 2506 1872 2764 1633 
Q 3022 1394 3022 966 
Q 3022 478 2636 193 
Q 2250 -91 1575 -91 
Q 1294 -91 989 -36 
Q 684 19 347 128 
L 347 722 
Q 666 556 975 473 
Q 1284 391 1588 391 
Q 1994 391 2212 530 
Q 2431 669 2431 922 
Q 2431 1156 2273 1281 
Q 2116 1406 1581 1522 
L 1381 1569 
Q 847 1681 609 1914 
Q 372 2147 372 2553 
Q 372 3047 722 3315 
Q 1072 3584 1716 3584 
Q 2034 3584 2315 3537 
Q 2597 3491 2834 3397 
z
" transform="scale(0.015625)"/>
     </defs>
     <use xlink:href="#DejaVuSans-44"/>
     <use xlink:href="#DejaVuSans-52" x="77.001953"/>
     <use xlink:href="#DejaVuSans-41" x="142.484375"/>
     <use xlink:href="#DejaVuSans-20" x="210.892578"/>
     <use xlink:href="#DejaVuSans-71" x="242.679688"/>
     <use xlink:href="#DejaVuSans-2d" x="306.15625"/>
     <use xlink:href="#DejaVuSans-76" x="339.615234"/>
     <use xlink:href="#DejaVuSans-61" x="398.794922"/>
     <use xlink:href="#DejaVuSans-6c" x="460.074219"/>
     <use xlink:href="#DejaVuSans-75" x="487.857422"/>
     <use xlink:href="#DejaVuSans-65" x="551.236328"/>
     <use xlink:href="#DejaVuSans-73" x="612.759766"/>
    </g>
   </g>
  </g>
  <g id="axes_2">
   <g id="patch_7">
    <path d="M 252.392727 228.637091 
L 414.72 228.637091 
L 414.72 120.418909 
L 252.392727 120.418909 
z
" style="fill: #ffffff"/>
   </g>
   <g clip-path="url(#pf486c05ce8)">
    <image xlink:href="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAOIAAACXCAYAAAAI/4elAAADD0lEQVR4nO3cv46UZRyG4W+WgR0MCwsLxtpoY2OMpYXldh6FIdHGytgTGgo5E2pC6zHYWJlYGTYhou66f1kLD8DqN7mL6zqBJ/N+ueft3tXhB99eL9Pu3hmfuDqY3zjf3x3f2Px+PL6xLMty/uD2/Mb+enxj983F+MbF3vzv2BlfAP6XECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoSA9XJvb3zk8tH8xtmDW+MbJ49ujG/89PLJanxkWZbPvv5x/GHpN5++m55Y7v88/+jznx+NT7gRoUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQsDq8P1vxh+aXe7fHZ949cuz8Yd5P/nh+fhZnR7Mf45lWZbLhxfjG/sP/x7f+PjgaHzj83u/jW+4ESFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFgfX1yMj4y/gT3lmyO5l/h3jnfzmmdXt4c3/hjuTO+8eKrp+MH9t2vH45/eDciBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECFi/Oz4eH1mdnY1vbMN7R1fjG5u323lg+OZf8//BJ6e3xje24YvN/Fm5ESFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIULAelnNP2i7s7c3vnF48P319MbV63+mJ5bV+K/4z+b1jfmNt7vjG9vw5ePH4xtuRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBDwL0sJRgDbLxbRAAAAAElFTkSuQmCC" id="image1c9929b382" transform="scale(1 -1)translate(0 -108.72)" x="252.392727" y="-119.917091" width="162.72" height="108.72"/>
   </g>
   <g id="matplotlib.axis_3">
    <g id="xtick_6">
     <g id="line2d_9">
      <g>
       <use xlink:href="#mbab493bbce" x="261.410909" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_10">
      <!-- 0 -->
      <g transform="translate(258.229659 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="xtick_7">
     <g id="line2d_10">
      <g>
       <use xlink:href="#mbab493bbce" x="297.483636" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_11">
      <!-- 2 -->
      <g transform="translate(294.302386 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="xtick_8">
     <g id="line2d_11">
      <g>
       <use xlink:href="#mbab493bbce" x="333.556364" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_12">
      <!-- 4 -->
      <g transform="translate(330.375114 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
    <g id="xtick_9">
     <g id="line2d_12">
      <g>
       <use xlink:href="#mbab493bbce" x="369.629091" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_13">
      <!-- 6 -->
      <g transform="translate(366.447841 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-36"/>
      </g>
     </g>
    </g>
    <g id="xtick_10">
     <g id="line2d_13">
      <g>
       <use xlink:href="#mbab493bbce" x="405.701818" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_14">
      <!-- 8 -->
      <g transform="translate(402.520568 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-38"/>
      </g>
     </g>
    </g>
   </g>
   <g id="matplotlib.axis_4">
    <g id="ytick_4">
     <g id="line2d_14">
      <g>
       <use xlink:href="#mab8508062e" x="252.392727" y="129.437091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_15">
      <!-- 0 -->
      <g transform="translate(239.030227 133.23631)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="ytick_5">
     <g id="line2d_15">
      <g>
       <use xlink:href="#mab8508062e" x="252.392727" y="165.509818" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_16">
      <!-- 2 -->
      <g transform="translate(239.030227 169.309037)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="ytick_6">
     <g id="line2d_16">
      <g>
       <use xlink:href="#mab8508062e" x="252.392727" y="201.582545" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_17">
      <!-- 4 -->
      <g transform="translate(239.030227 205.381764)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
   </g>
   <g id="patch_8">
    <path d="M 252.392727 228.637091 
L 252.392727 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_9">
    <path d="M 414.72 228.637091 
L 414.72 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_10">
    <path d="M 252.392727 228.637091 
L 414.72 228.637091 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_11">
    <path d="M 252.392727 120.418909 
L 414.72 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="text_18">
    <!-- MaxEntRL q-values -->
    <g transform="translate(276.455114 114.418909)scale(0.12 -0.12)">
     <defs>
      <path id="DejaVuSans-4d" d="M 628 4666 
L 1569 4666 
L 2759 1491 
L 3956 4666 
L 4897 4666 
L 4897 0 
L 4281 0 
L 4281 4097 
L 3078 897 
L 2444 897 
L 1241 4097 
L 1241 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-78" d="M 3513 3500 
L 2247 1797 
L 3578 0 
L 2900 0 
L 1881 1375 
L 863 0 
L 184 0 
L 1544 1831 
L 300 3500 
L 978 3500 
L 1906 2253 
L 2834 3500 
L 3513 3500 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-45" d="M 628 4666 
L 3578 4666 
L 3578 4134 
L 1259 4134 
L 1259 2753 
L 3481 2753 
L 3481 2222 
L 1259 2222 
L 1259 531 
L 3634 531 
L 3634 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-6e" d="M 3513 2113 
L 3513 0 
L 2938 0 
L 2938 2094 
Q 2938 2591 2744 2837 
Q 2550 3084 2163 3084 
Q 1697 3084 1428 2787 
Q 1159 2491 1159 1978 
L 1159 0 
L 581 0 
L 581 3500 
L 1159 3500 
L 1159 2956 
Q 1366 3272 1645 3428 
Q 1925 3584 2291 3584 
Q 2894 3584 3203 3211 
Q 3513 2838 3513 2113 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-74" d="M 1172 4494 
L 1172 3500 
L 2356 3500 
L 2356 3053 
L 1172 3053 
L 1172 1153 
Q 1172 725 1289 603 
Q 1406 481 1766 481 
L 2356 481 
L 2356 0 
L 1766 0 
Q 1100 0 847 248 
Q 594 497 594 1153 
L 594 3053 
L 172 3053 
L 172 3500 
L 594 3500 
L 594 4494 
L 1172 4494 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-4c" d="M 628 4666 
L 1259 4666 
L 1259 531 
L 3531 531 
L 3531 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
     </defs>
     <use xlink:href="#DejaVuSans-4d"/>
     <use xlink:href="#DejaVuSans-61" x="86.279297"/>
     <use xlink:href="#DejaVuSans-78" x="147.558594"/>
     <use xlink:href="#DejaVuSans-45" x="206.738281"/>
     <use xlink:href="#DejaVuSans-6e" x="269.921875"/>
     <use xlink:href="#DejaVuSans-74" x="333.300781"/>
     <use xlink:href="#DejaVuSans-52" x="372.509766"/>
     <use xlink:href="#DejaVuSans-4c" x="441.992188"/>
     <use xlink:href="#DejaVuSans-20" x="497.705078"/>
     <use xlink:href="#DejaVuSans-71" x="529.492188"/>
     <use xlink:href="#DejaVuSans-2d" x="592.96875"/>
     <use xlink:href="#DejaVuSans-76" x="626.427734"/>
     <use xlink:href="#DejaVuSans-61" x="685.607422"/>
     <use xlink:href="#DejaVuSans-6c" x="746.886719"/>
     <use xlink:href="#DejaVuSans-75" x="774.669922"/>
     <use xlink:href="#DejaVuSans-65" x="838.048828"/>
     <use xlink:href="#DejaVuSans-73" x="899.572266"/>
    </g>
   </g>
  </g>
 </g>
 <defs>
  <clipPath id="p51cb418d61">
   <rect x="57.6" y="120.418909" width="162.327273" height="108.218182"/>
  </clipPath>
  <clipPath id="pf486c05ce8">
   <rect x="252.392727" y="120.418909" width="162.327273" height="108.218182"/>
  </clipPath>
 </defs>
</svg>

<?xml version="1.0" encoding="utf-8" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN"
  "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns:xlink="http://www.w3.org/1999/xlink" width="460.8pt" height="345.6pt" viewBox="0 0 460.8 345.6" xmlns="http://www.w3.org/2000/svg" version="1.1">
 <metadata>
  <rdf:RDF xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:cc="http://creativecommons.org/ns#" xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
   <cc:Work>
    <dc:type rdf:resource="http://purl.org/dc/dcmitype/StillImage"/>
    <dc:date>2022-05-11T14:22:58.571216</dc:date>
    <dc:format>image/svg+xml</dc:format>
    <dc:creator>
     <cc:Agent>
      <dc:title>Matplotlib v3.5.1, https://matplotlib.org/</dc:title>
     </cc:Agent>
    </dc:creator>
   </cc:Work>
  </rdf:RDF>
 </metadata>
 <defs>
  <style type="text/css">*{stroke-linejoin: round; stroke-linecap: butt}</style>
 </defs>
 <g id="figure_1">
  <g id="patch_1">
   <path d="M 0 345.6 
L 460.8 345.6 
L 460.8 0 
L 0 0 
z
" style="fill: #ffffff"/>
  </g>
  <g id="axes_1">
   <g id="patch_2">
    <path d="M 57.6 228.637091 
L 219.927273 228.637091 
L 219.927273 120.418909 
L 57.6 120.418909 
z
" style="fill: #ffffff"/>
   </g>
   <g clip-path="url(#pc8acb1cc8d)">
    <image xlink:href="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAOIAAACXCAYAAAAI/4elAAADEklEQVR4nO3cPYpkZRiG4a+l+rdmqkUQYYQxMXJcgQuQzt2FmIgzmRsQwVDQxBWYuABDs0mMRRlREQxGUBH71yUYvc1tc10beDinuOtk797Zg/du1rSjw/GJ6/vH4xtX2/nnuNxuxjfWWuv8dH7n6mBvfOPiZH7j6nh+44XxBeA/CREChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQI2Nzc346P/HL2yvjG+W58Yl3s5m8xf//kg/lrtmutN9//ZPxhrg+mF9a6uDf/m5y/eDW+4YsIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAvbeeufj8Qut/+zme3/6xfxh3oeffzT+rvZ359MTa621Xjr9a3zj4e75+Mbr29/GNx6d/Dy+4YsIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAZuD3y/HR3bf/DS+cRte+2p+48e3j+dH1lrPvzuZ31gvj298+eFn4xfev3326viFd19ECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIELDZfP10fOT66Gh84zbs/zl/jPnes/3xjbXW2v56fSs7d8Gjg/mjz76IECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIWCzd3g4PrK3PRnfOHvw+GZ64/Lv+QPDpz9cjW+stdb+H/PPcn14N/7n3/j03fGNu/Gm4H9OiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIULAv6jkRTrL/w9aAAAAAElFTkSuQmCC" id="image269769491c" transform="scale(1 -1)translate(0 -108.72)" x="57.6" y="-119.917091" width="162.72" height="108.72"/>
   </g>
   <g id="matplotlib.axis_1">
    <g id="xtick_1">
     <g id="line2d_1">
      <defs>
       <path id="m98e70fd666" d="M 0 0 
L 0 3.5 
" style="stroke: #000000; stroke-width: 0.8"/>
      </defs>
      <g>
       <use xlink:href="#m98e70fd666" x="66.618182" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_1">
      <!-- 0 -->
      <g transform="translate(63.436932 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-30" d="M 2034 4250 
Q 1547 4250 1301 3770 
Q 1056 3291 1056 2328 
Q 1056 1369 1301 889 
Q 1547 409 2034 409 
Q 2525 409 2770 889 
Q 3016 1369 3016 2328 
Q 3016 3291 2770 3770 
Q 2525 4250 2034 4250 
z
M 2034 4750 
Q 2819 4750 3233 4129 
Q 3647 3509 3647 2328 
Q 3647 1150 3233 529 
Q 2819 -91 2034 -91 
Q 1250 -91 836 529 
Q 422 1150 422 2328 
Q 422 3509 836 4129 
Q 1250 4750 2034 4750 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="xtick_2">
     <g id="line2d_2">
      <g>
       <use xlink:href="#m98e70fd666" x="102.690909" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_2">
      <!-- 2 -->
      <g transform="translate(99.509659 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-32" d="M 1228 531 
L 3431 531 
L 3431 0 
L 469 0 
L 469 531 
Q 828 903 1448 1529 
Q 2069 2156 2228 2338 
Q 2531 2678 2651 2914 
Q 2772 3150 2772 3378 
Q 2772 3750 2511 3984 
Q 2250 4219 1831 4219 
Q 1534 4219 1204 4116 
Q 875 4013 500 3803 
L 500 4441 
Q 881 4594 1212 4672 
Q 1544 4750 1819 4750 
Q 2544 4750 2975 4387 
Q 3406 4025 3406 3419 
Q 3406 3131 3298 2873 
Q 3191 2616 2906 2266 
Q 2828 2175 2409 1742 
Q 1991 1309 1228 531 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="xtick_3">
     <g id="line2d_3">
      <g>
       <use xlink:href="#m98e70fd666" x="138.763636" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_3">
      <!-- 4 -->
      <g transform="translate(135.582386 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-34" d="M 2419 4116 
L 825 1625 
L 2419 1625 
L 2419 4116 
z
M 2253 4666 
L 3047 4666 
L 3047 1625 
L 3713 1625 
L 3713 1100 
L 3047 1100 
L 3047 0 
L 2419 0 
L 2419 1100 
L 313 1100 
L 313 1709 
L 2253 4666 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
    <g id="xtick_4">
     <g id="line2d_4">
      <g>
       <use xlink:href="#m98e70fd666" x="174.836364" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_4">
      <!-- 6 -->
      <g transform="translate(171.655114 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-36" d="M 2113 2584 
Q 1688 2584 1439 2293 
Q 1191 2003 1191 1497 
Q 1191 994 1439 701 
Q 1688 409 2113 409 
Q 2538 409 2786 701 
Q 3034 994 3034 1497 
Q 3034 2003 2786 2293 
Q 2538 2584 2113 2584 
z
M 3366 4563 
L 3366 3988 
Q 3128 4100 2886 4159 
Q 2644 4219 2406 4219 
Q 1781 4219 1451 3797 
Q 1122 3375 1075 2522 
Q 1259 2794 1537 2939 
Q 1816 3084 2150 3084 
Q 2853 3084 3261 2657 
Q 3669 2231 3669 1497 
Q 3669 778 3244 343 
Q 2819 -91 2113 -91 
Q 1303 -91 875 529 
Q 447 1150 447 2328 
Q 447 3434 972 4092 
Q 1497 4750 2381 4750 
Q 2619 4750 2861 4703 
Q 3103 4656 3366 4563 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-36"/>
      </g>
     </g>
    </g>
    <g id="xtick_5">
     <g id="line2d_5">
      <g>
       <use xlink:href="#m98e70fd666" x="210.909091" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_5">
      <!-- 8 -->
      <g transform="translate(207.727841 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-38" d="M 2034 2216 
Q 1584 2216 1326 1975 
Q 1069 1734 1069 1313 
Q 1069 891 1326 650 
Q 1584 409 2034 409 
Q 2484 409 2743 651 
Q 3003 894 3003 1313 
Q 3003 1734 2745 1975 
Q 2488 2216 2034 2216 
z
M 1403 2484 
Q 997 2584 770 2862 
Q 544 3141 544 3541 
Q 544 4100 942 4425 
Q 1341 4750 2034 4750 
Q 2731 4750 3128 4425 
Q 3525 4100 3525 3541 
Q 3525 3141 3298 2862 
Q 3072 2584 2669 2484 
Q 3125 2378 3379 2068 
Q 3634 1759 3634 1313 
Q 3634 634 3220 271 
Q 2806 -91 2034 -91 
Q 1263 -91 848 271 
Q 434 634 434 1313 
Q 434 1759 690 2068 
Q 947 2378 1403 2484 
z
M 1172 3481 
Q 1172 3119 1398 2916 
Q 1625 2713 2034 2713 
Q 2441 2713 2670 2916 
Q 2900 3119 2900 3481 
Q 2900 3844 2670 4047 
Q 2441 4250 2034 4250 
Q 1625 4250 1398 4047 
Q 1172 3844 1172 3481 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-38"/>
      </g>
     </g>
    </g>
   </g>
   <g id="matplotlib.axis_2">
    <g id="ytick_1">
     <g id="line2d_6">
      <defs>
       <path id="mb4d8f3b6a3" d="M 0 0 
L -3.5 0 
" style="stroke: #000000; stroke-width: 0.8"/>
      </defs>
      <g>
       <use xlink:href="#mb4d8f3b6a3" x="57.6" y="129.437091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_6">
      <!-- 0 -->
      <g transform="translate(44.2375 133.23631)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="ytick_2">
     <g id="line2d_7">
      <g>
       <use xlink:href="#mb4d8f3b6a3" x="57.6" y="165.509818" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_7">
      <!-- 2 -->
      <g transform="translate(44.2375 169.309037)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="ytick_3">
     <g id="line2d_8">
      <g>
       <use xlink:href="#mb4d8f3b6a3" x="57.6" y="201.582545" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_8">
      <!-- 4 -->
      <g transform="translate(44.2375 205.381764)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
   </g>
   <g id="patch_3">
    <path d="M 57.6 228.637091 
L 57.6 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_4">
    <path d="M 219.927273 228.637091 
L 219.927273 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_5">
    <path d="M 57.6 228.637091 
L 219.927273 228.637091 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_6">
    <path d="M 57.6 120.418909 
L 219.927273 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="text_9">
    <!-- DRA q-values -->
    <g transform="translate(98.872074 114.418909)scale(0.12 -0.12)">
     <defs>
      <path id="DejaVuSans-44" d="M 1259 4147 
L 1259 519 
L 2022 519 
Q 2988 519 3436 956 
Q 3884 1394 3884 2338 
Q 3884 3275 3436 3711 
Q 2988 4147 2022 4147 
L 1259 4147 
z
M 628 4666 
L 1925 4666 
Q 3281 4666 3915 4102 
Q 4550 3538 4550 2338 
Q 4550 1131 3912 565 
Q 3275 0 1925 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-52" d="M 2841 2188 
Q 3044 2119 3236 1894 
Q 3428 1669 3622 1275 
L 4263 0 
L 3584 0 
L 2988 1197 
Q 2756 1666 2539 1819 
Q 2322 1972 1947 1972 
L 1259 1972 
L 1259 0 
L 628 0 
L 628 4666 
L 2053 4666 
Q 2853 4666 3247 4331 
Q 3641 3997 3641 3322 
Q 3641 2881 3436 2590 
Q 3231 2300 2841 2188 
z
M 1259 4147 
L 1259 2491 
L 2053 2491 
Q 2509 2491 2742 2702 
Q 2975 2913 2975 3322 
Q 2975 3731 2742 3939 
Q 2509 4147 2053 4147 
L 1259 4147 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-41" d="M 2188 4044 
L 1331 1722 
L 3047 1722 
L 2188 4044 
z
M 1831 4666 
L 2547 4666 
L 4325 0 
L 3669 0 
L 3244 1197 
L 1141 1197 
L 716 0 
L 50 0 
L 1831 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-20" transform="scale(0.015625)"/>
      <path id="DejaVuSans-71" d="M 947 1747 
Q 947 1113 1208 752 
Q 1469 391 1925 391 
Q 2381 391 2643 752 
Q 2906 1113 2906 1747 
Q 2906 2381 2643 2742 
Q 2381 3103 1925 3103 
Q 1469 3103 1208 2742 
Q 947 2381 947 1747 
z
M 2906 525 
Q 2725 213 2448 61 
Q 2172 -91 1784 -91 
Q 1150 -91 751 415 
Q 353 922 353 1747 
Q 353 2572 751 3078 
Q 1150 3584 1784 3584 
Q 2172 3584 2448 3432 
Q 2725 3281 2906 2969 
L 2906 3500 
L 3481 3500 
L 3481 -1331 
L 2906 -1331 
L 2906 525 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-2d" d="M 313 2009 
L 1997 2009 
L 1997 1497 
L 313 1497 
L 313 2009 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-76" d="M 191 3500 
L 800 3500 
L 1894 563 
L 2988 3500 
L 3597 3500 
L 2284 0 
L 1503 0 
L 191 3500 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-61" d="M 2194 1759 
Q 1497 1759 1228 1600 
Q 959 1441 959 1056 
Q 959 750 1161 570 
Q 1363 391 1709 391 
Q 2188 391 2477 730 
Q 2766 1069 2766 1631 
L 2766 1759 
L 2194 1759 
z
M 3341 1997 
L 3341 0 
L 2766 0 
L 2766 531 
Q 2569 213 2275 61 
Q 1981 -91 1556 -91 
Q 1019 -91 701 211 
Q 384 513 384 1019 
Q 384 1609 779 1909 
Q 1175 2209 1959 2209 
L 2766 2209 
L 2766 2266 
Q 2766 2663 2505 2880 
Q 2244 3097 1772 3097 
Q 1472 3097 1187 3025 
Q 903 2953 641 2809 
L 641 3341 
Q 956 3463 1253 3523 
Q 1550 3584 1831 3584 
Q 2591 3584 2966 3190 
Q 3341 2797 3341 1997 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-6c" d="M 603 4863 
L 1178 4863 
L 1178 0 
L 603 0 
L 603 4863 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-75" d="M 544 1381 
L 544 3500 
L 1119 3500 
L 1119 1403 
Q 1119 906 1312 657 
Q 1506 409 1894 409 
Q 2359 409 2629 706 
Q 2900 1003 2900 1516 
L 2900 3500 
L 3475 3500 
L 3475 0 
L 2900 0 
L 2900 538 
Q 2691 219 2414 64 
Q 2138 -91 1772 -91 
Q 1169 -91 856 284 
Q 544 659 544 1381 
z
M 1991 3584 
L 1991 3584 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-65" d="M 3597 1894 
L 3597 1613 
L 953 1613 
Q 991 1019 1311 708 
Q 1631 397 2203 397 
Q 2534 397 2845 478 
Q 3156 559 3463 722 
L 3463 178 
Q 3153 47 2828 -22 
Q 2503 -91 2169 -91 
Q 1331 -91 842 396 
Q 353 884 353 1716 
Q 353 2575 817 3079 
Q 1281 3584 2069 3584 
Q 2775 3584 3186 3129 
Q 3597 2675 3597 1894 
z
M 3022 2063 
Q 3016 2534 2758 2815 
Q 2500 3097 2075 3097 
Q 1594 3097 1305 2825 
Q 1016 2553 972 2059 
L 3022 2063 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-73" d="M 2834 3397 
L 2834 2853 
Q 2591 2978 2328 3040 
Q 2066 3103 1784 3103 
Q 1356 3103 1142 2972 
Q 928 2841 928 2578 
Q 928 2378 1081 2264 
Q 1234 2150 1697 2047 
L 1894 2003 
Q 2506 1872 2764 1633 
Q 3022 1394 3022 966 
Q 3022 478 2636 193 
Q 2250 -91 1575 -91 
Q 1294 -91 989 -36 
Q 684 19 347 128 
L 347 722 
Q 666 556 975 473 
Q 1284 391 1588 391 
Q 1994 391 2212 530 
Q 2431 669 2431 922 
Q 2431 1156 2273 1281 
Q 2116 1406 1581 1522 
L 1381 1569 
Q 847 1681 609 1914 
Q 372 2147 372 2553 
Q 372 3047 722 3315 
Q 1072 3584 1716 3584 
Q 2034 3584 2315 3537 
Q 2597 3491 2834 3397 
z
" transform="scale(0.015625)"/>
     </defs>
     <use xlink:href="#DejaVuSans-44"/>
     <use xlink:href="#DejaVuSans-52" x="77.001953"/>
     <use xlink:href="#DejaVuSans-41" x="142.484375"/>
     <use xlink:href="#DejaVuSans-20" x="210.892578"/>
     <use xlink:href="#DejaVuSans-71" x="242.679688"/>
     <use xlink:href="#DejaVuSans-2d" x="306.15625"/>
     <use xlink:href="#DejaVuSans-76" x="339.615234"/>
     <use xlink:href="#DejaVuSans-61" x="398.794922"/>
     <use xlink:href="#DejaVuSans-6c" x="460.074219"/>
     <use xlink:href="#DejaVuSans-75" x="487.857422"/>
     <use xlink:href="#DejaVuSans-65" x="551.236328"/>
     <use xlink:href="#DejaVuSans-73" x="612.759766"/>
    </g>
   </g>
  </g>
  <g id="axes_2">
   <g id="patch_7">
    <path d="M 252.392727 228.637091 
L 414.72 228.637091 
L 414.72 120.418909 
L 252.392727 120.418909 
z
" style="fill: #ffffff"/>
   </g>
   <g clip-path="url(#p7165862d5f)">
    <image xlink:href="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAOIAAACXCAYAAAAI/4elAAADGElEQVR4nO3cT2rdZRiG4e80ITmntW0QBCsRR+KoLsGRQt2KOPHPqNAFiCBOugYX4NANKAiCUwUVsYo4iEjVpCVxCY7ew025rg08nPPj5pu9m3svvXu1pm2PxyeudvMbT2/vxjcutwfjG2ut9c8LR3vZmXZ+czM/cu2ZmAD+jxAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEgMO1246P/PbWnfGN85P5Q7NPbs3fYv7+/gd7uJi71msPPhn/Mcdn0wtrPT6d/yabJ+MTXkQoECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQsHnj7Y/GL7SenxxMT6yvPvtw/DDvKw8/Hv+vNicX0xNrrbVObj8e33j51p/jG6/e/H184+71n8c3vIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQcHh0dj4+svv6l/GNfTj9YvzQ93q6Ox7fWGutX9+cv75+9t3z4xufv/dw/ML7Nz+djn94LyIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIOFxffjs+cnm8n6O507Z/XIxv/P3ifv6r3Q9H4xs3Hs0fZP5xfGGt14+24xteRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBBwuNnD8d9rz90Y37h35/3xa7aXf/07PbGuX80f5V1rrYOL+e++udzPb5l299N3xje8iBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECHgP24lREDm8DrBAAAAAElFTkSuQmCC" id="image1d4d09e147" transform="scale(1 -1)translate(0 -108.72)" x="252.392727" y="-119.917091" width="162.72" height="108.72"/>
   </g>
   <g id="matplotlib.axis_3">
    <g id="xtick_6">
     <g id="line2d_9">
      <g>
       <use xlink:href="#m98e70fd666" x="261.410909" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_10">
      <!-- 0 -->
      <g transform="translate(258.229659 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="xtick_7">
     <g id="line2d_10">
      <g>
       <use xlink:href="#m98e70fd666" x="297.483636" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_11">
      <!-- 2 -->
      <g transform="translate(294.302386 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="xtick_8">
     <g id="line2d_11">
      <g>
       <use xlink:href="#m98e70fd666" x="333.556364" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_12">
      <!-- 4 -->
      <g transform="translate(330.375114 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
    <g id="xtick_9">
     <g id="line2d_12">
      <g>
       <use xlink:href="#m98e70fd666" x="369.629091" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_13">
      <!-- 6 -->
      <g transform="translate(366.447841 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-36"/>
      </g>
     </g>
    </g>
    <g id="xtick_10">
     <g id="line2d_13">
      <g>
       <use xlink:href="#m98e70fd666" x="405.701818" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_14">
      <!-- 8 -->
      <g transform="translate(402.520568 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-38"/>
      </g>
     </g>
    </g>
   </g>
   <g id="matplotlib.axis_4">
    <g id="ytick_4">
     <g id="line2d_14">
      <g>
       <use xlink:href="#mb4d8f3b6a3" x="252.392727" y="129.437091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_15">
      <!-- 0 -->
      <g transform="translate(239.030227 133.23631)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="ytick_5">
     <g id="line2d_15">
      <g>
       <use xlink:href="#mb4d8f3b6a3" x="252.392727" y="165.509818" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_16">
      <!-- 2 -->
      <g transform="translate(239.030227 169.309037)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="ytick_6">
     <g id="line2d_16">
      <g>
       <use xlink:href="#mb4d8f3b6a3" x="252.392727" y="201.582545" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_17">
      <!-- 4 -->
      <g transform="translate(239.030227 205.381764)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
   </g>
   <g id="patch_8">
    <path d="M 252.392727 228.637091 
L 252.392727 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_9">
    <path d="M 414.72 228.637091 
L 414.72 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_10">
    <path d="M 252.392727 228.637091 
L 414.72 228.637091 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_11">
    <path d="M 252.392727 120.418909 
L 414.72 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="text_18">
    <!-- MaxEntRL q-values -->
    <g transform="translate(276.455114 114.418909)scale(0.12 -0.12)">
     <defs>
      <path id="DejaVuSans-4d" d="M 628 4666 
L 1569 4666 
L 2759 1491 
L 3956 4666 
L 4897 4666 
L 4897 0 
L 4281 0 
L 4281 4097 
L 3078 897 
L 2444 897 
L 1241 4097 
L 1241 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-78" d="M 3513 3500 
L 2247 1797 
L 3578 0 
L 2900 0 
L 1881 1375 
L 863 0 
L 184 0 
L 1544 1831 
L 300 3500 
L 978 3500 
L 1906 2253 
L 2834 3500 
L 3513 3500 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-45" d="M 628 4666 
L 3578 4666 
L 3578 4134 
L 1259 4134 
L 1259 2753 
L 3481 2753 
L 3481 2222 
L 1259 2222 
L 1259 531 
L 3634 531 
L 3634 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-6e" d="M 3513 2113 
L 3513 0 
L 2938 0 
L 2938 2094 
Q 2938 2591 2744 2837 
Q 2550 3084 2163 3084 
Q 1697 3084 1428 2787 
Q 1159 2491 1159 1978 
L 1159 0 
L 581 0 
L 581 3500 
L 1159 3500 
L 1159 2956 
Q 1366 3272 1645 3428 
Q 1925 3584 2291 3584 
Q 2894 3584 3203 3211 
Q 3513 2838 3513 2113 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-74" d="M 1172 4494 
L 1172 3500 
L 2356 3500 
L 2356 3053 
L 1172 3053 
L 1172 1153 
Q 1172 725 1289 603 
Q 1406 481 1766 481 
L 2356 481 
L 2356 0 
L 1766 0 
Q 1100 0 847 248 
Q 594 497 594 1153 
L 594 3053 
L 172 3053 
L 172 3500 
L 594 3500 
L 594 4494 
L 1172 4494 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-4c" d="M 628 4666 
L 1259 4666 
L 1259 531 
L 3531 531 
L 3531 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
     </defs>
     <use xlink:href="#DejaVuSans-4d"/>
     <use xlink:href="#DejaVuSans-61" x="86.279297"/>
     <use xlink:href="#DejaVuSans-78" x="147.558594"/>
     <use xlink:href="#DejaVuSans-45" x="206.738281"/>
     <use xlink:href="#DejaVuSans-6e" x="269.921875"/>
     <use xlink:href="#DejaVuSans-74" x="333.300781"/>
     <use xlink:href="#DejaVuSans-52" x="372.509766"/>
     <use xlink:href="#DejaVuSans-4c" x="441.992188"/>
     <use xlink:href="#DejaVuSans-20" x="497.705078"/>
     <use xlink:href="#DejaVuSans-71" x="529.492188"/>
     <use xlink:href="#DejaVuSans-2d" x="592.96875"/>
     <use xlink:href="#DejaVuSans-76" x="626.427734"/>
     <use xlink:href="#DejaVuSans-61" x="685.607422"/>
     <use xlink:href="#DejaVuSans-6c" x="746.886719"/>
     <use xlink:href="#DejaVuSans-75" x="774.669922"/>
     <use xlink:href="#DejaVuSans-65" x="838.048828"/>
     <use xlink:href="#DejaVuSans-73" x="899.572266"/>
    </g>
   </g>
  </g>
 </g>
 <defs>
  <clipPath id="pc8acb1cc8d">
   <rect x="57.6" y="120.418909" width="162.327273" height="108.218182"/>
  </clipPath>
  <clipPath id="p7165862d5f">
   <rect x="252.392727" y="120.418909" width="162.327273" height="108.218182"/>
  </clipPath>
 </defs>
</svg>
<?xml version="1.0" encoding="utf-8" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN"
  "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns:xlink="http://www.w3.org/1999/xlink" width="460.8pt" height="345.6pt" viewBox="0 0 460.8 345.6" xmlns="http://www.w3.org/2000/svg" version="1.1">
 <metadata>
  <rdf:RDF xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:cc="http://creativecommons.org/ns#" xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
   <cc:Work>
    <dc:type rdf:resource="http://purl.org/dc/dcmitype/StillImage"/>
    <dc:date>2022-05-11T14:30:26.417308</dc:date>
    <dc:format>image/svg+xml</dc:format>
    <dc:creator>
     <cc:Agent>
      <dc:title>Matplotlib v3.5.1, https://matplotlib.org/</dc:title>
     </cc:Agent>
    </dc:creator>
   </cc:Work>
  </rdf:RDF>
 </metadata>
 <defs>
  <style type="text/css">*{stroke-linejoin: round; stroke-linecap: butt}</style>
 </defs>
 <g id="figure_1">
  <g id="patch_1">
   <path d="M 0 345.6 
L 460.8 345.6 
L 460.8 0 
L 0 0 
z
" style="fill: #ffffff"/>
  </g>
  <g id="axes_1">
   <g id="patch_2">
    <path d="M 57.6 228.637091 
L 219.927273 228.637091 
L 219.927273 120.418909 
L 57.6 120.418909 
z
" style="fill: #ffffff"/>
   </g>
   <g clip-path="url(#p718078bdac)">
    <image xlink:href="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAOIAAACXCAYAAAAI/4elAAADHUlEQVR4nO3cLY7dZRiH4XemJ53OTE+bIIYPAZhSECC6iTp0Na5IBCQsAAcJKyAplhWwAzQYZAUQCLSiUGaYL5aAeiY3zXVt4JfzP7nzumfn/tHDyzVsZ3s4PbHOjm6Nbzx/dX98Y/ds/O9Ya63190vXxjdOD3fGN3Yu5r/X+f7879gdXwD+kxAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEgM3PD+6Mj5xuxyfW6a35Q7Nn24vxjccffjx/zXat9fanX4x/sH/efT49sS5+vTG/sX8+vuFFhAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAE79z74fPzQ7Ol2/mbu919+ND7yxtefjX+rw+3x9MRaa62j7Z/jG6/ffDq+cefgt/GNuzd+Gd/wIkKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIULAZu/ZxRXMvBi9H317fXzjj/f2xjfWWuunN6+Nbzz562B849H7X41feP/h8WvjF95fjELgf06IECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQsDm8Jvvxkduv/Ly+MZV2P/9fHzj9o+b8Y211np2cnN84+Rk/PbvlXjn+vyhZC8iBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECNjsHswfT70K9+9+cjm9cf7keHpibS/3xjfWWmtzPH/IePds/C+5Em89eji+4UWEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAf8CXwJHtHQBBFwAAAAASUVORK5CYII=" id="image7f1791ea84" transform="scale(1 -1)translate(0 -108.72)" x="57.6" y="-119.917091" width="162.72" height="108.72"/>
   </g>
   <g id="matplotlib.axis_1">
    <g id="xtick_1">
     <g id="line2d_1">
      <defs>
       <path id="m4d6c8ec1bf" d="M 0 0 
L 0 3.5 
" style="stroke: #000000; stroke-width: 0.8"/>
      </defs>
      <g>
       <use xlink:href="#m4d6c8ec1bf" x="66.618182" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_1">
      <!-- 0 -->
      <g transform="translate(63.436932 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-30" d="M 2034 4250 
Q 1547 4250 1301 3770 
Q 1056 3291 1056 2328 
Q 1056 1369 1301 889 
Q 1547 409 2034 409 
Q 2525 409 2770 889 
Q 3016 1369 3016 2328 
Q 3016 3291 2770 3770 
Q 2525 4250 2034 4250 
z
M 2034 4750 
Q 2819 4750 3233 4129 
Q 3647 3509 3647 2328 
Q 3647 1150 3233 529 
Q 2819 -91 2034 -91 
Q 1250 -91 836 529 
Q 422 1150 422 2328 
Q 422 3509 836 4129 
Q 1250 4750 2034 4750 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="xtick_2">
     <g id="line2d_2">
      <g>
       <use xlink:href="#m4d6c8ec1bf" x="102.690909" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_2">
      <!-- 2 -->
      <g transform="translate(99.509659 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-32" d="M 1228 531 
L 3431 531 
L 3431 0 
L 469 0 
L 469 531 
Q 828 903 1448 1529 
Q 2069 2156 2228 2338 
Q 2531 2678 2651 2914 
Q 2772 3150 2772 3378 
Q 2772 3750 2511 3984 
Q 2250 4219 1831 4219 
Q 1534 4219 1204 4116 
Q 875 4013 500 3803 
L 500 4441 
Q 881 4594 1212 4672 
Q 1544 4750 1819 4750 
Q 2544 4750 2975 4387 
Q 3406 4025 3406 3419 
Q 3406 3131 3298 2873 
Q 3191 2616 2906 2266 
Q 2828 2175 2409 1742 
Q 1991 1309 1228 531 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="xtick_3">
     <g id="line2d_3">
      <g>
       <use xlink:href="#m4d6c8ec1bf" x="138.763636" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_3">
      <!-- 4 -->
      <g transform="translate(135.582386 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-34" d="M 2419 4116 
L 825 1625 
L 2419 1625 
L 2419 4116 
z
M 2253 4666 
L 3047 4666 
L 3047 1625 
L 3713 1625 
L 3713 1100 
L 3047 1100 
L 3047 0 
L 2419 0 
L 2419 1100 
L 313 1100 
L 313 1709 
L 2253 4666 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
    <g id="xtick_4">
     <g id="line2d_4">
      <g>
       <use xlink:href="#m4d6c8ec1bf" x="174.836364" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_4">
      <!-- 6 -->
      <g transform="translate(171.655114 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-36" d="M 2113 2584 
Q 1688 2584 1439 2293 
Q 1191 2003 1191 1497 
Q 1191 994 1439 701 
Q 1688 409 2113 409 
Q 2538 409 2786 701 
Q 3034 994 3034 1497 
Q 3034 2003 2786 2293 
Q 2538 2584 2113 2584 
z
M 3366 4563 
L 3366 3988 
Q 3128 4100 2886 4159 
Q 2644 4219 2406 4219 
Q 1781 4219 1451 3797 
Q 1122 3375 1075 2522 
Q 1259 2794 1537 2939 
Q 1816 3084 2150 3084 
Q 2853 3084 3261 2657 
Q 3669 2231 3669 1497 
Q 3669 778 3244 343 
Q 2819 -91 2113 -91 
Q 1303 -91 875 529 
Q 447 1150 447 2328 
Q 447 3434 972 4092 
Q 1497 4750 2381 4750 
Q 2619 4750 2861 4703 
Q 3103 4656 3366 4563 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-36"/>
      </g>
     </g>
    </g>
    <g id="xtick_5">
     <g id="line2d_5">
      <g>
       <use xlink:href="#m4d6c8ec1bf" x="210.909091" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_5">
      <!-- 8 -->
      <g transform="translate(207.727841 243.235528)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-38" d="M 2034 2216 
Q 1584 2216 1326 1975 
Q 1069 1734 1069 1313 
Q 1069 891 1326 650 
Q 1584 409 2034 409 
Q 2484 409 2743 651 
Q 3003 894 3003 1313 
Q 3003 1734 2745 1975 
Q 2488 2216 2034 2216 
z
M 1403 2484 
Q 997 2584 770 2862 
Q 544 3141 544 3541 
Q 544 4100 942 4425 
Q 1341 4750 2034 4750 
Q 2731 4750 3128 4425 
Q 3525 4100 3525 3541 
Q 3525 3141 3298 2862 
Q 3072 2584 2669 2484 
Q 3125 2378 3379 2068 
Q 3634 1759 3634 1313 
Q 3634 634 3220 271 
Q 2806 -91 2034 -91 
Q 1263 -91 848 271 
Q 434 634 434 1313 
Q 434 1759 690 2068 
Q 947 2378 1403 2484 
z
M 1172 3481 
Q 1172 3119 1398 2916 
Q 1625 2713 2034 2713 
Q 2441 2713 2670 2916 
Q 2900 3119 2900 3481 
Q 2900 3844 2670 4047 
Q 2441 4250 2034 4250 
Q 1625 4250 1398 4047 
Q 1172 3844 1172 3481 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-38"/>
      </g>
     </g>
    </g>
   </g>
   <g id="matplotlib.axis_2">
    <g id="ytick_1">
     <g id="line2d_6">
      <defs>
       <path id="m9382a749e1" d="M 0 0 
L -3.5 0 
" style="stroke: #000000; stroke-width: 0.8"/>
      </defs>
      <g>
       <use xlink:href="#m9382a749e1" x="57.6" y="129.437091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_6">
      <!-- 0 -->
      <g transform="translate(44.2375 133.23631)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="ytick_2">
     <g id="line2d_7">
      <g>
       <use xlink:href="#m9382a749e1" x="57.6" y="165.509818" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_7">
      <!-- 2 -->
      <g transform="translate(44.2375 169.309037)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="ytick_3">
     <g id="line2d_8">
      <g>
       <use xlink:href="#m9382a749e1" x="57.6" y="201.582545" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_8">
      <!-- 4 -->
      <g transform="translate(44.2375 205.381764)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
   </g>
   <g id="patch_3">
    <path d="M 57.6 228.637091 
L 57.6 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_4">
    <path d="M 219.927273 228.637091 
L 219.927273 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_5">
    <path d="M 57.6 228.637091 
L 219.927273 228.637091 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_6">
    <path d="M 57.6 120.418909 
L 219.927273 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="text_9">
    <!-- DRA q-values -->
    <g transform="translate(98.872074 114.418909)scale(0.12 -0.12)">
     <defs>
      <path id="DejaVuSans-44" d="M 1259 4147 
L 1259 519 
L 2022 519 
Q 2988 519 3436 956 
Q 3884 1394 3884 2338 
Q 3884 3275 3436 3711 
Q 2988 4147 2022 4147 
L 1259 4147 
z
M 628 4666 
L 1925 4666 
Q 3281 4666 3915 4102 
Q 4550 3538 4550 2338 
Q 4550 1131 3912 565 
Q 3275 0 1925 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-52" d="M 2841 2188 
Q 3044 2119 3236 1894 
Q 3428 1669 3622 1275 
L 4263 0 
L 3584 0 
L 2988 1197 
Q 2756 1666 2539 1819 
Q 2322 1972 1947 1972 
L 1259 1972 
L 1259 0 
L 628 0 
L 628 4666 
L 2053 4666 
Q 2853 4666 3247 4331 
Q 3641 3997 3641 3322 
Q 3641 2881 3436 2590 
Q 3231 2300 2841 2188 
z
M 1259 4147 
L 1259 2491 
L 2053 2491 
Q 2509 2491 2742 2702 
Q 2975 2913 2975 3322 
Q 2975 3731 2742 3939 
Q 2509 4147 2053 4147 
L 1259 4147 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-41" d="M 2188 4044 
L 1331 1722 
L 3047 1722 
L 2188 4044 
z
M 1831 4666 
L 2547 4666 
L 4325 0 
L 3669 0 
L 3244 1197 
L 1141 1197 
L 716 0 
L 50 0 
L 1831 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-20" transform="scale(0.015625)"/>
      <path id="DejaVuSans-71" d="M 947 1747 
Q 947 1113 1208 752 
Q 1469 391 1925 391 
Q 2381 391 2643 752 
Q 2906 1113 2906 1747 
Q 2906 2381 2643 2742 
Q 2381 3103 1925 3103 
Q 1469 3103 1208 2742 
Q 947 2381 947 1747 
z
M 2906 525 
Q 2725 213 2448 61 
Q 2172 -91 1784 -91 
Q 1150 -91 751 415 
Q 353 922 353 1747 
Q 353 2572 751 3078 
Q 1150 3584 1784 3584 
Q 2172 3584 2448 3432 
Q 2725 3281 2906 2969 
L 2906 3500 
L 3481 3500 
L 3481 -1331 
L 2906 -1331 
L 2906 525 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-2d" d="M 313 2009 
L 1997 2009 
L 1997 1497 
L 313 1497 
L 313 2009 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-76" d="M 191 3500 
L 800 3500 
L 1894 563 
L 2988 3500 
L 3597 3500 
L 2284 0 
L 1503 0 
L 191 3500 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-61" d="M 2194 1759 
Q 1497 1759 1228 1600 
Q 959 1441 959 1056 
Q 959 750 1161 570 
Q 1363 391 1709 391 
Q 2188 391 2477 730 
Q 2766 1069 2766 1631 
L 2766 1759 
L 2194 1759 
z
M 3341 1997 
L 3341 0 
L 2766 0 
L 2766 531 
Q 2569 213 2275 61 
Q 1981 -91 1556 -91 
Q 1019 -91 701 211 
Q 384 513 384 1019 
Q 384 1609 779 1909 
Q 1175 2209 1959 2209 
L 2766 2209 
L 2766 2266 
Q 2766 2663 2505 2880 
Q 2244 3097 1772 3097 
Q 1472 3097 1187 3025 
Q 903 2953 641 2809 
L 641 3341 
Q 956 3463 1253 3523 
Q 1550 3584 1831 3584 
Q 2591 3584 2966 3190 
Q 3341 2797 3341 1997 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-6c" d="M 603 4863 
L 1178 4863 
L 1178 0 
L 603 0 
L 603 4863 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-75" d="M 544 1381 
L 544 3500 
L 1119 3500 
L 1119 1403 
Q 1119 906 1312 657 
Q 1506 409 1894 409 
Q 2359 409 2629 706 
Q 2900 1003 2900 1516 
L 2900 3500 
L 3475 3500 
L 3475 0 
L 2900 0 
L 2900 538 
Q 2691 219 2414 64 
Q 2138 -91 1772 -91 
Q 1169 -91 856 284 
Q 544 659 544 1381 
z
M 1991 3584 
L 1991 3584 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-65" d="M 3597 1894 
L 3597 1613 
L 953 1613 
Q 991 1019 1311 708 
Q 1631 397 2203 397 
Q 2534 397 2845 478 
Q 3156 559 3463 722 
L 3463 178 
Q 3153 47 2828 -22 
Q 2503 -91 2169 -91 
Q 1331 -91 842 396 
Q 353 884 353 1716 
Q 353 2575 817 3079 
Q 1281 3584 2069 3584 
Q 2775 3584 3186 3129 
Q 3597 2675 3597 1894 
z
M 3022 2063 
Q 3016 2534 2758 2815 
Q 2500 3097 2075 3097 
Q 1594 3097 1305 2825 
Q 1016 2553 972 2059 
L 3022 2063 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-73" d="M 2834 3397 
L 2834 2853 
Q 2591 2978 2328 3040 
Q 2066 3103 1784 3103 
Q 1356 3103 1142 2972 
Q 928 2841 928 2578 
Q 928 2378 1081 2264 
Q 1234 2150 1697 2047 
L 1894 2003 
Q 2506 1872 2764 1633 
Q 3022 1394 3022 966 
Q 3022 478 2636 193 
Q 2250 -91 1575 -91 
Q 1294 -91 989 -36 
Q 684 19 347 128 
L 347 722 
Q 666 556 975 473 
Q 1284 391 1588 391 
Q 1994 391 2212 530 
Q 2431 669 2431 922 
Q 2431 1156 2273 1281 
Q 2116 1406 1581 1522 
L 1381 1569 
Q 847 1681 609 1914 
Q 372 2147 372 2553 
Q 372 3047 722 3315 
Q 1072 3584 1716 3584 
Q 2034 3584 2315 3537 
Q 2597 3491 2834 3397 
z
" transform="scale(0.015625)"/>
     </defs>
     <use xlink:href="#DejaVuSans-44"/>
     <use xlink:href="#DejaVuSans-52" x="77.001953"/>
     <use xlink:href="#DejaVuSans-41" x="142.484375"/>
     <use xlink:href="#DejaVuSans-20" x="210.892578"/>
     <use xlink:href="#DejaVuSans-71" x="242.679688"/>
     <use xlink:href="#DejaVuSans-2d" x="306.15625"/>
     <use xlink:href="#DejaVuSans-76" x="339.615234"/>
     <use xlink:href="#DejaVuSans-61" x="398.794922"/>
     <use xlink:href="#DejaVuSans-6c" x="460.074219"/>
     <use xlink:href="#DejaVuSans-75" x="487.857422"/>
     <use xlink:href="#DejaVuSans-65" x="551.236328"/>
     <use xlink:href="#DejaVuSans-73" x="612.759766"/>
    </g>
   </g>
  </g>
  <g id="axes_2">
   <g id="patch_7">
    <path d="M 252.392727 228.637091 
L 414.72 228.637091 
L 414.72 120.418909 
L 252.392727 120.418909 
z
" style="fill: #ffffff"/>
   </g>
   <g clip-path="url(#pa403192565)">
    <image xlink:href="data:image/png;base64,
iVBORw0KGgoAAAANSUhEUgAAAOIAAACXCAYAAAAI/4elAAADDUlEQVR4nO3cMYpdBRiG4XNnbiaJoCY3GbRSkCimlRDcgbMKGxutBAVbsbRQcD3uIK0bCCmCKEkxBsnE3LkWLsDqP7wJz7OBj3su7zndv/ns5heHZdo7p+MTy/Z4fOLljevjG0cXL8c3lmVZ9tevrLAx/5/sT47GNy5uzG/MLwD/S4gQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCwPbyw/fGR57fvja/cWv+mO3z3fx767dfvtmMjyzLcu/zn8YPS//97vxPOXoxPrGc350/+uyLCAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQI2Zx98O35odn/7remJ5dcH349fs/3oh5/Hn9XFrf30xLIsy3K0m7/Me7o7H9/4+OYf4xufvPlofMMXEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQK2h7+ejY8cjy+s443H44e+l2t/rvO0zu9cHd/4/dlufOPB2Y/jF96/evj++B/viwgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBECtvsnT8dHNiscMV7D2w//Gd84jJ/L/c/lycn8yOH1eM/fv3plfOP1eFLwihMiBAgRAoQIAUKEACFCgBAhQIgQIEQIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIELBdDofxkc0Kx2zPTr8e/yGXj1c4lLxd5924W2Hj+MXlCivzPv3uy/ENX0QIECIECBEChAgBQoQAIUKAECFAiBAgRAgQIgQIEQKECAFChAAhQoAQIUCIECBECBAiBAgRAoQIAUKEACFCgBAhQIgQ8C9ph0UqsmtEzAAAAABJRU5ErkJggg==" id="image9efbc4eac2" transform="scale(1 -1)translate(0 -108.72)" x="252.392727" y="-119.917091" width="162.72" height="108.72"/>
   </g>
   <g id="matplotlib.axis_3">
    <g id="xtick_6">
     <g id="line2d_9">
      <g>
       <use xlink:href="#m4d6c8ec1bf" x="261.410909" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_10">
      <!-- 0 -->
      <g transform="translate(258.229659 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="xtick_7">
     <g id="line2d_10">
      <g>
       <use xlink:href="#m4d6c8ec1bf" x="297.483636" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_11">
      <!-- 2 -->
      <g transform="translate(294.302386 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="xtick_8">
     <g id="line2d_11">
      <g>
       <use xlink:href="#m4d6c8ec1bf" x="333.556364" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_12">
      <!-- 4 -->
      <g transform="translate(330.375114 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
    <g id="xtick_9">
     <g id="line2d_12">
      <g>
       <use xlink:href="#m4d6c8ec1bf" x="369.629091" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_13">
      <!-- 6 -->
      <g transform="translate(366.447841 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-36"/>
      </g>
     </g>
    </g>
    <g id="xtick_10">
     <g id="line2d_13">
      <g>
       <use xlink:href="#m4d6c8ec1bf" x="405.701818" y="228.637091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_14">
      <!-- 8 -->
      <g transform="translate(402.520568 243.235528)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-38"/>
      </g>
     </g>
    </g>
   </g>
   <g id="matplotlib.axis_4">
    <g id="ytick_4">
     <g id="line2d_14">
      <g>
       <use xlink:href="#m9382a749e1" x="252.392727" y="129.437091" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_15">
      <!-- 0 -->
      <g transform="translate(239.030227 133.23631)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
      </g>
     </g>
    </g>
    <g id="ytick_5">
     <g id="line2d_15">
      <g>
       <use xlink:href="#m9382a749e1" x="252.392727" y="165.509818" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_16">
      <!-- 2 -->
      <g transform="translate(239.030227 169.309037)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="ytick_6">
     <g id="line2d_16">
      <g>
       <use xlink:href="#m9382a749e1" x="252.392727" y="201.582545" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_17">
      <!-- 4 -->
      <g transform="translate(239.030227 205.381764)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
   </g>
   <g id="patch_8">
    <path d="M 252.392727 228.637091 
L 252.392727 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_9">
    <path d="M 414.72 228.637091 
L 414.72 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_10">
    <path d="M 252.392727 228.637091 
L 414.72 228.637091 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_11">
    <path d="M 252.392727 120.418909 
L 414.72 120.418909 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="text_18">
    <!-- MaxEntRL q-values -->
    <g transform="translate(276.455114 114.418909)scale(0.12 -0.12)">
     <defs>
      <path id="DejaVuSans-4d" d="M 628 4666 
L 1569 4666 
L 2759 1491 
L 3956 4666 
L 4897 4666 
L 4897 0 
L 4281 0 
L 4281 4097 
L 3078 897 
L 2444 897 
L 1241 4097 
L 1241 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-78" d="M 3513 3500 
L 2247 1797 
L 3578 0 
L 2900 0 
L 1881 1375 
L 863 0 
L 184 0 
L 1544 1831 
L 300 3500 
L 978 3500 
L 1906 2253 
L 2834 3500 
L 3513 3500 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-45" d="M 628 4666 
L 3578 4666 
L 3578 4134 
L 1259 4134 
L 1259 2753 
L 3481 2753 
L 3481 2222 
L 1259 2222 
L 1259 531 
L 3634 531 
L 3634 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-6e" d="M 3513 2113 
L 3513 0 
L 2938 0 
L 2938 2094 
Q 2938 2591 2744 2837 
Q 2550 3084 2163 3084 
Q 1697 3084 1428 2787 
Q 1159 2491 1159 1978 
L 1159 0 
L 581 0 
L 581 3500 
L 1159 3500 
L 1159 2956 
Q 1366 3272 1645 3428 
Q 1925 3584 2291 3584 
Q 2894 3584 3203 3211 
Q 3513 2838 3513 2113 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-74" d="M 1172 4494 
L 1172 3500 
L 2356 3500 
L 2356 3053 
L 1172 3053 
L 1172 1153 
Q 1172 725 1289 603 
Q 1406 481 1766 481 
L 2356 481 
L 2356 0 
L 1766 0 
Q 1100 0 847 248 
Q 594 497 594 1153 
L 594 3053 
L 172 3053 
L 172 3500 
L 594 3500 
L 594 4494 
L 1172 4494 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-4c" d="M 628 4666 
L 1259 4666 
L 1259 531 
L 3531 531 
L 3531 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
     </defs>
     <use xlink:href="#DejaVuSans-4d"/>
     <use xlink:href="#DejaVuSans-61" x="86.279297"/>
     <use xlink:href="#DejaVuSans-78" x="147.558594"/>
     <use xlink:href="#DejaVuSans-45" x="206.738281"/>
     <use xlink:href="#DejaVuSans-6e" x="269.921875"/>
     <use xlink:href="#DejaVuSans-74" x="333.300781"/>
     <use xlink:href="#DejaVuSans-52" x="372.509766"/>
     <use xlink:href="#DejaVuSans-4c" x="441.992188"/>
     <use xlink:href="#DejaVuSans-20" x="497.705078"/>
     <use xlink:href="#DejaVuSans-71" x="529.492188"/>
     <use xlink:href="#DejaVuSans-2d" x="592.96875"/>
     <use xlink:href="#DejaVuSans-76" x="626.427734"/>
     <use xlink:href="#DejaVuSans-61" x="685.607422"/>
     <use xlink:href="#DejaVuSans-6c" x="746.886719"/>
     <use xlink:href="#DejaVuSans-75" x="774.669922"/>
     <use xlink:href="#DejaVuSans-65" x="838.048828"/>
     <use xlink:href="#DejaVuSans-73" x="899.572266"/>
    </g>
   </g>
  </g>
 </g>
 <defs>
  <clipPath id="p718078bdac">
   <rect x="57.6" y="120.418909" width="162.327273" height="108.218182"/>
  </clipPath>
  <clipPath id="pa403192565">
   <rect x="252.392727" y="120.418909" width="162.327273" height="108.218182"/>
  </clipPath>
 </defs>
</svg>



#### Memory 2AFC task

##### Task schematic

|                | High stakes | Low stakes |
| -------------- | ----------- | ---------- |
| High frequency | set 1       | set 2      |
| Low frequency  | set 3       | set 4      |

Each set has 3 options. Let's call them A, B, and C.


Max entropy RL shows a similar pattern of behavior as DRA. For some reason, I do not have the same plot for DRA, and I need to debug the code. But I know that varying $\lambda, \sigma_\text{base}$ can give all of these curves. DRA, however, has more independent control over the choice accuracy in the different sets owing to the extra parameter.

<?xml version="1.0" encoding="utf-8" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN"
  "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns:xlink="http://www.w3.org/1999/xlink" width="460.8pt" height="345.6pt" viewBox="0 0 460.8 345.6" xmlns="http://www.w3.org/2000/svg" version="1.1">
 <metadata>
  <rdf:RDF xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:cc="http://creativecommons.org/ns#" xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
   <cc:Work>
    <dc:type rdf:resource="http://purl.org/dc/dcmitype/StillImage"/>
    <dc:date>2023-02-14T14:55:06.545995</dc:date>
    <dc:format>image/svg+xml</dc:format>
    <dc:creator>
     <cc:Agent>
      <dc:title>Matplotlib v3.5.1, https://matplotlib.org/</dc:title>
     </cc:Agent>
    </dc:creator>
   </cc:Work>
  </rdf:RDF>
 </metadata>
 <defs>
  <style type="text/css">*{stroke-linejoin: round; stroke-linecap: butt}</style>
 </defs>
 <g id="figure_1">
  <g id="patch_1">
   <path d="M 0 345.6 
L 460.8 345.6 
L 460.8 0 
L 0 0 
z
" style="fill: #ffffff"/>
  </g>
  <g id="axes_1">
   <g id="patch_2">
    <path d="M 57.6 307.584 
L 414.72 307.584 
L 414.72 41.472 
L 57.6 41.472 
z
" style="fill: #eaeaf2"/>
   </g>
   <g id="matplotlib.axis_1">
    <g id="xtick_1">
     <g id="line2d_1">
      <path d="M 73.832727 307.584 
L 73.832727 41.472 
" clip-path="url(#pb3b1e9e224)" style="fill: none; stroke: #ffffff; stroke-linecap: round"/>
     </g>
     <g id="text_1">
      <!-- 1 -->
      <g style="fill: #262626" transform="translate(70.333352 325.442281)scale(0.11 -0.11)">
       <defs>
        <path id="DejaVuSans-31" d="M 794 531 
L 1825 531 
L 1825 4091 
L 703 3866 
L 703 4441 
L 1819 4666 
L 2450 4666 
L 2450 531 
L 3481 531 
L 3481 0 
L 794 0 
L 794 531 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-31"/>
      </g>
     </g>
    </g>
    <g id="xtick_2">
     <g id="line2d_2">
      <path d="M 182.050909 307.584 
L 182.050909 41.472 
" clip-path="url(#pb3b1e9e224)" style="fill: none; stroke: #ffffff; stroke-linecap: round"/>
     </g>
     <g id="text_2">
      <!-- 2 -->
      <g style="fill: #262626" transform="translate(178.551534 325.442281)scale(0.11 -0.11)">
       <defs>
        <path id="DejaVuSans-32" d="M 1228 531 
L 3431 531 
L 3431 0 
L 469 0 
L 469 531 
Q 828 903 1448 1529 
Q 2069 2156 2228 2338 
Q 2531 2678 2651 2914 
Q 2772 3150 2772 3378 
Q 2772 3750 2511 3984 
Q 2250 4219 1831 4219 
Q 1534 4219 1204 4116 
Q 875 4013 500 3803 
L 500 4441 
Q 881 4594 1212 4672 
Q 1544 4750 1819 4750 
Q 2544 4750 2975 4387 
Q 3406 4025 3406 3419 
Q 3406 3131 3298 2873 
Q 3191 2616 2906 2266 
Q 2828 2175 2409 1742 
Q 1991 1309 1228 531 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-32"/>
      </g>
     </g>
    </g>
    <g id="xtick_3">
     <g id="line2d_3">
      <path d="M 290.269091 307.584 
L 290.269091 41.472 
" clip-path="url(#pb3b1e9e224)" style="fill: none; stroke: #ffffff; stroke-linecap: round"/>
     </g>
     <g id="text_3">
      <!-- 3 -->
      <g style="fill: #262626" transform="translate(286.769716 325.442281)scale(0.11 -0.11)">
       <defs>
        <path id="DejaVuSans-33" d="M 2597 2516 
Q 3050 2419 3304 2112 
Q 3559 1806 3559 1356 
Q 3559 666 3084 287 
Q 2609 -91 1734 -91 
Q 1441 -91 1130 -33 
Q 819 25 488 141 
L 488 750 
Q 750 597 1062 519 
Q 1375 441 1716 441 
Q 2309 441 2620 675 
Q 2931 909 2931 1356 
Q 2931 1769 2642 2001 
Q 2353 2234 1838 2234 
L 1294 2234 
L 1294 2753 
L 1863 2753 
Q 2328 2753 2575 2939 
Q 2822 3125 2822 3475 
Q 2822 3834 2567 4026 
Q 2313 4219 1838 4219 
Q 1578 4219 1281 4162 
Q 984 4106 628 3988 
L 628 4550 
Q 988 4650 1302 4700 
Q 1616 4750 1894 4750 
Q 2613 4750 3031 4423 
Q 3450 4097 3450 3541 
Q 3450 3153 3228 2886 
Q 3006 2619 2597 2516 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-33"/>
      </g>
     </g>
    </g>
    <g id="xtick_4">
     <g id="line2d_4">
      <path d="M 398.487273 307.584 
L 398.487273 41.472 
" clip-path="url(#pb3b1e9e224)" style="fill: none; stroke: #ffffff; stroke-linecap: round"/>
     </g>
     <g id="text_4">
      <!-- 4 -->
      <g style="fill: #262626" transform="translate(394.987898 325.442281)scale(0.11 -0.11)">
       <defs>
        <path id="DejaVuSans-34" d="M 2419 4116 
L 825 1625 
L 2419 1625 
L 2419 4116 
z
M 2253 4666 
L 3047 4666 
L 3047 1625 
L 3713 1625 
L 3713 1100 
L 3047 1100 
L 3047 0 
L 2419 0 
L 2419 1100 
L 313 1100 
L 313 1709 
L 2253 4666 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-34"/>
      </g>
     </g>
    </g>
    <g id="text_5">
     <!-- set -->
     <g style="fill: #262626" transform="translate(226.990312 340.848062)scale(0.12 -0.12)">
      <defs>
       <path id="DejaVuSans-73" d="M 2834 3397 
L 2834 2853 
Q 2591 2978 2328 3040 
Q 2066 3103 1784 3103 
Q 1356 3103 1142 2972 
Q 928 2841 928 2578 
Q 928 2378 1081 2264 
Q 1234 2150 1697 2047 
L 1894 2003 
Q 2506 1872 2764 1633 
Q 3022 1394 3022 966 
Q 3022 478 2636 193 
Q 2250 -91 1575 -91 
Q 1294 -91 989 -36 
Q 684 19 347 128 
L 347 722 
Q 666 556 975 473 
Q 1284 391 1588 391 
Q 1994 391 2212 530 
Q 2431 669 2431 922 
Q 2431 1156 2273 1281 
Q 2116 1406 1581 1522 
L 1381 1569 
Q 847 1681 609 1914 
Q 372 2147 372 2553 
Q 372 3047 722 3315 
Q 1072 3584 1716 3584 
Q 2034 3584 2315 3537 
Q 2597 3491 2834 3397 
z
" transform="scale(0.015625)"/>
       <path id="DejaVuSans-65" d="M 3597 1894 
L 3597 1613 
L 953 1613 
Q 991 1019 1311 708 
Q 1631 397 2203 397 
Q 2534 397 2845 478 
Q 3156 559 3463 722 
L 3463 178 
Q 3153 47 2828 -22 
Q 2503 -91 2169 -91 
Q 1331 -91 842 396 
Q 353 884 353 1716 
Q 353 2575 817 3079 
Q 1281 3584 2069 3584 
Q 2775 3584 3186 3129 
Q 3597 2675 3597 1894 
z
M 3022 2063 
Q 3016 2534 2758 2815 
Q 2500 3097 2075 3097 
Q 1594 3097 1305 2825 
Q 1016 2553 972 2059 
L 3022 2063 
z
" transform="scale(0.015625)"/>
       <path id="DejaVuSans-74" d="M 1172 4494 
L 1172 3500 
L 2356 3500 
L 2356 3053 
L 1172 3053 
L 1172 1153 
Q 1172 725 1289 603 
Q 1406 481 1766 481 
L 2356 481 
L 2356 0 
L 1766 0 
Q 1100 0 847 248 
Q 594 497 594 1153 
L 594 3053 
L 172 3053 
L 172 3500 
L 594 3500 
L 594 4494 
L 1172 4494 
z
" transform="scale(0.015625)"/>
      </defs>
      <use xlink:href="#DejaVuSans-73"/>
      <use xlink:href="#DejaVuSans-65" x="52.099609"/>
      <use xlink:href="#DejaVuSans-74" x="113.623047"/>
     </g>
    </g>
   </g>
   <g id="matplotlib.axis_2">
    <g id="ytick_1">
     <g id="line2d_5">
      <path d="M 57.6 275.264671 
L 414.72 275.264671 
" clip-path="url(#pb3b1e9e224)" style="fill: none; stroke: #ffffff; stroke-linecap: round"/>
     </g>
     <g id="text_6">
      <!-- 0.5 -->
      <g style="fill: #262626" transform="translate(30.606563 279.443811)scale(0.11 -0.11)">
       <defs>
        <path id="DejaVuSans-30" d="M 2034 4250 
Q 1547 4250 1301 3770 
Q 1056 3291 1056 2328 
Q 1056 1369 1301 889 
Q 1547 409 2034 409 
Q 2525 409 2770 889 
Q 3016 1369 3016 2328 
Q 3016 3291 2770 3770 
Q 2525 4250 2034 4250 
z
M 2034 4750 
Q 2819 4750 3233 4129 
Q 3647 3509 3647 2328 
Q 3647 1150 3233 529 
Q 2819 -91 2034 -91 
Q 1250 -91 836 529 
Q 422 1150 422 2328 
Q 422 3509 836 4129 
Q 1250 4750 2034 4750 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-2e" d="M 684 794 
L 1344 794 
L 1344 0 
L 684 0 
L 684 794 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-35" d="M 691 4666 
L 3169 4666 
L 3169 4134 
L 1269 4134 
L 1269 2991 
Q 1406 3038 1543 3061 
Q 1681 3084 1819 3084 
Q 2600 3084 3056 2656 
Q 3513 2228 3513 1497 
Q 3513 744 3044 326 
Q 2575 -91 1722 -91 
Q 1428 -91 1123 -41 
Q 819 9 494 109 
L 494 744 
Q 775 591 1075 516 
Q 1375 441 1709 441 
Q 2250 441 2565 725 
Q 2881 1009 2881 1497 
Q 2881 1984 2565 2268 
Q 2250 2553 1709 2553 
Q 1456 2553 1204 2497 
Q 953 2441 691 2322 
L 691 4666 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-30"/>
       <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
       <use xlink:href="#DejaVuSans-35" x="95.410156"/>
      </g>
     </g>
    </g>
    <g id="ytick_2">
     <g id="line2d_6">
      <path d="M 57.6 229.003805 
L 414.72 229.003805 
" clip-path="url(#pb3b1e9e224)" style="fill: none; stroke: #ffffff; stroke-linecap: round"/>
     </g>
     <g id="text_7">
      <!-- 0.6 -->
      <g style="fill: #262626" transform="translate(30.606563 233.182946)scale(0.11 -0.11)">
       <defs>
        <path id="DejaVuSans-36" d="M 2113 2584 
Q 1688 2584 1439 2293 
Q 1191 2003 1191 1497 
Q 1191 994 1439 701 
Q 1688 409 2113 409 
Q 2538 409 2786 701 
Q 3034 994 3034 1497 
Q 3034 2003 2786 2293 
Q 2538 2584 2113 2584 
z
M 3366 4563 
L 3366 3988 
Q 3128 4100 2886 4159 
Q 2644 4219 2406 4219 
Q 1781 4219 1451 3797 
Q 1122 3375 1075 2522 
Q 1259 2794 1537 2939 
Q 1816 3084 2150 3084 
Q 2853 3084 3261 2657 
Q 3669 2231 3669 1497 
Q 3669 778 3244 343 
Q 2819 -91 2113 -91 
Q 1303 -91 875 529 
Q 447 1150 447 2328 
Q 447 3434 972 4092 
Q 1497 4750 2381 4750 
Q 2619 4750 2861 4703 
Q 3103 4656 3366 4563 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-30"/>
       <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
       <use xlink:href="#DejaVuSans-36" x="95.410156"/>
      </g>
     </g>
    </g>
    <g id="ytick_3">
     <g id="line2d_7">
      <path d="M 57.6 182.74294 
L 414.72 182.74294 
" clip-path="url(#pb3b1e9e224)" style="fill: none; stroke: #ffffff; stroke-linecap: round"/>
     </g>
     <g id="text_8">
      <!-- 0.7 -->
      <g style="fill: #262626" transform="translate(30.606563 186.922081)scale(0.11 -0.11)">
       <defs>
        <path id="DejaVuSans-37" d="M 525 4666 
L 3525 4666 
L 3525 4397 
L 1831 0 
L 1172 0 
L 2766 4134 
L 525 4134 
L 525 4666 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-30"/>
       <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
       <use xlink:href="#DejaVuSans-37" x="95.410156"/>
      </g>
     </g>
    </g>
    <g id="ytick_4">
     <g id="line2d_8">
      <path d="M 57.6 136.482075 
L 414.72 136.482075 
" clip-path="url(#pb3b1e9e224)" style="fill: none; stroke: #ffffff; stroke-linecap: round"/>
     </g>
     <g id="text_9">
      <!-- 0.8 -->
      <g style="fill: #262626" transform="translate(30.606563 140.661215)scale(0.11 -0.11)">
       <defs>
        <path id="DejaVuSans-38" d="M 2034 2216 
Q 1584 2216 1326 1975 
Q 1069 1734 1069 1313 
Q 1069 891 1326 650 
Q 1584 409 2034 409 
Q 2484 409 2743 651 
Q 3003 894 3003 1313 
Q 3003 1734 2745 1975 
Q 2488 2216 2034 2216 
z
M 1403 2484 
Q 997 2584 770 2862 
Q 544 3141 544 3541 
Q 544 4100 942 4425 
Q 1341 4750 2034 4750 
Q 2731 4750 3128 4425 
Q 3525 4100 3525 3541 
Q 3525 3141 3298 2862 
Q 3072 2584 2669 2484 
Q 3125 2378 3379 2068 
Q 3634 1759 3634 1313 
Q 3634 634 3220 271 
Q 2806 -91 2034 -91 
Q 1263 -91 848 271 
Q 434 634 434 1313 
Q 434 1759 690 2068 
Q 947 2378 1403 2484 
z
M 1172 3481 
Q 1172 3119 1398 2916 
Q 1625 2713 2034 2713 
Q 2441 2713 2670 2916 
Q 2900 3119 2900 3481 
Q 2900 3844 2670 4047 
Q 2441 4250 2034 4250 
Q 1625 4250 1398 4047 
Q 1172 3844 1172 3481 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-30"/>
       <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
       <use xlink:href="#DejaVuSans-38" x="95.410156"/>
      </g>
     </g>
    </g>
    <g id="ytick_5">
     <g id="line2d_9">
      <path d="M 57.6 90.221209 
L 414.72 90.221209 
" clip-path="url(#pb3b1e9e224)" style="fill: none; stroke: #ffffff; stroke-linecap: round"/>
     </g>
     <g id="text_10">
      <!-- 0.9 -->
      <g style="fill: #262626" transform="translate(30.606563 94.40035)scale(0.11 -0.11)">
       <defs>
        <path id="DejaVuSans-39" d="M 703 97 
L 703 672 
Q 941 559 1184 500 
Q 1428 441 1663 441 
Q 2288 441 2617 861 
Q 2947 1281 2994 2138 
Q 2813 1869 2534 1725 
Q 2256 1581 1919 1581 
Q 1219 1581 811 2004 
Q 403 2428 403 3163 
Q 403 3881 828 4315 
Q 1253 4750 1959 4750 
Q 2769 4750 3195 4129 
Q 3622 3509 3622 2328 
Q 3622 1225 3098 567 
Q 2575 -91 1691 -91 
Q 1453 -91 1209 -44 
Q 966 3 703 97 
z
M 1959 2075 
Q 2384 2075 2632 2365 
Q 2881 2656 2881 3163 
Q 2881 3666 2632 3958 
Q 2384 4250 1959 4250 
Q 1534 4250 1286 3958 
Q 1038 3666 1038 3163 
Q 1038 2656 1286 2365 
Q 1534 2075 1959 2075 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-30"/>
       <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
       <use xlink:href="#DejaVuSans-39" x="95.410156"/>
      </g>
     </g>
    </g>
    <g id="ytick_6">
     <g id="line2d_10">
      <path d="M 57.6 43.960344 
L 414.72 43.960344 
" clip-path="url(#pb3b1e9e224)" style="fill: none; stroke: #ffffff; stroke-linecap: round"/>
     </g>
     <g id="text_11">
      <!-- 1.0 -->
      <g style="fill: #262626" transform="translate(30.606563 48.139484)scale(0.11 -0.11)">
       <use xlink:href="#DejaVuSans-31"/>
       <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
       <use xlink:href="#DejaVuSans-30" x="95.410156"/>
      </g>
     </g>
    </g>
    <g id="text_12">
     <!-- Choice accuracy -->
     <g style="fill: #262626" transform="translate(24.110937 223.827375)rotate(-90)scale(0.12 -0.12)">
      <defs>
       <path id="DejaVuSans-43" d="M 4122 4306 
L 4122 3641 
Q 3803 3938 3442 4084 
Q 3081 4231 2675 4231 
Q 1875 4231 1450 3742 
Q 1025 3253 1025 2328 
Q 1025 1406 1450 917 
Q 1875 428 2675 428 
Q 3081 428 3442 575 
Q 3803 722 4122 1019 
L 4122 359 
Q 3791 134 3420 21 
Q 3050 -91 2638 -91 
Q 1578 -91 968 557 
Q 359 1206 359 2328 
Q 359 3453 968 4101 
Q 1578 4750 2638 4750 
Q 3056 4750 3426 4639 
Q 3797 4528 4122 4306 
z
" transform="scale(0.015625)"/>
       <path id="DejaVuSans-68" d="M 3513 2113 
L 3513 0 
L 2938 0 
L 2938 2094 
Q 2938 2591 2744 2837 
Q 2550 3084 2163 3084 
Q 1697 3084 1428 2787 
Q 1159 2491 1159 1978 
L 1159 0 
L 581 0 
L 581 4863 
L 1159 4863 
L 1159 2956 
Q 1366 3272 1645 3428 
Q 1925 3584 2291 3584 
Q 2894 3584 3203 3211 
Q 3513 2838 3513 2113 
z
" transform="scale(0.015625)"/>
       <path id="DejaVuSans-6f" d="M 1959 3097 
Q 1497 3097 1228 2736 
Q 959 2375 959 1747 
Q 959 1119 1226 758 
Q 1494 397 1959 397 
Q 2419 397 2687 759 
Q 2956 1122 2956 1747 
Q 2956 2369 2687 2733 
Q 2419 3097 1959 3097 
z
M 1959 3584 
Q 2709 3584 3137 3096 
Q 3566 2609 3566 1747 
Q 3566 888 3137 398 
Q 2709 -91 1959 -91 
Q 1206 -91 779 398 
Q 353 888 353 1747 
Q 353 2609 779 3096 
Q 1206 3584 1959 3584 
z
" transform="scale(0.015625)"/>
       <path id="DejaVuSans-69" d="M 603 3500 
L 1178 3500 
L 1178 0 
L 603 0 
L 603 3500 
z
M 603 4863 
L 1178 4863 
L 1178 4134 
L 603 4134 
L 603 4863 
z
" transform="scale(0.015625)"/>
       <path id="DejaVuSans-63" d="M 3122 3366 
L 3122 2828 
Q 2878 2963 2633 3030 
Q 2388 3097 2138 3097 
Q 1578 3097 1268 2742 
Q 959 2388 959 1747 
Q 959 1106 1268 751 
Q 1578 397 2138 397 
Q 2388 397 2633 464 
Q 2878 531 3122 666 
L 3122 134 
Q 2881 22 2623 -34 
Q 2366 -91 2075 -91 
Q 1284 -91 818 406 
Q 353 903 353 1747 
Q 353 2603 823 3093 
Q 1294 3584 2113 3584 
Q 2378 3584 2631 3529 
Q 2884 3475 3122 3366 
z
" transform="scale(0.015625)"/>
       <path id="DejaVuSans-20" transform="scale(0.015625)"/>
       <path id="DejaVuSans-61" d="M 2194 1759 
Q 1497 1759 1228 1600 
Q 959 1441 959 1056 
Q 959 750 1161 570 
Q 1363 391 1709 391 
Q 2188 391 2477 730 
Q 2766 1069 2766 1631 
L 2766 1759 
L 2194 1759 
z
M 3341 1997 
L 3341 0 
L 2766 0 
L 2766 531 
Q 2569 213 2275 61 
Q 1981 -91 1556 -91 
Q 1019 -91 701 211 
Q 384 513 384 1019 
Q 384 1609 779 1909 
Q 1175 2209 1959 2209 
L 2766 2209 
L 2766 2266 
Q 2766 2663 2505 2880 
Q 2244 3097 1772 3097 
Q 1472 3097 1187 3025 
Q 903 2953 641 2809 
L 641 3341 
Q 956 3463 1253 3523 
Q 1550 3584 1831 3584 
Q 2591 3584 2966 3190 
Q 3341 2797 3341 1997 
z
" transform="scale(0.015625)"/>
       <path id="DejaVuSans-75" d="M 544 1381 
L 544 3500 
L 1119 3500 
L 1119 1403 
Q 1119 906 1312 657 
Q 1506 409 1894 409 
Q 2359 409 2629 706 
Q 2900 1003 2900 1516 
L 2900 3500 
L 3475 3500 
L 3475 0 
L 2900 0 
L 2900 538 
Q 2691 219 2414 64 
Q 2138 -91 1772 -91 
Q 1169 -91 856 284 
Q 544 659 544 1381 
z
M 1991 3584 
L 1991 3584 
z
" transform="scale(0.015625)"/>
       <path id="DejaVuSans-72" d="M 2631 2963 
Q 2534 3019 2420 3045 
Q 2306 3072 2169 3072 
Q 1681 3072 1420 2755 
Q 1159 2438 1159 1844 
L 1159 0 
L 581 0 
L 581 3500 
L 1159 3500 
L 1159 2956 
Q 1341 3275 1631 3429 
Q 1922 3584 2338 3584 
Q 2397 3584 2469 3576 
Q 2541 3569 2628 3553 
L 2631 2963 
z
" transform="scale(0.015625)"/>
       <path id="DejaVuSans-79" d="M 2059 -325 
Q 1816 -950 1584 -1140 
Q 1353 -1331 966 -1331 
L 506 -1331 
L 506 -850 
L 844 -850 
Q 1081 -850 1212 -737 
Q 1344 -625 1503 -206 
L 1606 56 
L 191 3500 
L 800 3500 
L 1894 763 
L 2988 3500 
L 3597 3500 
L 2059 -325 
z
" transform="scale(0.015625)"/>
      </defs>
      <use xlink:href="#DejaVuSans-43"/>
      <use xlink:href="#DejaVuSans-68" x="69.824219"/>
      <use xlink:href="#DejaVuSans-6f" x="133.203125"/>
      <use xlink:href="#DejaVuSans-69" x="194.384766"/>
      <use xlink:href="#DejaVuSans-63" x="222.167969"/>
      <use xlink:href="#DejaVuSans-65" x="277.148438"/>
      <use xlink:href="#DejaVuSans-20" x="338.671875"/>
      <use xlink:href="#DejaVuSans-61" x="370.458984"/>
      <use xlink:href="#DejaVuSans-63" x="431.738281"/>
      <use xlink:href="#DejaVuSans-63" x="486.71875"/>
      <use xlink:href="#DejaVuSans-75" x="541.699219"/>
      <use xlink:href="#DejaVuSans-72" x="605.078125"/>
      <use xlink:href="#DejaVuSans-61" x="646.191406"/>
      <use xlink:href="#DejaVuSans-63" x="707.470703"/>
      <use xlink:href="#DejaVuSans-79" x="762.451172"/>
     </g>
    </g>
   </g>
   <g id="PolyCollection_1">
    <defs>
     <path id="md21105d8de" d="M 73.832727 -292.032 
L 73.832727 -287.348268 
L 182.050909 -281.460576 
L 290.269091 -252.345292 
L 398.487273 -243.487052 
L 398.487273 -259.507721 
L 398.487273 -259.507721 
L 290.269091 -267.934108 
L 182.050909 -286.747789 
L 73.832727 -292.032 
z
" style="stroke: #edd1cb; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#pb3b1e9e224)">
     <use xlink:href="#md21105d8de" x="0" y="345.6" style="fill: #edd1cb; fill-opacity: 0.2; stroke: #edd1cb; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_2">
    <defs>
     <path id="mfe2572a741" d="M 73.832727 -289.269799 
L 73.832727 -284.225779 
L 182.050909 -243.39324 
L 290.269091 -245.182862 
L 398.487273 -201.786969 
L 398.487273 -223.695575 
L 398.487273 -223.695575 
L 290.269091 -261.192998 
L 182.050909 -252.520513 
L 73.832727 -289.269799 
z
" style="stroke: #eac8c4; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#pb3b1e9e224)">
     <use xlink:href="#mfe2572a741" x="0" y="345.6" style="fill: #eac8c4; fill-opacity: 0.2; stroke: #eac8c4; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_3">
    <defs>
     <path id="m1b0a02bdb5" d="M 73.832727 -283.024822 
L 73.832727 -277.017035 
L 182.050909 -186.107588 
L 290.269091 -235.492517 
L 398.487273 -158.391075 
L 398.487273 -181.995492 
L 398.487273 -181.995492 
L 290.269091 -253.60925 
L 182.050909 -198.480448 
L 73.832727 -283.024822 
z
" style="stroke: #e6bdbc; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#pb3b1e9e224)">
     <use xlink:href="#m1b0a02bdb5" x="0" y="345.6" style="fill: #e6bdbc; fill-opacity: 0.2; stroke: #e6bdbc; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_4">
    <defs>
     <path id="ma1c5e89a04" d="M 73.832727 -236.067402 
L 73.832727 -225.97636 
L 182.050909 -113.0894 
L 290.269091 -189.990027 
L 398.487273 -100.670323 
L 398.487273 -128.487933 
L 398.487273 -128.487933 
L 290.269091 -211.477314 
L 182.050909 -127.260693 
L 73.832727 -236.067402 
z
" style="stroke: #d59ba8; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#pb3b1e9e224)">
     <use xlink:href="#ma1c5e89a04" x="0" y="345.6" style="fill: #d59ba8; fill-opacity: 0.2; stroke: #d59ba8; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_5">
    <defs>
     <path id="m909d0e13e2" d="M 73.832727 -187.308546 
L 73.832727 -174.095016 
L 182.050909 -84.987005 
L 290.269091 -138.589065 
L 398.487273 -69.071371 
L 398.487273 -96.457129 
L 398.487273 -96.457129 
L 290.269091 -163.868227 
L 182.050909 -98.924112 
L 73.832727 -187.308546 
z
" style="stroke: #ab6990; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#pb3b1e9e224)">
     <use xlink:href="#m909d0e13e2" x="0" y="345.6" style="fill: #ab6990; fill-opacity: 0.2; stroke: #ab6990; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_6">
    <defs>
     <path id="m7700a117ae" d="M 73.832727 -131.467045 
L 73.832727 -117.052558 
L 182.050909 -79.942986 
L 290.269091 -109.939349 
L 398.487273 -50.112 
L 398.487273 -76.2338 
L 398.487273 -76.2338 
L 290.269091 -136.482468 
L 182.050909 -94.357472 
L 73.832727 -131.467045 
z
" style="stroke: #2d1e3e; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#pb3b1e9e224)">
     <use xlink:href="#m7700a117ae" x="0" y="345.6" style="fill: #2d1e3e; fill-opacity: 0.2; stroke: #2d1e3e; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="line2d_11">
    <path d="M 73.832727 55.849818 
L 182.050909 61.494317 
L 290.269091 86.092279 
L 398.487273 94.518667 
" clip-path="url(#pb3b1e9e224)" style="fill: none; stroke: #edd1cb; stroke-width: 1.5; stroke-linecap: round"/>
   </g>
   <g id="line2d_12">
    <path d="M 73.832727 58.852211 
L 182.050909 97.523028 
L 290.269091 91.99075 
L 398.487273 132.858728 
" clip-path="url(#pb3b1e9e224)" style="fill: none; stroke: #eac8c4; stroke-width: 1.5; stroke-linecap: round"/>
   </g>
   <g id="line2d_13">
    <path d="M 73.832727 65.457475 
L 182.050909 152.887148 
L 290.269091 100.838457 
L 398.487273 174.569344 
" clip-path="url(#pb3b1e9e224)" style="fill: none; stroke: #e6bdbc; stroke-width: 1.5; stroke-linecap: round"/>
   </g>
   <g id="line2d_14">
    <path d="M 73.832727 114.456522 
L 182.050909 225.665145 
L 290.269091 144.234351 
L 398.487273 231.447458 
" clip-path="url(#pb3b1e9e224)" style="fill: none; stroke: #d59ba8; stroke-width: 1.5; stroke-linecap: round"/>
   </g>
   <g id="line2d_15">
    <path d="M 73.832727 164.896718 
L 182.050909 253.76754 
L 290.269091 193.528716 
L 398.487273 262.203771 
" clip-path="url(#pb3b1e9e224)" style="fill: none; stroke: #ab6990; stroke-width: 1.5; stroke-linecap: round"/>
   </g>
   <g id="line2d_16">
    <path d="M 73.832727 221.221604 
L 182.050909 258.211081 
L 290.269091 221.757112 
L 398.487273 282.4271 
" clip-path="url(#pb3b1e9e224)" style="fill: none; stroke: #2d1e3e; stroke-width: 1.5; stroke-linecap: round"/>
   </g>
   <g id="line2d_17"/>
   <g id="line2d_18"/>
   <g id="line2d_19"/>
   <g id="line2d_20"/>
   <g id="line2d_21"/>
   <g id="line2d_22"/>
   <g id="patch_3">
    <path d="M 57.6 307.584 
L 57.6 41.472 
" style="fill: none; stroke: #ffffff; stroke-width: 1.25; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_4">
    <path d="M 414.72 307.584 
L 414.72 41.472 
" style="fill: none; stroke: #ffffff; stroke-width: 1.25; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_5">
    <path d="M 57.6 307.584 
L 414.72 307.584 
" style="fill: none; stroke: #ffffff; stroke-width: 1.25; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_6">
    <path d="M 57.6 41.472 
L 414.72 41.472 
" style="fill: none; stroke: #ffffff; stroke-width: 1.25; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="text_13">
    <!-- MaxEntRL all trials; all runs combined -->
    <g style="fill: #262626" transform="translate(122.97375 35.472)scale(0.12 -0.12)">
     <defs>
      <path id="DejaVuSans-4d" d="M 628 4666 
L 1569 4666 
L 2759 1491 
L 3956 4666 
L 4897 4666 
L 4897 0 
L 4281 0 
L 4281 4097 
L 3078 897 
L 2444 897 
L 1241 4097 
L 1241 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-78" d="M 3513 3500 
L 2247 1797 
L 3578 0 
L 2900 0 
L 1881 1375 
L 863 0 
L 184 0 
L 1544 1831 
L 300 3500 
L 978 3500 
L 1906 2253 
L 2834 3500 
L 3513 3500 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-45" d="M 628 4666 
L 3578 4666 
L 3578 4134 
L 1259 4134 
L 1259 2753 
L 3481 2753 
L 3481 2222 
L 1259 2222 
L 1259 531 
L 3634 531 
L 3634 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-6e" d="M 3513 2113 
L 3513 0 
L 2938 0 
L 2938 2094 
Q 2938 2591 2744 2837 
Q 2550 3084 2163 3084 
Q 1697 3084 1428 2787 
Q 1159 2491 1159 1978 
L 1159 0 
L 581 0 
L 581 3500 
L 1159 3500 
L 1159 2956 
Q 1366 3272 1645 3428 
Q 1925 3584 2291 3584 
Q 2894 3584 3203 3211 
Q 3513 2838 3513 2113 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-52" d="M 2841 2188 
Q 3044 2119 3236 1894 
Q 3428 1669 3622 1275 
L 4263 0 
L 3584 0 
L 2988 1197 
Q 2756 1666 2539 1819 
Q 2322 1972 1947 1972 
L 1259 1972 
L 1259 0 
L 628 0 
L 628 4666 
L 2053 4666 
Q 2853 4666 3247 4331 
Q 3641 3997 3641 3322 
Q 3641 2881 3436 2590 
Q 3231 2300 2841 2188 
z
M 1259 4147 
L 1259 2491 
L 2053 2491 
Q 2509 2491 2742 2702 
Q 2975 2913 2975 3322 
Q 2975 3731 2742 3939 
Q 2509 4147 2053 4147 
L 1259 4147 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-4c" d="M 628 4666 
L 1259 4666 
L 1259 531 
L 3531 531 
L 3531 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-6c" d="M 603 4863 
L 1178 4863 
L 1178 0 
L 603 0 
L 603 4863 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-3b" d="M 750 3309 
L 1409 3309 
L 1409 2516 
L 750 2516 
L 750 3309 
z
M 750 794 
L 1409 794 
L 1409 256 
L 897 -744 
L 494 -744 
L 750 256 
L 750 794 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-6d" d="M 3328 2828 
Q 3544 3216 3844 3400 
Q 4144 3584 4550 3584 
Q 5097 3584 5394 3201 
Q 5691 2819 5691 2113 
L 5691 0 
L 5113 0 
L 5113 2094 
Q 5113 2597 4934 2840 
Q 4756 3084 4391 3084 
Q 3944 3084 3684 2787 
Q 3425 2491 3425 1978 
L 3425 0 
L 2847 0 
L 2847 2094 
Q 2847 2600 2669 2842 
Q 2491 3084 2119 3084 
Q 1678 3084 1418 2786 
Q 1159 2488 1159 1978 
L 1159 0 
L 581 0 
L 581 3500 
L 1159 3500 
L 1159 2956 
Q 1356 3278 1631 3431 
Q 1906 3584 2284 3584 
Q 2666 3584 2933 3390 
Q 3200 3197 3328 2828 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-62" d="M 3116 1747 
Q 3116 2381 2855 2742 
Q 2594 3103 2138 3103 
Q 1681 3103 1420 2742 
Q 1159 2381 1159 1747 
Q 1159 1113 1420 752 
Q 1681 391 2138 391 
Q 2594 391 2855 752 
Q 3116 1113 3116 1747 
z
M 1159 2969 
Q 1341 3281 1617 3432 
Q 1894 3584 2278 3584 
Q 2916 3584 3314 3078 
Q 3713 2572 3713 1747 
Q 3713 922 3314 415 
Q 2916 -91 2278 -91 
Q 1894 -91 1617 61 
Q 1341 213 1159 525 
L 1159 0 
L 581 0 
L 581 4863 
L 1159 4863 
L 1159 2969 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-64" d="M 2906 2969 
L 2906 4863 
L 3481 4863 
L 3481 0 
L 2906 0 
L 2906 525 
Q 2725 213 2448 61 
Q 2172 -91 1784 -91 
Q 1150 -91 751 415 
Q 353 922 353 1747 
Q 353 2572 751 3078 
Q 1150 3584 1784 3584 
Q 2172 3584 2448 3432 
Q 2725 3281 2906 2969 
z
M 947 1747 
Q 947 1113 1208 752 
Q 1469 391 1925 391 
Q 2381 391 2643 752 
Q 2906 1113 2906 1747 
Q 2906 2381 2643 2742 
Q 2381 3103 1925 3103 
Q 1469 3103 1208 2742 
Q 947 2381 947 1747 
z
" transform="scale(0.015625)"/>
     </defs>
     <use xlink:href="#DejaVuSans-4d"/>
     <use xlink:href="#DejaVuSans-61" x="86.279297"/>
     <use xlink:href="#DejaVuSans-78" x="147.558594"/>
     <use xlink:href="#DejaVuSans-45" x="206.738281"/>
     <use xlink:href="#DejaVuSans-6e" x="269.921875"/>
     <use xlink:href="#DejaVuSans-74" x="333.300781"/>
     <use xlink:href="#DejaVuSans-52" x="372.509766"/>
     <use xlink:href="#DejaVuSans-4c" x="441.992188"/>
     <use xlink:href="#DejaVuSans-20" x="497.705078"/>
     <use xlink:href="#DejaVuSans-61" x="529.492188"/>
     <use xlink:href="#DejaVuSans-6c" x="590.771484"/>
     <use xlink:href="#DejaVuSans-6c" x="618.554688"/>
     <use xlink:href="#DejaVuSans-20" x="646.337891"/>
     <use xlink:href="#DejaVuSans-74" x="678.125"/>
     <use xlink:href="#DejaVuSans-72" x="717.333984"/>
     <use xlink:href="#DejaVuSans-69" x="758.447266"/>
     <use xlink:href="#DejaVuSans-61" x="786.230469"/>
     <use xlink:href="#DejaVuSans-6c" x="847.509766"/>
     <use xlink:href="#DejaVuSans-73" x="875.292969"/>
     <use xlink:href="#DejaVuSans-3b" x="927.392578"/>
     <use xlink:href="#DejaVuSans-20" x="961.083984"/>
     <use xlink:href="#DejaVuSans-61" x="992.871094"/>
     <use xlink:href="#DejaVuSans-6c" x="1054.150391"/>
     <use xlink:href="#DejaVuSans-6c" x="1081.933594"/>
     <use xlink:href="#DejaVuSans-20" x="1109.716797"/>
     <use xlink:href="#DejaVuSans-72" x="1141.503906"/>
     <use xlink:href="#DejaVuSans-75" x="1182.617188"/>
     <use xlink:href="#DejaVuSans-6e" x="1245.996094"/>
     <use xlink:href="#DejaVuSans-73" x="1309.375"/>
     <use xlink:href="#DejaVuSans-20" x="1361.474609"/>
     <use xlink:href="#DejaVuSans-63" x="1393.261719"/>
     <use xlink:href="#DejaVuSans-6f" x="1448.242188"/>
     <use xlink:href="#DejaVuSans-6d" x="1509.423828"/>
     <use xlink:href="#DejaVuSans-62" x="1606.835938"/>
     <use xlink:href="#DejaVuSans-69" x="1670.3125"/>
     <use xlink:href="#DejaVuSans-6e" x="1698.095703"/>
     <use xlink:href="#DejaVuSans-65" x="1761.474609"/>
     <use xlink:href="#DejaVuSans-64" x="1822.998047"/>
    </g>
   </g>
   <g id="legend_1">
    <g id="patch_7">
     <path d="M 65.3 302.084 
L 124.992187 302.084 
Q 127.192187 302.084 127.192187 299.884 
L 127.192187 186.994625 
Q 127.192187 184.794625 124.992187 184.794625 
L 65.3 184.794625 
Q 63.1 184.794625 63.1 186.994625 
L 63.1 299.884 
Q 63.1 302.084 65.3 302.084 
z
" style="fill: #eaeaf2; opacity: 0.8; stroke: #cccccc; stroke-linejoin: miter"/>
    </g>
    <g id="text_14">
     <!-- alpha -->
     <g style="fill: #262626" transform="translate(78.513906 198.31275)scale(0.12 -0.12)">
      <defs>
       <path id="DejaVuSans-70" d="M 1159 525 
L 1159 -1331 
L 581 -1331 
L 581 3500 
L 1159 3500 
L 1159 2969 
Q 1341 3281 1617 3432 
Q 1894 3584 2278 3584 
Q 2916 3584 3314 3078 
Q 3713 2572 3713 1747 
Q 3713 922 3314 415 
Q 2916 -91 2278 -91 
Q 1894 -91 1617 61 
Q 1341 213 1159 525 
z
M 3116 1747 
Q 3116 2381 2855 2742 
Q 2594 3103 2138 3103 
Q 1681 3103 1420 2742 
Q 1159 2381 1159 1747 
Q 1159 1113 1420 752 
Q 1681 391 2138 391 
Q 2594 391 2855 752 
Q 3116 1113 3116 1747 
z
" transform="scale(0.015625)"/>
      </defs>
      <use xlink:href="#DejaVuSans-61"/>
      <use xlink:href="#DejaVuSans-6c" x="61.279297"/>
      <use xlink:href="#DejaVuSans-70" x="89.0625"/>
      <use xlink:href="#DejaVuSans-68" x="152.539062"/>
      <use xlink:href="#DejaVuSans-61" x="215.917969"/>
     </g>
    </g>
    <g id="line2d_23">
     <path d="M 67.5 210.816656 
L 78.5 210.816656 
L 89.5 210.816656 
" style="fill: none; stroke: #edd1cb; stroke-width: 1.5; stroke-linecap: round"/>
    </g>
    <g id="text_15">
     <!-- 0.1 -->
     <g style="fill: #262626" transform="translate(98.3 214.666656)scale(0.11 -0.11)">
      <use xlink:href="#DejaVuSans-30"/>
      <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
      <use xlink:href="#DejaVuSans-31" x="95.410156"/>
     </g>
    </g>
    <g id="line2d_24">
     <path d="M 67.5 226.962594 
L 78.5 226.962594 
L 89.5 226.962594 
" style="fill: none; stroke: #eac8c4; stroke-width: 1.5; stroke-linecap: round"/>
    </g>
    <g id="text_16">
     <!-- 0.5 -->
     <g style="fill: #262626" transform="translate(98.3 230.812594)scale(0.11 -0.11)">
      <use xlink:href="#DejaVuSans-30"/>
      <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
      <use xlink:href="#DejaVuSans-35" x="95.410156"/>
     </g>
    </g>
    <g id="line2d_25">
     <path d="M 67.5 243.108531 
L 78.5 243.108531 
L 89.5 243.108531 
" style="fill: none; stroke: #e6bdbc; stroke-width: 1.5; stroke-linecap: round"/>
    </g>
    <g id="text_17">
     <!-- 1.0 -->
     <g style="fill: #262626" transform="translate(98.3 246.958531)scale(0.11 -0.11)">
      <use xlink:href="#DejaVuSans-31"/>
      <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
      <use xlink:href="#DejaVuSans-30" x="95.410156"/>
     </g>
    </g>
    <g id="line2d_26">
     <path d="M 67.5 259.254469 
L 78.5 259.254469 
L 89.5 259.254469 
" style="fill: none; stroke: #d59ba8; stroke-width: 1.5; stroke-linecap: round"/>
    </g>
    <g id="text_18">
     <!-- 2.5 -->
     <g style="fill: #262626" transform="translate(98.3 263.104469)scale(0.11 -0.11)">
      <use xlink:href="#DejaVuSans-32"/>
      <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
      <use xlink:href="#DejaVuSans-35" x="95.410156"/>
     </g>
    </g>
    <g id="line2d_27">
     <path d="M 67.5 275.400406 
L 78.5 275.400406 
L 89.5 275.400406 
" style="fill: none; stroke: #ab6990; stroke-width: 1.5; stroke-linecap: round"/>
    </g>
    <g id="text_19">
     <!-- 5.0 -->
     <g style="fill: #262626" transform="translate(98.3 279.250406)scale(0.11 -0.11)">
      <use xlink:href="#DejaVuSans-35"/>
      <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
      <use xlink:href="#DejaVuSans-30" x="95.410156"/>
     </g>
    </g>
    <g id="line2d_28">
     <path d="M 67.5 291.546344 
L 78.5 291.546344 
L 89.5 291.546344 
" style="fill: none; stroke: #2d1e3e; stroke-width: 1.5; stroke-linecap: round"/>
    </g>
    <g id="text_20">
     <!-- 10.0 -->
     <g style="fill: #262626" transform="translate(98.3 295.396344)scale(0.11 -0.11)">
      <use xlink:href="#DejaVuSans-31"/>
      <use xlink:href="#DejaVuSans-30" x="63.623047"/>
      <use xlink:href="#DejaVuSans-2e" x="127.246094"/>
      <use xlink:href="#DejaVuSans-30" x="159.033203"/>
     </g>
    </g>
   </g>
  </g>
 </g>
 <defs>
  <clipPath id="pb3b1e9e224">
   <rect x="57.6" y="41.472" width="357.12" height="266.112"/>
  </clipPath>
 </defs>
</svg>


This also looks qualitatively similar to the pilot data we recorded, although the models are not really distinguishable from frequency/stakes if we were to just look at the regular trials.

Data from training sessions:
![Pasted image 20230214154342.png](/img/user/images/Pasted%20image%2020230214154342.png)

Data from test sessions:
![Pasted image 20230214154441.png](/img/user/images/Pasted%20image%2020230214154441.png)

#### Bottleneck task

##### Task schematic

![Pasted image 20230214171443.png](/img/user/images/Pasted%20image%2020230214171443.png)

The orange transitions are stochastic. On any given trial, only two of the four choice options are available with their probabilities carefully chosen to adapt the experienced frequency of the states in color. If agents are behaving optimally, the top branches are visited 80% of the time and the bottom branches are visited 20% of the time.

##### Max-entropy RL

<?xml version="1.0" encoding="utf-8" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN"
  "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns:xlink="http://www.w3.org/1999/xlink" width="460.8pt" height="345.6pt" viewBox="0 0 460.8 345.6" xmlns="http://www.w3.org/2000/svg" version="1.1">
 <metadata>
  <rdf:RDF xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:cc="http://creativecommons.org/ns#" xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
   <cc:Work>
    <dc:type rdf:resource="http://purl.org/dc/dcmitype/StillImage"/>
    <dc:date>2023-02-14T19:03:44.338823</dc:date>
    <dc:format>image/svg+xml</dc:format>
    <dc:creator>
     <cc:Agent>
      <dc:title>Matplotlib v3.5.1, https://matplotlib.org/</dc:title>
     </cc:Agent>
    </dc:creator>
   </cc:Work>
  </rdf:RDF>
 </metadata>
 <defs>
  <style type="text/css">*{stroke-linejoin: round; stroke-linecap: butt}</style>
 </defs>
 <g id="figure_1">
  <g id="patch_1">
   <path d="M 0 345.6 
L 460.8 345.6 
L 460.8 0 
L 0 0 
z
" style="fill: #ffffff"/>
  </g>
  <g id="axes_1">
   <g id="patch_2">
    <path d="M 57.6 307.584 
L 414.72 307.584 
L 414.72 41.472 
L 57.6 41.472 
z
" style="fill: #ffffff"/>
   </g>
   <g id="PolyCollection_1">
    <defs>
     <path id="m487221ce62" d="M 73.832727 -292.032 
L 73.832727 -289.364477 
L 182.050909 -269.022643 
L 290.269091 -286.586547 
L 398.487273 -79.954565 
L 398.487273 -246.249931 
L 398.487273 -246.249931 
L 290.269091 -290.518604 
L 182.050909 -286.341074 
L 73.832727 -292.032 
z
" style="stroke: #edd1cb; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#p2c189dfd22)">
     <use xlink:href="#m487221ce62" x="0" y="345.6" style="fill: #edd1cb; fill-opacity: 0.2; stroke: #edd1cb; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_2">
    <defs>
     <path id="mbdd47da42e" d="M 73.832727 -268.517627 
L 73.832727 -258.128902 
L 182.050909 -172.211471 
L 290.269091 -238.241525 
L 398.487273 -138.302694 
L 398.487273 -186.542265 
L 398.487273 -186.542265 
L 290.269091 -250.87856 
L 182.050909 -207.122419 
L 73.832727 -268.517627 
z
" style="stroke: #dca7ae; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#p2c189dfd22)">
     <use xlink:href="#mbdd47da42e" x="0" y="345.6" style="fill: #dca7ae; fill-opacity: 0.2; stroke: #dca7ae; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_3">
    <defs>
     <path id="ma1550b462f" d="M 73.832727 -234.346068 
L 73.832727 -219.334595 
L 182.050909 -108.204282 
L 290.269091 -200.694235 
L 398.487273 -127.977063 
L 398.487273 -169.492773 
L 398.487273 -169.492773 
L 290.269091 -218.081849 
L 182.050909 -145.790261 
L 73.832727 -234.346068 
z
" style="stroke: #c07d99; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#p2c189dfd22)">
     <use xlink:href="#ma1550b462f" x="0" y="345.6" style="fill: #c07d99; fill-opacity: 0.2; stroke: #c07d99; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_4">
    <defs>
     <path id="m9dca2c19d1" d="M 73.832727 -161.418683 
L 73.832727 -137.575328 
L 182.050909 -50.112 
L 290.269091 -128.569584 
L 398.487273 -109.337828 
L 398.487273 -142.55369 
L 398.487273 -142.55369 
L 290.269091 -151.338994 
L 182.050909 -84.363065 
L 73.832727 -161.418683 
z
" style="stroke: #2d1e3e; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#p2c189dfd22)">
     <use xlink:href="#m9dca2c19d1" x="0" y="345.6" style="fill: #2d1e3e; fill-opacity: 0.2; stroke: #2d1e3e; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="matplotlib.axis_1">
    <g id="xtick_1">
     <g id="line2d_1">
      <defs>
       <path id="m3492cc86db" d="M 0 0 
L 0 3.5 
" style="stroke: #000000; stroke-width: 0.8"/>
      </defs>
      <g>
       <use xlink:href="#m3492cc86db" x="73.832727" y="307.584" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_1">
      <!-- Blue (5) -->
      <g transform="translate(54.096009 322.182437)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-42" d="M 1259 2228 
L 1259 519 
L 2272 519 
Q 2781 519 3026 730 
Q 3272 941 3272 1375 
Q 3272 1813 3026 2020 
Q 2781 2228 2272 2228 
L 1259 2228 
z
M 1259 4147 
L 1259 2741 
L 2194 2741 
Q 2656 2741 2882 2914 
Q 3109 3088 3109 3444 
Q 3109 3797 2882 3972 
Q 2656 4147 2194 4147 
L 1259 4147 
z
M 628 4666 
L 2241 4666 
Q 2963 4666 3353 4366 
Q 3744 4066 3744 3513 
Q 3744 3084 3544 2831 
Q 3344 2578 2956 2516 
Q 3422 2416 3680 2098 
Q 3938 1781 3938 1306 
Q 3938 681 3513 340 
Q 3088 0 2303 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-6c" d="M 603 4863 
L 1178 4863 
L 1178 0 
L 603 0 
L 603 4863 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-75" d="M 544 1381 
L 544 3500 
L 1119 3500 
L 1119 1403 
Q 1119 906 1312 657 
Q 1506 409 1894 409 
Q 2359 409 2629 706 
Q 2900 1003 2900 1516 
L 2900 3500 
L 3475 3500 
L 3475 0 
L 2900 0 
L 2900 538 
Q 2691 219 2414 64 
Q 2138 -91 1772 -91 
Q 1169 -91 856 284 
Q 544 659 544 1381 
z
M 1991 3584 
L 1991 3584 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-65" d="M 3597 1894 
L 3597 1613 
L 953 1613 
Q 991 1019 1311 708 
Q 1631 397 2203 397 
Q 2534 397 2845 478 
Q 3156 559 3463 722 
L 3463 178 
Q 3153 47 2828 -22 
Q 2503 -91 2169 -91 
Q 1331 -91 842 396 
Q 353 884 353 1716 
Q 353 2575 817 3079 
Q 1281 3584 2069 3584 
Q 2775 3584 3186 3129 
Q 3597 2675 3597 1894 
z
M 3022 2063 
Q 3016 2534 2758 2815 
Q 2500 3097 2075 3097 
Q 1594 3097 1305 2825 
Q 1016 2553 972 2059 
L 3022 2063 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-20" transform="scale(0.015625)"/>
        <path id="DejaVuSans-28" d="M 1984 4856 
Q 1566 4138 1362 3434 
Q 1159 2731 1159 2009 
Q 1159 1288 1364 580 
Q 1569 -128 1984 -844 
L 1484 -844 
Q 1016 -109 783 600 
Q 550 1309 550 2009 
Q 550 2706 781 3412 
Q 1013 4119 1484 4856 
L 1984 4856 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-35" d="M 691 4666 
L 3169 4666 
L 3169 4134 
L 1269 4134 
L 1269 2991 
Q 1406 3038 1543 3061 
Q 1681 3084 1819 3084 
Q 2600 3084 3056 2656 
Q 3513 2228 3513 1497 
Q 3513 744 3044 326 
Q 2575 -91 1722 -91 
Q 1428 -91 1123 -41 
Q 819 9 494 109 
L 494 744 
Q 775 591 1075 516 
Q 1375 441 1709 441 
Q 2250 441 2565 725 
Q 2881 1009 2881 1497 
Q 2881 1984 2565 2268 
Q 2250 2553 1709 2553 
Q 1456 2553 1204 2497 
Q 953 2441 691 2322 
L 691 4666 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-29" d="M 513 4856 
L 1013 4856 
Q 1481 4119 1714 3412 
Q 1947 2706 1947 2009 
Q 1947 1309 1714 600 
Q 1481 -109 1013 -844 
L 513 -844 
Q 928 -128 1133 580 
Q 1338 1288 1338 2009 
Q 1338 2731 1133 3434 
Q 928 4138 513 4856 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-42"/>
       <use xlink:href="#DejaVuSans-6c" x="68.603516"/>
       <use xlink:href="#DejaVuSans-75" x="96.386719"/>
       <use xlink:href="#DejaVuSans-65" x="159.765625"/>
       <use xlink:href="#DejaVuSans-20" x="221.289062"/>
       <use xlink:href="#DejaVuSans-28" x="253.076172"/>
       <use xlink:href="#DejaVuSans-35" x="292.089844"/>
       <use xlink:href="#DejaVuSans-29" x="355.712891"/>
      </g>
     </g>
    </g>
    <g id="xtick_2">
     <g id="line2d_2">
      <g>
       <use xlink:href="#m3492cc86db" x="182.050909" y="307.584" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_2">
      <!-- Green (6) -->
      <g transform="translate(158.239972 322.182437)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-47" d="M 3809 666 
L 3809 1919 
L 2778 1919 
L 2778 2438 
L 4434 2438 
L 4434 434 
Q 4069 175 3628 42 
Q 3188 -91 2688 -91 
Q 1594 -91 976 548 
Q 359 1188 359 2328 
Q 359 3472 976 4111 
Q 1594 4750 2688 4750 
Q 3144 4750 3555 4637 
Q 3966 4525 4313 4306 
L 4313 3634 
Q 3963 3931 3569 4081 
Q 3175 4231 2741 4231 
Q 1884 4231 1454 3753 
Q 1025 3275 1025 2328 
Q 1025 1384 1454 906 
Q 1884 428 2741 428 
Q 3075 428 3337 486 
Q 3600 544 3809 666 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-72" d="M 2631 2963 
Q 2534 3019 2420 3045 
Q 2306 3072 2169 3072 
Q 1681 3072 1420 2755 
Q 1159 2438 1159 1844 
L 1159 0 
L 581 0 
L 581 3500 
L 1159 3500 
L 1159 2956 
Q 1341 3275 1631 3429 
Q 1922 3584 2338 3584 
Q 2397 3584 2469 3576 
Q 2541 3569 2628 3553 
L 2631 2963 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-6e" d="M 3513 2113 
L 3513 0 
L 2938 0 
L 2938 2094 
Q 2938 2591 2744 2837 
Q 2550 3084 2163 3084 
Q 1697 3084 1428 2787 
Q 1159 2491 1159 1978 
L 1159 0 
L 581 0 
L 581 3500 
L 1159 3500 
L 1159 2956 
Q 1366 3272 1645 3428 
Q 1925 3584 2291 3584 
Q 2894 3584 3203 3211 
Q 3513 2838 3513 2113 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-36" d="M 2113 2584 
Q 1688 2584 1439 2293 
Q 1191 2003 1191 1497 
Q 1191 994 1439 701 
Q 1688 409 2113 409 
Q 2538 409 2786 701 
Q 3034 994 3034 1497 
Q 3034 2003 2786 2293 
Q 2538 2584 2113 2584 
z
M 3366 4563 
L 3366 3988 
Q 3128 4100 2886 4159 
Q 2644 4219 2406 4219 
Q 1781 4219 1451 3797 
Q 1122 3375 1075 2522 
Q 1259 2794 1537 2939 
Q 1816 3084 2150 3084 
Q 2853 3084 3261 2657 
Q 3669 2231 3669 1497 
Q 3669 778 3244 343 
Q 2819 -91 2113 -91 
Q 1303 -91 875 529 
Q 447 1150 447 2328 
Q 447 3434 972 4092 
Q 1497 4750 2381 4750 
Q 2619 4750 2861 4703 
Q 3103 4656 3366 4563 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-47"/>
       <use xlink:href="#DejaVuSans-72" x="77.490234"/>
       <use xlink:href="#DejaVuSans-65" x="116.353516"/>
       <use xlink:href="#DejaVuSans-65" x="177.876953"/>
       <use xlink:href="#DejaVuSans-6e" x="239.400391"/>
       <use xlink:href="#DejaVuSans-20" x="302.779297"/>
       <use xlink:href="#DejaVuSans-28" x="334.566406"/>
       <use xlink:href="#DejaVuSans-36" x="373.580078"/>
       <use xlink:href="#DejaVuSans-29" x="437.203125"/>
      </g>
     </g>
    </g>
    <g id="xtick_3">
     <g id="line2d_3">
      <g>
       <use xlink:href="#m3492cc86db" x="290.269091" y="307.584" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_3">
      <!-- Red (16) -->
      <g transform="translate(268.915966 322.182437)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-52" d="M 2841 2188 
Q 3044 2119 3236 1894 
Q 3428 1669 3622 1275 
L 4263 0 
L 3584 0 
L 2988 1197 
Q 2756 1666 2539 1819 
Q 2322 1972 1947 1972 
L 1259 1972 
L 1259 0 
L 628 0 
L 628 4666 
L 2053 4666 
Q 2853 4666 3247 4331 
Q 3641 3997 3641 3322 
Q 3641 2881 3436 2590 
Q 3231 2300 2841 2188 
z
M 1259 4147 
L 1259 2491 
L 2053 2491 
Q 2509 2491 2742 2702 
Q 2975 2913 2975 3322 
Q 2975 3731 2742 3939 
Q 2509 4147 2053 4147 
L 1259 4147 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-64" d="M 2906 2969 
L 2906 4863 
L 3481 4863 
L 3481 0 
L 2906 0 
L 2906 525 
Q 2725 213 2448 61 
Q 2172 -91 1784 -91 
Q 1150 -91 751 415 
Q 353 922 353 1747 
Q 353 2572 751 3078 
Q 1150 3584 1784 3584 
Q 2172 3584 2448 3432 
Q 2725 3281 2906 2969 
z
M 947 1747 
Q 947 1113 1208 752 
Q 1469 391 1925 391 
Q 2381 391 2643 752 
Q 2906 1113 2906 1747 
Q 2906 2381 2643 2742 
Q 2381 3103 1925 3103 
Q 1469 3103 1208 2742 
Q 947 2381 947 1747 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-31" d="M 794 531 
L 1825 531 
L 1825 4091 
L 703 3866 
L 703 4441 
L 1819 4666 
L 2450 4666 
L 2450 531 
L 3481 531 
L 3481 0 
L 794 0 
L 794 531 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-52"/>
       <use xlink:href="#DejaVuSans-65" x="64.982422"/>
       <use xlink:href="#DejaVuSans-64" x="126.505859"/>
       <use xlink:href="#DejaVuSans-20" x="189.982422"/>
       <use xlink:href="#DejaVuSans-28" x="221.769531"/>
       <use xlink:href="#DejaVuSans-31" x="260.783203"/>
       <use xlink:href="#DejaVuSans-36" x="324.40625"/>
       <use xlink:href="#DejaVuSans-29" x="388.029297"/>
      </g>
     </g>
    </g>
    <g id="xtick_4">
     <g id="line2d_4">
      <g>
       <use xlink:href="#m3492cc86db" x="398.487273" y="307.584" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_4">
      <!-- Yellow (17) -->
      <g transform="translate(371.239616 322.182437)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-59" d="M -13 4666 
L 666 4666 
L 1959 2747 
L 3244 4666 
L 3922 4666 
L 2272 2222 
L 2272 0 
L 1638 0 
L 1638 2222 
L -13 4666 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-6f" d="M 1959 3097 
Q 1497 3097 1228 2736 
Q 959 2375 959 1747 
Q 959 1119 1226 758 
Q 1494 397 1959 397 
Q 2419 397 2687 759 
Q 2956 1122 2956 1747 
Q 2956 2369 2687 2733 
Q 2419 3097 1959 3097 
z
M 1959 3584 
Q 2709 3584 3137 3096 
Q 3566 2609 3566 1747 
Q 3566 888 3137 398 
Q 2709 -91 1959 -91 
Q 1206 -91 779 398 
Q 353 888 353 1747 
Q 353 2609 779 3096 
Q 1206 3584 1959 3584 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-77" d="M 269 3500 
L 844 3500 
L 1563 769 
L 2278 3500 
L 2956 3500 
L 3675 769 
L 4391 3500 
L 4966 3500 
L 4050 0 
L 3372 0 
L 2619 2869 
L 1863 0 
L 1184 0 
L 269 3500 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-37" d="M 525 4666 
L 3525 4666 
L 3525 4397 
L 1831 0 
L 1172 0 
L 2766 4134 
L 525 4134 
L 525 4666 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-59"/>
       <use xlink:href="#DejaVuSans-65" x="47.833984"/>
       <use xlink:href="#DejaVuSans-6c" x="109.357422"/>
       <use xlink:href="#DejaVuSans-6c" x="137.140625"/>
       <use xlink:href="#DejaVuSans-6f" x="164.923828"/>
       <use xlink:href="#DejaVuSans-77" x="226.105469"/>
       <use xlink:href="#DejaVuSans-20" x="307.892578"/>
       <use xlink:href="#DejaVuSans-28" x="339.679688"/>
       <use xlink:href="#DejaVuSans-31" x="378.693359"/>
       <use xlink:href="#DejaVuSans-37" x="442.316406"/>
       <use xlink:href="#DejaVuSans-29" x="505.939453"/>
      </g>
     </g>
    </g>
    <g id="text_5">
     <!-- state -->
     <g transform="translate(223.494375 335.860562)scale(0.1 -0.1)">
      <defs>
       <path id="DejaVuSans-73" d="M 2834 3397 
L 2834 2853 
Q 2591 2978 2328 3040 
Q 2066 3103 1784 3103 
Q 1356 3103 1142 2972 
Q 928 2841 928 2578 
Q 928 2378 1081 2264 
Q 1234 2150 1697 2047 
L 1894 2003 
Q 2506 1872 2764 1633 
Q 3022 1394 3022 966 
Q 3022 478 2636 193 
Q 2250 -91 1575 -91 
Q 1294 -91 989 -36 
Q 684 19 347 128 
L 347 722 
Q 666 556 975 473 
Q 1284 391 1588 391 
Q 1994 391 2212 530 
Q 2431 669 2431 922 
Q 2431 1156 2273 1281 
Q 2116 1406 1581 1522 
L 1381 1569 
Q 847 1681 609 1914 
Q 372 2147 372 2553 
Q 372 3047 722 3315 
Q 1072 3584 1716 3584 
Q 2034 3584 2315 3537 
Q 2597 3491 2834 3397 
z
" transform="scale(0.015625)"/>
       <path id="DejaVuSans-74" d="M 1172 4494 
L 1172 3500 
L 2356 3500 
L 2356 3053 
L 1172 3053 
L 1172 1153 
Q 1172 725 1289 603 
Q 1406 481 1766 481 
L 2356 481 
L 2356 0 
L 1766 0 
Q 1100 0 847 248 
Q 594 497 594 1153 
L 594 3053 
L 172 3053 
L 172 3500 
L 594 3500 
L 594 4494 
L 1172 4494 
z
" transform="scale(0.015625)"/>
       <path id="DejaVuSans-61" d="M 2194 1759 
Q 1497 1759 1228 1600 
Q 959 1441 959 1056 
Q 959 750 1161 570 
Q 1363 391 1709 391 
Q 2188 391 2477 730 
Q 2766 1069 2766 1631 
L 2766 1759 
L 2194 1759 
z
M 3341 1997 
L 3341 0 
L 2766 0 
L 2766 531 
Q 2569 213 2275 61 
Q 1981 -91 1556 -91 
Q 1019 -91 701 211 
Q 384 513 384 1019 
Q 384 1609 779 1909 
Q 1175 2209 1959 2209 
L 2766 2209 
L 2766 2266 
Q 2766 2663 2505 2880 
Q 2244 3097 1772 3097 
Q 1472 3097 1187 3025 
Q 903 2953 641 2809 
L 641 3341 
Q 956 3463 1253 3523 
Q 1550 3584 1831 3584 
Q 2591 3584 2966 3190 
Q 3341 2797 3341 1997 
z
" transform="scale(0.015625)"/>
      </defs>
      <use xlink:href="#DejaVuSans-73"/>
      <use xlink:href="#DejaVuSans-74" x="52.099609"/>
      <use xlink:href="#DejaVuSans-61" x="91.308594"/>
      <use xlink:href="#DejaVuSans-74" x="152.587891"/>
      <use xlink:href="#DejaVuSans-65" x="191.796875"/>
     </g>
    </g>
   </g>
   <g id="matplotlib.axis_2">
    <g id="ytick_1">
     <g id="line2d_5">
      <defs>
       <path id="m81ade0b1cc" d="M 0 0 
L -3.5 0 
" style="stroke: #000000; stroke-width: 0.8"/>
      </defs>
      <g>
       <use xlink:href="#m81ade0b1cc" x="57.6" y="298.786149" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_6">
      <!-- 0.60 -->
      <g transform="translate(28.334375 302.585367)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-30" d="M 2034 4250 
Q 1547 4250 1301 3770 
Q 1056 3291 1056 2328 
Q 1056 1369 1301 889 
Q 1547 409 2034 409 
Q 2525 409 2770 889 
Q 3016 1369 3016 2328 
Q 3016 3291 2770 3770 
Q 2525 4250 2034 4250 
z
M 2034 4750 
Q 2819 4750 3233 4129 
Q 3647 3509 3647 2328 
Q 3647 1150 3233 529 
Q 2819 -91 2034 -91 
Q 1250 -91 836 529 
Q 422 1150 422 2328 
Q 422 3509 836 4129 
Q 1250 4750 2034 4750 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-2e" d="M 684 794 
L 1344 794 
L 1344 0 
L 684 0 
L 684 794 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-30"/>
       <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
       <use xlink:href="#DejaVuSans-36" x="95.410156"/>
       <use xlink:href="#DejaVuSans-30" x="159.033203"/>
      </g>
     </g>
    </g>
    <g id="ytick_2">
     <g id="line2d_6">
      <g>
       <use xlink:href="#m81ade0b1cc" x="57.6" y="268.012629" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_7">
      <!-- 0.65 -->
      <g transform="translate(28.334375 271.811848)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
       <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
       <use xlink:href="#DejaVuSans-36" x="95.410156"/>
       <use xlink:href="#DejaVuSans-35" x="159.033203"/>
      </g>
     </g>
    </g>
    <g id="ytick_3">
     <g id="line2d_7">
      <g>
       <use xlink:href="#m81ade0b1cc" x="57.6" y="237.239109" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_8">
      <!-- 0.70 -->
      <g transform="translate(28.334375 241.038328)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
       <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
       <use xlink:href="#DejaVuSans-37" x="95.410156"/>
       <use xlink:href="#DejaVuSans-30" x="159.033203"/>
      </g>
     </g>
    </g>
    <g id="ytick_4">
     <g id="line2d_8">
      <g>
       <use xlink:href="#m81ade0b1cc" x="57.6" y="206.46559" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_9">
      <!-- 0.75 -->
      <g transform="translate(28.334375 210.264808)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
       <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
       <use xlink:href="#DejaVuSans-37" x="95.410156"/>
       <use xlink:href="#DejaVuSans-35" x="159.033203"/>
      </g>
     </g>
    </g>
    <g id="ytick_5">
     <g id="line2d_9">
      <g>
       <use xlink:href="#m81ade0b1cc" x="57.6" y="175.69207" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_10">
      <!-- 0.80 -->
      <g transform="translate(28.334375 179.491289)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-38" d="M 2034 2216 
Q 1584 2216 1326 1975 
Q 1069 1734 1069 1313 
Q 1069 891 1326 650 
Q 1584 409 2034 409 
Q 2484 409 2743 651 
Q 3003 894 3003 1313 
Q 3003 1734 2745 1975 
Q 2488 2216 2034 2216 
z
M 1403 2484 
Q 997 2584 770 2862 
Q 544 3141 544 3541 
Q 544 4100 942 4425 
Q 1341 4750 2034 4750 
Q 2731 4750 3128 4425 
Q 3525 4100 3525 3541 
Q 3525 3141 3298 2862 
Q 3072 2584 2669 2484 
Q 3125 2378 3379 2068 
Q 3634 1759 3634 1313 
Q 3634 634 3220 271 
Q 2806 -91 2034 -91 
Q 1263 -91 848 271 
Q 434 634 434 1313 
Q 434 1759 690 2068 
Q 947 2378 1403 2484 
z
M 1172 3481 
Q 1172 3119 1398 2916 
Q 1625 2713 2034 2713 
Q 2441 2713 2670 2916 
Q 2900 3119 2900 3481 
Q 2900 3844 2670 4047 
Q 2441 4250 2034 4250 
Q 1625 4250 1398 4047 
Q 1172 3844 1172 3481 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-30"/>
       <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
       <use xlink:href="#DejaVuSans-38" x="95.410156"/>
       <use xlink:href="#DejaVuSans-30" x="159.033203"/>
      </g>
     </g>
    </g>
    <g id="ytick_6">
     <g id="line2d_10">
      <g>
       <use xlink:href="#m81ade0b1cc" x="57.6" y="144.91855" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_11">
      <!-- 0.85 -->
      <g transform="translate(28.334375 148.717769)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
       <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
       <use xlink:href="#DejaVuSans-38" x="95.410156"/>
       <use xlink:href="#DejaVuSans-35" x="159.033203"/>
      </g>
     </g>
    </g>
    <g id="ytick_7">
     <g id="line2d_11">
      <g>
       <use xlink:href="#m81ade0b1cc" x="57.6" y="114.145031" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_12">
      <!-- 0.90 -->
      <g transform="translate(28.334375 117.94425)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-39" d="M 703 97 
L 703 672 
Q 941 559 1184 500 
Q 1428 441 1663 441 
Q 2288 441 2617 861 
Q 2947 1281 2994 2138 
Q 2813 1869 2534 1725 
Q 2256 1581 1919 1581 
Q 1219 1581 811 2004 
Q 403 2428 403 3163 
Q 403 3881 828 4315 
Q 1253 4750 1959 4750 
Q 2769 4750 3195 4129 
Q 3622 3509 3622 2328 
Q 3622 1225 3098 567 
Q 2575 -91 1691 -91 
Q 1453 -91 1209 -44 
Q 966 3 703 97 
z
M 1959 2075 
Q 2384 2075 2632 2365 
Q 2881 2656 2881 3163 
Q 2881 3666 2632 3958 
Q 2384 4250 1959 4250 
Q 1534 4250 1286 3958 
Q 1038 3666 1038 3163 
Q 1038 2656 1286 2365 
Q 1534 2075 1959 2075 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-30"/>
       <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
       <use xlink:href="#DejaVuSans-39" x="95.410156"/>
       <use xlink:href="#DejaVuSans-30" x="159.033203"/>
      </g>
     </g>
    </g>
    <g id="ytick_8">
     <g id="line2d_12">
      <g>
       <use xlink:href="#m81ade0b1cc" x="57.6" y="83.371511" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_13">
      <!-- 0.95 -->
      <g transform="translate(28.334375 87.17073)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
       <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
       <use xlink:href="#DejaVuSans-39" x="95.410156"/>
       <use xlink:href="#DejaVuSans-35" x="159.033203"/>
      </g>
     </g>
    </g>
    <g id="ytick_9">
     <g id="line2d_13">
      <g>
       <use xlink:href="#m81ade0b1cc" x="57.6" y="52.597992" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_14">
      <!-- 1.00 -->
      <g transform="translate(28.334375 56.39721)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-31"/>
       <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
       <use xlink:href="#DejaVuSans-30" x="95.410156"/>
       <use xlink:href="#DejaVuSans-30" x="159.033203"/>
      </g>
     </g>
    </g>
    <g id="text_15">
     <!-- Choice accuracy -->
     <g transform="translate(22.254687 215.610813)rotate(-90)scale(0.1 -0.1)">
      <defs>
       <path id="DejaVuSans-43" d="M 4122 4306 
L 4122 3641 
Q 3803 3938 3442 4084 
Q 3081 4231 2675 4231 
Q 1875 4231 1450 3742 
Q 1025 3253 1025 2328 
Q 1025 1406 1450 917 
Q 1875 428 2675 428 
Q 3081 428 3442 575 
Q 3803 722 4122 1019 
L 4122 359 
Q 3791 134 3420 21 
Q 3050 -91 2638 -91 
Q 1578 -91 968 557 
Q 359 1206 359 2328 
Q 359 3453 968 4101 
Q 1578 4750 2638 4750 
Q 3056 4750 3426 4639 
Q 3797 4528 4122 4306 
z
" transform="scale(0.015625)"/>
       <path id="DejaVuSans-68" d="M 3513 2113 
L 3513 0 
L 2938 0 
L 2938 2094 
Q 2938 2591 2744 2837 
Q 2550 3084 2163 3084 
Q 1697 3084 1428 2787 
Q 1159 2491 1159 1978 
L 1159 0 
L 581 0 
L 581 4863 
L 1159 4863 
L 1159 2956 
Q 1366 3272 1645 3428 
Q 1925 3584 2291 3584 
Q 2894 3584 3203 3211 
Q 3513 2838 3513 2113 
z
" transform="scale(0.015625)"/>
       <path id="DejaVuSans-69" d="M 603 3500 
L 1178 3500 
L 1178 0 
L 603 0 
L 603 3500 
z
M 603 4863 
L 1178 4863 
L 1178 4134 
L 603 4134 
L 603 4863 
z
" transform="scale(0.015625)"/>
       <path id="DejaVuSans-63" d="M 3122 3366 
L 3122 2828 
Q 2878 2963 2633 3030 
Q 2388 3097 2138 3097 
Q 1578 3097 1268 2742 
Q 959 2388 959 1747 
Q 959 1106 1268 751 
Q 1578 397 2138 397 
Q 2388 397 2633 464 
Q 2878 531 3122 666 
L 3122 134 
Q 2881 22 2623 -34 
Q 2366 -91 2075 -91 
Q 1284 -91 818 406 
Q 353 903 353 1747 
Q 353 2603 823 3093 
Q 1294 3584 2113 3584 
Q 2378 3584 2631 3529 
Q 2884 3475 3122 3366 
z
" transform="scale(0.015625)"/>
       <path id="DejaVuSans-79" d="M 2059 -325 
Q 1816 -950 1584 -1140 
Q 1353 -1331 966 -1331 
L 506 -1331 
L 506 -850 
L 844 -850 
Q 1081 -850 1212 -737 
Q 1344 -625 1503 -206 
L 1606 56 
L 191 3500 
L 800 3500 
L 1894 763 
L 2988 3500 
L 3597 3500 
L 2059 -325 
z
" transform="scale(0.015625)"/>
      </defs>
      <use xlink:href="#DejaVuSans-43"/>
      <use xlink:href="#DejaVuSans-68" x="69.824219"/>
      <use xlink:href="#DejaVuSans-6f" x="133.203125"/>
      <use xlink:href="#DejaVuSans-69" x="194.384766"/>
      <use xlink:href="#DejaVuSans-63" x="222.167969"/>
      <use xlink:href="#DejaVuSans-65" x="277.148438"/>
      <use xlink:href="#DejaVuSans-20" x="338.671875"/>
      <use xlink:href="#DejaVuSans-61" x="370.458984"/>
      <use xlink:href="#DejaVuSans-63" x="431.738281"/>
      <use xlink:href="#DejaVuSans-63" x="486.71875"/>
      <use xlink:href="#DejaVuSans-75" x="541.699219"/>
      <use xlink:href="#DejaVuSans-72" x="605.078125"/>
      <use xlink:href="#DejaVuSans-61" x="646.191406"/>
      <use xlink:href="#DejaVuSans-63" x="707.470703"/>
      <use xlink:href="#DejaVuSans-79" x="762.451172"/>
     </g>
    </g>
   </g>
   <g id="line2d_14">
    <path d="M 73.832727 54.780511 
L 182.050909 67.252048 
L 290.269091 56.943949 
L 398.487273 170.957682 
" clip-path="url(#p2c189dfd22)" style="fill: none; stroke: #edd1cb; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="line2d_15">
    <path d="M 73.832727 81.880752 
L 182.050909 155.474583 
L 290.269091 100.805939 
L 398.487273 184.009237 
" clip-path="url(#p2c189dfd22)" style="fill: none; stroke: #dca7ae; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="line2d_16">
    <path d="M 73.832727 118.759669 
L 182.050909 219.38577 
L 290.269091 136.211958 
L 398.487273 195.827189 
" clip-path="url(#p2c189dfd22)" style="fill: none; stroke: #c07d99; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="line2d_17">
    <path d="M 73.832727 195.935659 
L 182.050909 278.362467 
L 290.269091 206.240965 
L 398.487273 218.677304 
" clip-path="url(#p2c189dfd22)" style="fill: none; stroke: #2d1e3e; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="line2d_18"/>
   <g id="line2d_19"/>
   <g id="line2d_20"/>
   <g id="line2d_21"/>
   <g id="patch_3">
    <path d="M 57.6 307.584 
L 57.6 41.472 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_4">
    <path d="M 414.72 307.584 
L 414.72 41.472 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_5">
    <path d="M 57.6 307.584 
L 414.72 307.584 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_6">
    <path d="M 57.6 41.472 
L 414.72 41.472 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="text_16">
    <!-- Max-entropy RL -->
    <g transform="translate(188.961563 35.472)scale(0.12 -0.12)">
     <defs>
      <path id="DejaVuSans-4d" d="M 628 4666 
L 1569 4666 
L 2759 1491 
L 3956 4666 
L 4897 4666 
L 4897 0 
L 4281 0 
L 4281 4097 
L 3078 897 
L 2444 897 
L 1241 4097 
L 1241 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-78" d="M 3513 3500 
L 2247 1797 
L 3578 0 
L 2900 0 
L 1881 1375 
L 863 0 
L 184 0 
L 1544 1831 
L 300 3500 
L 978 3500 
L 1906 2253 
L 2834 3500 
L 3513 3500 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-2d" d="M 313 2009 
L 1997 2009 
L 1997 1497 
L 313 1497 
L 313 2009 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-70" d="M 1159 525 
L 1159 -1331 
L 581 -1331 
L 581 3500 
L 1159 3500 
L 1159 2969 
Q 1341 3281 1617 3432 
Q 1894 3584 2278 3584 
Q 2916 3584 3314 3078 
Q 3713 2572 3713 1747 
Q 3713 922 3314 415 
Q 2916 -91 2278 -91 
Q 1894 -91 1617 61 
Q 1341 213 1159 525 
z
M 3116 1747 
Q 3116 2381 2855 2742 
Q 2594 3103 2138 3103 
Q 1681 3103 1420 2742 
Q 1159 2381 1159 1747 
Q 1159 1113 1420 752 
Q 1681 391 2138 391 
Q 2594 391 2855 752 
Q 3116 1113 3116 1747 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-4c" d="M 628 4666 
L 1259 4666 
L 1259 531 
L 3531 531 
L 3531 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
     </defs>
     <use xlink:href="#DejaVuSans-4d"/>
     <use xlink:href="#DejaVuSans-61" x="86.279297"/>
     <use xlink:href="#DejaVuSans-78" x="147.558594"/>
     <use xlink:href="#DejaVuSans-2d" x="206.738281"/>
     <use xlink:href="#DejaVuSans-65" x="242.822266"/>
     <use xlink:href="#DejaVuSans-6e" x="304.345703"/>
     <use xlink:href="#DejaVuSans-74" x="367.724609"/>
     <use xlink:href="#DejaVuSans-72" x="406.933594"/>
     <use xlink:href="#DejaVuSans-6f" x="445.796875"/>
     <use xlink:href="#DejaVuSans-70" x="506.978516"/>
     <use xlink:href="#DejaVuSans-79" x="570.455078"/>
     <use xlink:href="#DejaVuSans-20" x="629.634766"/>
     <use xlink:href="#DejaVuSans-52" x="661.421875"/>
     <use xlink:href="#DejaVuSans-4c" x="730.904297"/>
    </g>
   </g>
   <g id="legend_1">
    <g id="patch_7">
     <path d="M 347.091875 302.584 
L 407.72 302.584 
Q 409.72 302.584 409.72 300.584 
L 409.72 228.193375 
Q 409.72 226.193375 407.72 226.193375 
L 347.091875 226.193375 
Q 345.091875 226.193375 345.091875 228.193375 
L 345.091875 300.584 
Q 345.091875 302.584 347.091875 302.584 
z
" style="fill: #ffffff; opacity: 0.8; stroke: #cccccc; stroke-linejoin: miter"/>
    </g>
    <g id="text_17">
     <!-- alpha -->
     <g transform="translate(363.545781 237.791812)scale(0.1 -0.1)">
      <use xlink:href="#DejaVuSans-61"/>
      <use xlink:href="#DejaVuSans-6c" x="61.279297"/>
      <use xlink:href="#DejaVuSans-70" x="89.0625"/>
      <use xlink:href="#DejaVuSans-68" x="152.539062"/>
      <use xlink:href="#DejaVuSans-61" x="215.917969"/>
     </g>
    </g>
    <g id="line2d_22">
     <path d="M 349.091875 248.969937 
L 359.091875 248.969937 
L 369.091875 248.969937 
" style="fill: none; stroke: #edd1cb; stroke-width: 1.5; stroke-linecap: square"/>
    </g>
    <g id="text_18">
     <!-- 5.0 -->
     <g transform="translate(377.091875 252.469937)scale(0.1 -0.1)">
      <use xlink:href="#DejaVuSans-35"/>
      <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
      <use xlink:href="#DejaVuSans-30" x="95.410156"/>
     </g>
    </g>
    <g id="line2d_23">
     <path d="M 349.091875 263.648062 
L 359.091875 263.648062 
L 369.091875 263.648062 
" style="fill: none; stroke: #dca7ae; stroke-width: 1.5; stroke-linecap: square"/>
    </g>
    <g id="text_19">
     <!-- 50.0 -->
     <g transform="translate(377.091875 267.148062)scale(0.1 -0.1)">
      <use xlink:href="#DejaVuSans-35"/>
      <use xlink:href="#DejaVuSans-30" x="63.623047"/>
      <use xlink:href="#DejaVuSans-2e" x="127.246094"/>
      <use xlink:href="#DejaVuSans-30" x="159.033203"/>
     </g>
    </g>
    <g id="line2d_24">
     <path d="M 349.091875 278.326188 
L 359.091875 278.326188 
L 369.091875 278.326188 
" style="fill: none; stroke: #c07d99; stroke-width: 1.5; stroke-linecap: square"/>
    </g>
    <g id="text_20">
     <!-- 100.0 -->
     <g transform="translate(377.091875 281.826188)scale(0.1 -0.1)">
      <use xlink:href="#DejaVuSans-31"/>
      <use xlink:href="#DejaVuSans-30" x="63.623047"/>
      <use xlink:href="#DejaVuSans-30" x="127.246094"/>
      <use xlink:href="#DejaVuSans-2e" x="190.869141"/>
      <use xlink:href="#DejaVuSans-30" x="222.65625"/>
     </g>
    </g>
    <g id="line2d_25">
     <path d="M 349.091875 293.004312 
L 359.091875 293.004312 
L 369.091875 293.004312 
" style="fill: none; stroke: #2d1e3e; stroke-width: 1.5; stroke-linecap: square"/>
    </g>
    <g id="text_21">
     <!-- 250.0 -->
     <g transform="translate(377.091875 296.504312)scale(0.1 -0.1)">
      <defs>
       <path id="DejaVuSans-32" d="M 1228 531 
L 3431 531 
L 3431 0 
L 469 0 
L 469 531 
Q 828 903 1448 1529 
Q 2069 2156 2228 2338 
Q 2531 2678 2651 2914 
Q 2772 3150 2772 3378 
Q 2772 3750 2511 3984 
Q 2250 4219 1831 4219 
Q 1534 4219 1204 4116 
Q 875 4013 500 3803 
L 500 4441 
Q 881 4594 1212 4672 
Q 1544 4750 1819 4750 
Q 2544 4750 2975 4387 
Q 3406 4025 3406 3419 
Q 3406 3131 3298 2873 
Q 3191 2616 2906 2266 
Q 2828 2175 2409 1742 
Q 1991 1309 1228 531 
z
" transform="scale(0.015625)"/>
      </defs>
      <use xlink:href="#DejaVuSans-32"/>
      <use xlink:href="#DejaVuSans-35" x="63.623047"/>
      <use xlink:href="#DejaVuSans-30" x="127.246094"/>
      <use xlink:href="#DejaVuSans-2e" x="190.869141"/>
      <use xlink:href="#DejaVuSans-30" x="222.65625"/>
     </g>
    </g>
   </g>
  </g>
 </g>
 <defs>
  <clipPath id="p2c189dfd22">
   <rect x="57.6" y="41.472" width="357.12" height="266.112"/>
  </clipPath>
 </defs>
</svg>


##### DRA

DRA has an extra parameter, so it can produce a wider range of curves than max-entropy RL. However, it doesn't stick to the rigid structure of the curve like max-entropy RL. It's more like the blue state (5) is the most accurate, and the rest could be anything. In this task, the green state (6) is *not* the least accurate, unlike the 2AFC task. Probably because it is less controlled? But in any case, the fact that it can fit pretty much anything makes me wonder if this is even a useful model to provide a normative explanation of human behavior.

<?xml version="1.0" encoding="utf-8" standalone="no"?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN"
  "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg xmlns:xlink="http://www.w3.org/1999/xlink" width="1498.14pt" height="360pt" viewBox="0 0 1498.14 360" xmlns="http://www.w3.org/2000/svg" version="1.1">
 <metadata>
  <rdf:RDF xmlns:dc="http://purl.org/dc/elements/1.1/" xmlns:cc="http://creativecommons.org/ns#" xmlns:rdf="http://www.w3.org/1999/02/22-rdf-syntax-ns#">
   <cc:Work>
    <dc:type rdf:resource="http://purl.org/dc/dcmitype/StillImage"/>
    <dc:date>2023-02-14T19:02:44.264943</dc:date>
    <dc:format>image/svg+xml</dc:format>
    <dc:creator>
     <cc:Agent>
      <dc:title>Matplotlib v3.5.1, https://matplotlib.org/</dc:title>
     </cc:Agent>
    </dc:creator>
   </cc:Work>
  </rdf:RDF>
 </metadata>
 <defs>
  <style type="text/css">*{stroke-linejoin: round; stroke-linecap: butt}</style>
 </defs>
 <g id="figure_1">
  <g id="patch_1">
   <path d="M 0 360 
L 1498.14 360 
L 1498.14 0 
L 0 0 
z
" style="fill: #ffffff"/>
  </g>
  <g id="axes_1">
   <g id="patch_2">
    <path d="M 49.646695 318.04 
L 372.420023 318.04 
L 372.420023 24.72 
L 49.646695 24.72 
z
" style="fill: #ffffff"/>
   </g>
   <g id="PolyCollection_1">
    <defs>
     <path id="m4de5c6c188" d="M 64.31821 -321.069066 
L 64.31821 -318.593937 
L 162.128309 -174.003535 
L 259.938409 -309.633636 
L 357.748508 -195.030003 
L 357.748508 -292.935101 
L 357.748508 -292.935101 
L 259.938409 -314.404646 
L 162.128309 -207.827048 
L 64.31821 -321.069066 
z
" style="stroke: #edd1cb; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#p62ac668042)">
     <use xlink:href="#m4de5c6c188" x="0" y="360" style="fill: #edd1cb; fill-opacity: 0.2; stroke: #edd1cb; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_2">
    <defs>
     <path id="mebdab158cf" d="M 64.31821 -320.498781 
L 64.31821 -317.425437 
L 162.128309 -307.438272 
L 259.938409 -311.643907 
L 357.748508 -65.305749 
L 357.748508 -230.520601 
L 357.748508 -230.520601 
L 259.938409 -315.940825 
L 162.128309 -317.61136 
L 64.31821 -320.498781 
z
" style="stroke: #e9c6c2; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#p62ac668042)">
     <use xlink:href="#mebdab158cf" x="0" y="360" style="fill: #e9c6c2; fill-opacity: 0.2; stroke: #e9c6c2; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_3">
    <defs>
     <path id="m67d9444ea5" d="M 64.31821 -321.947273 
L 64.31821 -320.150483 
L 162.128309 -307.835257 
L 259.938409 -310.427347 
L 357.748508 -55.292727 
L 357.748508 -215.501069 
L 357.748508 -215.501069 
L 259.938409 -314.882078 
L 162.128309 -318.286805 
L 64.31821 -321.947273 
z
" style="stroke: #dfaeb2; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#p62ac668042)">
     <use xlink:href="#m67d9444ea5" x="0" y="360" style="fill: #dfaeb2; fill-opacity: 0.2; stroke: #dfaeb2; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_4">
    <defs>
     <path id="m493e571c00" d="M 64.31821 -321.780259 
L 64.31821 -320.025688 
L 162.128309 -300.683419 
L 259.938409 -307.684867 
L 357.748508 -63.146077 
L 357.748508 -270.47452 
L 357.748508 -270.47452 
L 259.938409 -312.854177 
L 162.128309 -314.197926 
L 64.31821 -321.780259 
z
" style="stroke: #af6c91; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#p62ac668042)">
     <use xlink:href="#m493e571c00" x="0" y="360" style="fill: #af6c91; fill-opacity: 0.2; stroke: #af6c91; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_5">
    <defs>
     <path id="m6ac297afcf" d="M 64.31821 -320.876779 
L 64.31821 -318.195808 
L 162.128309 -288.543097 
L 259.938409 -302.215999 
L 357.748508 -118.965273 
L 357.748508 -254.526178 
L 357.748508 -254.526178 
L 259.938409 -308.320188 
L 162.128309 -305.836614 
L 64.31821 -320.876779 
z
" style="stroke: #2d1e3e; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#p62ac668042)">
     <use xlink:href="#m6ac297afcf" x="0" y="360" style="fill: #2d1e3e; fill-opacity: 0.2; stroke: #2d1e3e; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="matplotlib.axis_1">
    <g id="xtick_1">
     <g id="line2d_1">
      <defs>
       <path id="m2a2fc8578d" d="M 0 0 
L 0 3.5 
" style="stroke: #000000; stroke-width: 0.8"/>
      </defs>
      <g>
       <use xlink:href="#m2a2fc8578d" x="64.31821" y="318.04" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_1">
      <!-- Blue (5) -->
      <g transform="translate(44.581491 332.638438)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-42" d="M 1259 2228 
L 1259 519 
L 2272 519 
Q 2781 519 3026 730 
Q 3272 941 3272 1375 
Q 3272 1813 3026 2020 
Q 2781 2228 2272 2228 
L 1259 2228 
z
M 1259 4147 
L 1259 2741 
L 2194 2741 
Q 2656 2741 2882 2914 
Q 3109 3088 3109 3444 
Q 3109 3797 2882 3972 
Q 2656 4147 2194 4147 
L 1259 4147 
z
M 628 4666 
L 2241 4666 
Q 2963 4666 3353 4366 
Q 3744 4066 3744 3513 
Q 3744 3084 3544 2831 
Q 3344 2578 2956 2516 
Q 3422 2416 3680 2098 
Q 3938 1781 3938 1306 
Q 3938 681 3513 340 
Q 3088 0 2303 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-6c" d="M 603 4863 
L 1178 4863 
L 1178 0 
L 603 0 
L 603 4863 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-75" d="M 544 1381 
L 544 3500 
L 1119 3500 
L 1119 1403 
Q 1119 906 1312 657 
Q 1506 409 1894 409 
Q 2359 409 2629 706 
Q 2900 1003 2900 1516 
L 2900 3500 
L 3475 3500 
L 3475 0 
L 2900 0 
L 2900 538 
Q 2691 219 2414 64 
Q 2138 -91 1772 -91 
Q 1169 -91 856 284 
Q 544 659 544 1381 
z
M 1991 3584 
L 1991 3584 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-65" d="M 3597 1894 
L 3597 1613 
L 953 1613 
Q 991 1019 1311 708 
Q 1631 397 2203 397 
Q 2534 397 2845 478 
Q 3156 559 3463 722 
L 3463 178 
Q 3153 47 2828 -22 
Q 2503 -91 2169 -91 
Q 1331 -91 842 396 
Q 353 884 353 1716 
Q 353 2575 817 3079 
Q 1281 3584 2069 3584 
Q 2775 3584 3186 3129 
Q 3597 2675 3597 1894 
z
M 3022 2063 
Q 3016 2534 2758 2815 
Q 2500 3097 2075 3097 
Q 1594 3097 1305 2825 
Q 1016 2553 972 2059 
L 3022 2063 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-20" transform="scale(0.015625)"/>
        <path id="DejaVuSans-28" d="M 1984 4856 
Q 1566 4138 1362 3434 
Q 1159 2731 1159 2009 
Q 1159 1288 1364 580 
Q 1569 -128 1984 -844 
L 1484 -844 
Q 1016 -109 783 600 
Q 550 1309 550 2009 
Q 550 2706 781 3412 
Q 1013 4119 1484 4856 
L 1984 4856 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-35" d="M 691 4666 
L 3169 4666 
L 3169 4134 
L 1269 4134 
L 1269 2991 
Q 1406 3038 1543 3061 
Q 1681 3084 1819 3084 
Q 2600 3084 3056 2656 
Q 3513 2228 3513 1497 
Q 3513 744 3044 326 
Q 2575 -91 1722 -91 
Q 1428 -91 1123 -41 
Q 819 9 494 109 
L 494 744 
Q 775 591 1075 516 
Q 1375 441 1709 441 
Q 2250 441 2565 725 
Q 2881 1009 2881 1497 
Q 2881 1984 2565 2268 
Q 2250 2553 1709 2553 
Q 1456 2553 1204 2497 
Q 953 2441 691 2322 
L 691 4666 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-29" d="M 513 4856 
L 1013 4856 
Q 1481 4119 1714 3412 
Q 1947 2706 1947 2009 
Q 1947 1309 1714 600 
Q 1481 -109 1013 -844 
L 513 -844 
Q 928 -128 1133 580 
Q 1338 1288 1338 2009 
Q 1338 2731 1133 3434 
Q 928 4138 513 4856 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-42"/>
       <use xlink:href="#DejaVuSans-6c" x="68.603516"/>
       <use xlink:href="#DejaVuSans-75" x="96.386719"/>
       <use xlink:href="#DejaVuSans-65" x="159.765625"/>
       <use xlink:href="#DejaVuSans-20" x="221.289062"/>
       <use xlink:href="#DejaVuSans-28" x="253.076172"/>
       <use xlink:href="#DejaVuSans-35" x="292.089844"/>
       <use xlink:href="#DejaVuSans-29" x="355.712891"/>
      </g>
     </g>
    </g>
    <g id="xtick_2">
     <g id="line2d_2">
      <g>
       <use xlink:href="#m2a2fc8578d" x="162.128309" y="318.04" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_2">
      <!-- Green (6) -->
      <g transform="translate(138.317372 332.638438)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-47" d="M 3809 666 
L 3809 1919 
L 2778 1919 
L 2778 2438 
L 4434 2438 
L 4434 434 
Q 4069 175 3628 42 
Q 3188 -91 2688 -91 
Q 1594 -91 976 548 
Q 359 1188 359 2328 
Q 359 3472 976 4111 
Q 1594 4750 2688 4750 
Q 3144 4750 3555 4637 
Q 3966 4525 4313 4306 
L 4313 3634 
Q 3963 3931 3569 4081 
Q 3175 4231 2741 4231 
Q 1884 4231 1454 3753 
Q 1025 3275 1025 2328 
Q 1025 1384 1454 906 
Q 1884 428 2741 428 
Q 3075 428 3337 486 
Q 3600 544 3809 666 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-72" d="M 2631 2963 
Q 2534 3019 2420 3045 
Q 2306 3072 2169 3072 
Q 1681 3072 1420 2755 
Q 1159 2438 1159 1844 
L 1159 0 
L 581 0 
L 581 3500 
L 1159 3500 
L 1159 2956 
Q 1341 3275 1631 3429 
Q 1922 3584 2338 3584 
Q 2397 3584 2469 3576 
Q 2541 3569 2628 3553 
L 2631 2963 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-6e" d="M 3513 2113 
L 3513 0 
L 2938 0 
L 2938 2094 
Q 2938 2591 2744 2837 
Q 2550 3084 2163 3084 
Q 1697 3084 1428 2787 
Q 1159 2491 1159 1978 
L 1159 0 
L 581 0 
L 581 3500 
L 1159 3500 
L 1159 2956 
Q 1366 3272 1645 3428 
Q 1925 3584 2291 3584 
Q 2894 3584 3203 3211 
Q 3513 2838 3513 2113 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-36" d="M 2113 2584 
Q 1688 2584 1439 2293 
Q 1191 2003 1191 1497 
Q 1191 994 1439 701 
Q 1688 409 2113 409 
Q 2538 409 2786 701 
Q 3034 994 3034 1497 
Q 3034 2003 2786 2293 
Q 2538 2584 2113 2584 
z
M 3366 4563 
L 3366 3988 
Q 3128 4100 2886 4159 
Q 2644 4219 2406 4219 
Q 1781 4219 1451 3797 
Q 1122 3375 1075 2522 
Q 1259 2794 1537 2939 
Q 1816 3084 2150 3084 
Q 2853 3084 3261 2657 
Q 3669 2231 3669 1497 
Q 3669 778 3244 343 
Q 2819 -91 2113 -91 
Q 1303 -91 875 529 
Q 447 1150 447 2328 
Q 447 3434 972 4092 
Q 1497 4750 2381 4750 
Q 2619 4750 2861 4703 
Q 3103 4656 3366 4563 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-47"/>
       <use xlink:href="#DejaVuSans-72" x="77.490234"/>
       <use xlink:href="#DejaVuSans-65" x="116.353516"/>
       <use xlink:href="#DejaVuSans-65" x="177.876953"/>
       <use xlink:href="#DejaVuSans-6e" x="239.400391"/>
       <use xlink:href="#DejaVuSans-20" x="302.779297"/>
       <use xlink:href="#DejaVuSans-28" x="334.566406"/>
       <use xlink:href="#DejaVuSans-36" x="373.580078"/>
       <use xlink:href="#DejaVuSans-29" x="437.203125"/>
      </g>
     </g>
    </g>
    <g id="xtick_3">
     <g id="line2d_3">
      <g>
       <use xlink:href="#m2a2fc8578d" x="259.938409" y="318.04" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_3">
      <!-- Red (16) -->
      <g transform="translate(238.585284 332.638438)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-52" d="M 2841 2188 
Q 3044 2119 3236 1894 
Q 3428 1669 3622 1275 
L 4263 0 
L 3584 0 
L 2988 1197 
Q 2756 1666 2539 1819 
Q 2322 1972 1947 1972 
L 1259 1972 
L 1259 0 
L 628 0 
L 628 4666 
L 2053 4666 
Q 2853 4666 3247 4331 
Q 3641 3997 3641 3322 
Q 3641 2881 3436 2590 
Q 3231 2300 2841 2188 
z
M 1259 4147 
L 1259 2491 
L 2053 2491 
Q 2509 2491 2742 2702 
Q 2975 2913 2975 3322 
Q 2975 3731 2742 3939 
Q 2509 4147 2053 4147 
L 1259 4147 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-64" d="M 2906 2969 
L 2906 4863 
L 3481 4863 
L 3481 0 
L 2906 0 
L 2906 525 
Q 2725 213 2448 61 
Q 2172 -91 1784 -91 
Q 1150 -91 751 415 
Q 353 922 353 1747 
Q 353 2572 751 3078 
Q 1150 3584 1784 3584 
Q 2172 3584 2448 3432 
Q 2725 3281 2906 2969 
z
M 947 1747 
Q 947 1113 1208 752 
Q 1469 391 1925 391 
Q 2381 391 2643 752 
Q 2906 1113 2906 1747 
Q 2906 2381 2643 2742 
Q 2381 3103 1925 3103 
Q 1469 3103 1208 2742 
Q 947 2381 947 1747 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-31" d="M 794 531 
L 1825 531 
L 1825 4091 
L 703 3866 
L 703 4441 
L 1819 4666 
L 2450 4666 
L 2450 531 
L 3481 531 
L 3481 0 
L 794 0 
L 794 531 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-52"/>
       <use xlink:href="#DejaVuSans-65" x="64.982422"/>
       <use xlink:href="#DejaVuSans-64" x="126.505859"/>
       <use xlink:href="#DejaVuSans-20" x="189.982422"/>
       <use xlink:href="#DejaVuSans-28" x="221.769531"/>
       <use xlink:href="#DejaVuSans-31" x="260.783203"/>
       <use xlink:href="#DejaVuSans-36" x="324.40625"/>
       <use xlink:href="#DejaVuSans-29" x="388.029297"/>
      </g>
     </g>
    </g>
    <g id="xtick_4">
     <g id="line2d_4">
      <g>
       <use xlink:href="#m2a2fc8578d" x="357.748508" y="318.04" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_4">
      <!-- Yellow (17) -->
      <g transform="translate(330.500852 332.638438)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-59" d="M -13 4666 
L 666 4666 
L 1959 2747 
L 3244 4666 
L 3922 4666 
L 2272 2222 
L 2272 0 
L 1638 0 
L 1638 2222 
L -13 4666 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-6f" d="M 1959 3097 
Q 1497 3097 1228 2736 
Q 959 2375 959 1747 
Q 959 1119 1226 758 
Q 1494 397 1959 397 
Q 2419 397 2687 759 
Q 2956 1122 2956 1747 
Q 2956 2369 2687 2733 
Q 2419 3097 1959 3097 
z
M 1959 3584 
Q 2709 3584 3137 3096 
Q 3566 2609 3566 1747 
Q 3566 888 3137 398 
Q 2709 -91 1959 -91 
Q 1206 -91 779 398 
Q 353 888 353 1747 
Q 353 2609 779 3096 
Q 1206 3584 1959 3584 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-77" d="M 269 3500 
L 844 3500 
L 1563 769 
L 2278 3500 
L 2956 3500 
L 3675 769 
L 4391 3500 
L 4966 3500 
L 4050 0 
L 3372 0 
L 2619 2869 
L 1863 0 
L 1184 0 
L 269 3500 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-37" d="M 525 4666 
L 3525 4666 
L 3525 4397 
L 1831 0 
L 1172 0 
L 2766 4134 
L 525 4134 
L 525 4666 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-59"/>
       <use xlink:href="#DejaVuSans-65" x="47.833984"/>
       <use xlink:href="#DejaVuSans-6c" x="109.357422"/>
       <use xlink:href="#DejaVuSans-6c" x="137.140625"/>
       <use xlink:href="#DejaVuSans-6f" x="164.923828"/>
       <use xlink:href="#DejaVuSans-77" x="226.105469"/>
       <use xlink:href="#DejaVuSans-20" x="307.892578"/>
       <use xlink:href="#DejaVuSans-28" x="339.679688"/>
       <use xlink:href="#DejaVuSans-31" x="378.693359"/>
       <use xlink:href="#DejaVuSans-37" x="442.316406"/>
       <use xlink:href="#DejaVuSans-29" x="505.939453"/>
      </g>
     </g>
    </g>
    <g id="text_5">
     <!-- state -->
     <g transform="translate(198.367734 346.316563)scale(0.1 -0.1)">
      <defs>
       <path id="DejaVuSans-73" d="M 2834 3397 
L 2834 2853 
Q 2591 2978 2328 3040 
Q 2066 3103 1784 3103 
Q 1356 3103 1142 2972 
Q 928 2841 928 2578 
Q 928 2378 1081 2264 
Q 1234 2150 1697 2047 
L 1894 2003 
Q 2506 1872 2764 1633 
Q 3022 1394 3022 966 
Q 3022 478 2636 193 
Q 2250 -91 1575 -91 
Q 1294 -91 989 -36 
Q 684 19 347 128 
L 347 722 
Q 666 556 975 473 
Q 1284 391 1588 391 
Q 1994 391 2212 530 
Q 2431 669 2431 922 
Q 2431 1156 2273 1281 
Q 2116 1406 1581 1522 
L 1381 1569 
Q 847 1681 609 1914 
Q 372 2147 372 2553 
Q 372 3047 722 3315 
Q 1072 3584 1716 3584 
Q 2034 3584 2315 3537 
Q 2597 3491 2834 3397 
z
" transform="scale(0.015625)"/>
       <path id="DejaVuSans-74" d="M 1172 4494 
L 1172 3500 
L 2356 3500 
L 2356 3053 
L 1172 3053 
L 1172 1153 
Q 1172 725 1289 603 
Q 1406 481 1766 481 
L 2356 481 
L 2356 0 
L 1766 0 
Q 1100 0 847 248 
Q 594 497 594 1153 
L 594 3053 
L 172 3053 
L 172 3500 
L 594 3500 
L 594 4494 
L 1172 4494 
z
" transform="scale(0.015625)"/>
       <path id="DejaVuSans-61" d="M 2194 1759 
Q 1497 1759 1228 1600 
Q 959 1441 959 1056 
Q 959 750 1161 570 
Q 1363 391 1709 391 
Q 2188 391 2477 730 
Q 2766 1069 2766 1631 
L 2766 1759 
L 2194 1759 
z
M 3341 1997 
L 3341 0 
L 2766 0 
L 2766 531 
Q 2569 213 2275 61 
Q 1981 -91 1556 -91 
Q 1019 -91 701 211 
Q 384 513 384 1019 
Q 384 1609 779 1909 
Q 1175 2209 1959 2209 
L 2766 2209 
L 2766 2266 
Q 2766 2663 2505 2880 
Q 2244 3097 1772 3097 
Q 1472 3097 1187 3025 
Q 903 2953 641 2809 
L 641 3341 
Q 956 3463 1253 3523 
Q 1550 3584 1831 3584 
Q 2591 3584 2966 3190 
Q 3341 2797 3341 1997 
z
" transform="scale(0.015625)"/>
      </defs>
      <use xlink:href="#DejaVuSans-73"/>
      <use xlink:href="#DejaVuSans-74" x="52.099609"/>
      <use xlink:href="#DejaVuSans-61" x="91.308594"/>
      <use xlink:href="#DejaVuSans-74" x="152.587891"/>
      <use xlink:href="#DejaVuSans-65" x="191.796875"/>
     </g>
    </g>
   </g>
   <g id="matplotlib.axis_2">
    <g id="ytick_1">
     <g id="line2d_5">
      <defs>
       <path id="m4d544a2910" d="M 0 0 
L -3.5 0 
" style="stroke: #000000; stroke-width: 0.8"/>
      </defs>
      <g>
       <use xlink:href="#m4d544a2910" x="49.646695" y="302.037134" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_6">
      <!-- 0.4 -->
      <g transform="translate(26.74357 305.836352)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-30" d="M 2034 4250 
Q 1547 4250 1301 3770 
Q 1056 3291 1056 2328 
Q 1056 1369 1301 889 
Q 1547 409 2034 409 
Q 2525 409 2770 889 
Q 3016 1369 3016 2328 
Q 3016 3291 2770 3770 
Q 2525 4250 2034 4250 
z
M 2034 4750 
Q 2819 4750 3233 4129 
Q 3647 3509 3647 2328 
Q 3647 1150 3233 529 
Q 2819 -91 2034 -91 
Q 1250 -91 836 529 
Q 422 1150 422 2328 
Q 422 3509 836 4129 
Q 1250 4750 2034 4750 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-2e" d="M 684 794 
L 1344 794 
L 1344 0 
L 684 0 
L 684 794 
z
" transform="scale(0.015625)"/>
        <path id="DejaVuSans-34" d="M 2419 4116 
L 825 1625 
L 2419 1625 
L 2419 4116 
z
M 2253 4666 
L 3047 4666 
L 3047 1625 
L 3713 1625 
L 3713 1100 
L 3047 1100 
L 3047 0 
L 2419 0 
L 2419 1100 
L 313 1100 
L 313 1709 
L 2253 4666 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-30"/>
       <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
       <use xlink:href="#DejaVuSans-34" x="95.410156"/>
      </g>
     </g>
    </g>
    <g id="ytick_2">
     <g id="line2d_6">
      <g>
       <use xlink:href="#m4d544a2910" x="49.646695" y="257.97984" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_7">
      <!-- 0.5 -->
      <g transform="translate(26.74357 261.779058)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
       <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
       <use xlink:href="#DejaVuSans-35" x="95.410156"/>
      </g>
     </g>
    </g>
    <g id="ytick_3">
     <g id="line2d_7">
      <g>
       <use xlink:href="#m4d544a2910" x="49.646695" y="213.922546" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_8">
      <!-- 0.6 -->
      <g transform="translate(26.74357 217.721764)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
       <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
       <use xlink:href="#DejaVuSans-36" x="95.410156"/>
      </g>
     </g>
    </g>
    <g id="ytick_4">
     <g id="line2d_8">
      <g>
       <use xlink:href="#m4d544a2910" x="49.646695" y="169.865251" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_9">
      <!-- 0.7 -->
      <g transform="translate(26.74357 173.66447)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-30"/>
       <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
       <use xlink:href="#DejaVuSans-37" x="95.410156"/>
      </g>
     </g>
    </g>
    <g id="ytick_5">
     <g id="line2d_9">
      <g>
       <use xlink:href="#m4d544a2910" x="49.646695" y="125.807957" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_10">
      <!-- 0.8 -->
      <g transform="translate(26.74357 129.607176)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-38" d="M 2034 2216 
Q 1584 2216 1326 1975 
Q 1069 1734 1069 1313 
Q 1069 891 1326 650 
Q 1584 409 2034 409 
Q 2484 409 2743 651 
Q 3003 894 3003 1313 
Q 3003 1734 2745 1975 
Q 2488 2216 2034 2216 
z
M 1403 2484 
Q 997 2584 770 2862 
Q 544 3141 544 3541 
Q 544 4100 942 4425 
Q 1341 4750 2034 4750 
Q 2731 4750 3128 4425 
Q 3525 4100 3525 3541 
Q 3525 3141 3298 2862 
Q 3072 2584 2669 2484 
Q 3125 2378 3379 2068 
Q 3634 1759 3634 1313 
Q 3634 634 3220 271 
Q 2806 -91 2034 -91 
Q 1263 -91 848 271 
Q 434 634 434 1313 
Q 434 1759 690 2068 
Q 947 2378 1403 2484 
z
M 1172 3481 
Q 1172 3119 1398 2916 
Q 1625 2713 2034 2713 
Q 2441 2713 2670 2916 
Q 2900 3119 2900 3481 
Q 2900 3844 2670 4047 
Q 2441 4250 2034 4250 
Q 1625 4250 1398 4047 
Q 1172 3844 1172 3481 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-30"/>
       <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
       <use xlink:href="#DejaVuSans-38" x="95.410156"/>
      </g>
     </g>
    </g>
    <g id="ytick_6">
     <g id="line2d_10">
      <g>
       <use xlink:href="#m4d544a2910" x="49.646695" y="81.750663" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_11">
      <!-- 0.9 -->
      <g transform="translate(26.74357 85.549882)scale(0.1 -0.1)">
       <defs>
        <path id="DejaVuSans-39" d="M 703 97 
L 703 672 
Q 941 559 1184 500 
Q 1428 441 1663 441 
Q 2288 441 2617 861 
Q 2947 1281 2994 2138 
Q 2813 1869 2534 1725 
Q 2256 1581 1919 1581 
Q 1219 1581 811 2004 
Q 403 2428 403 3163 
Q 403 3881 828 4315 
Q 1253 4750 1959 4750 
Q 2769 4750 3195 4129 
Q 3622 3509 3622 2328 
Q 3622 1225 3098 567 
Q 2575 -91 1691 -91 
Q 1453 -91 1209 -44 
Q 966 3 703 97 
z
M 1959 2075 
Q 2384 2075 2632 2365 
Q 2881 2656 2881 3163 
Q 2881 3666 2632 3958 
Q 2384 4250 1959 4250 
Q 1534 4250 1286 3958 
Q 1038 3666 1038 3163 
Q 1038 2656 1286 2365 
Q 1534 2075 1959 2075 
z
" transform="scale(0.015625)"/>
       </defs>
       <use xlink:href="#DejaVuSans-30"/>
       <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
       <use xlink:href="#DejaVuSans-39" x="95.410156"/>
      </g>
     </g>
    </g>
    <g id="ytick_7">
     <g id="line2d_11">
      <g>
       <use xlink:href="#m4d544a2910" x="49.646695" y="37.693369" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_12">
      <!-- 1.0 -->
      <g transform="translate(26.74357 41.492588)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-31"/>
       <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
       <use xlink:href="#DejaVuSans-30" x="95.410156"/>
      </g>
     </g>
    </g>
    <g id="text_13">
     <!-- Choice accuracy -->
     <g transform="translate(20.663883 212.462813)rotate(-90)scale(0.1 -0.1)">
      <defs>
       <path id="DejaVuSans-43" d="M 4122 4306 
L 4122 3641 
Q 3803 3938 3442 4084 
Q 3081 4231 2675 4231 
Q 1875 4231 1450 3742 
Q 1025 3253 1025 2328 
Q 1025 1406 1450 917 
Q 1875 428 2675 428 
Q 3081 428 3442 575 
Q 3803 722 4122 1019 
L 4122 359 
Q 3791 134 3420 21 
Q 3050 -91 2638 -91 
Q 1578 -91 968 557 
Q 359 1206 359 2328 
Q 359 3453 968 4101 
Q 1578 4750 2638 4750 
Q 3056 4750 3426 4639 
Q 3797 4528 4122 4306 
z
" transform="scale(0.015625)"/>
       <path id="DejaVuSans-68" d="M 3513 2113 
L 3513 0 
L 2938 0 
L 2938 2094 
Q 2938 2591 2744 2837 
Q 2550 3084 2163 3084 
Q 1697 3084 1428 2787 
Q 1159 2491 1159 1978 
L 1159 0 
L 581 0 
L 581 4863 
L 1159 4863 
L 1159 2956 
Q 1366 3272 1645 3428 
Q 1925 3584 2291 3584 
Q 2894 3584 3203 3211 
Q 3513 2838 3513 2113 
z
" transform="scale(0.015625)"/>
       <path id="DejaVuSans-69" d="M 603 3500 
L 1178 3500 
L 1178 0 
L 603 0 
L 603 3500 
z
M 603 4863 
L 1178 4863 
L 1178 4134 
L 603 4134 
L 603 4863 
z
" transform="scale(0.015625)"/>
       <path id="DejaVuSans-63" d="M 3122 3366 
L 3122 2828 
Q 2878 2963 2633 3030 
Q 2388 3097 2138 3097 
Q 1578 3097 1268 2742 
Q 959 2388 959 1747 
Q 959 1106 1268 751 
Q 1578 397 2138 397 
Q 2388 397 2633 464 
Q 2878 531 3122 666 
L 3122 134 
Q 2881 22 2623 -34 
Q 2366 -91 2075 -91 
Q 1284 -91 818 406 
Q 353 903 353 1747 
Q 353 2603 823 3093 
Q 1294 3584 2113 3584 
Q 2378 3584 2631 3529 
Q 2884 3475 3122 3366 
z
" transform="scale(0.015625)"/>
       <path id="DejaVuSans-79" d="M 2059 -325 
Q 1816 -950 1584 -1140 
Q 1353 -1331 966 -1331 
L 506 -1331 
L 506 -850 
L 844 -850 
Q 1081 -850 1212 -737 
Q 1344 -625 1503 -206 
L 1606 56 
L 191 3500 
L 800 3500 
L 1894 763 
L 2988 3500 
L 3597 3500 
L 2059 -325 
z
" transform="scale(0.015625)"/>
      </defs>
      <use xlink:href="#DejaVuSans-43"/>
      <use xlink:href="#DejaVuSans-68" x="69.824219"/>
      <use xlink:href="#DejaVuSans-6f" x="133.203125"/>
      <use xlink:href="#DejaVuSans-69" x="194.384766"/>
      <use xlink:href="#DejaVuSans-63" x="222.167969"/>
      <use xlink:href="#DejaVuSans-65" x="277.148438"/>
      <use xlink:href="#DejaVuSans-20" x="338.671875"/>
      <use xlink:href="#DejaVuSans-61" x="370.458984"/>
      <use xlink:href="#DejaVuSans-63" x="431.738281"/>
      <use xlink:href="#DejaVuSans-63" x="486.71875"/>
      <use xlink:href="#DejaVuSans-75" x="541.699219"/>
      <use xlink:href="#DejaVuSans-72" x="605.078125"/>
      <use xlink:href="#DejaVuSans-61" x="646.191406"/>
      <use xlink:href="#DejaVuSans-63" x="707.470703"/>
      <use xlink:href="#DejaVuSans-79" x="762.451172"/>
     </g>
    </g>
   </g>
   <g id="line2d_12">
    <path d="M 64.31821 39.991703 
L 162.128309 168.651074 
L 259.938409 47.831765 
L 357.748508 116.017448 
" clip-path="url(#p62ac668042)" style="fill: none; stroke: #edd1cb; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="line2d_13">
    <path d="M 64.31821 40.947498 
L 162.128309 47.083911 
L 259.938409 46.131762 
L 357.748508 202.908222 
" clip-path="url(#p62ac668042)" style="fill: none; stroke: #e9c6c2; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="line2d_14">
    <path d="M 64.31821 38.951122 
L 162.128309 46.536987 
L 259.938409 47.345287 
L 357.748508 224.603102 
" clip-path="url(#p62ac668042)" style="fill: none; stroke: #dfaeb2; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="line2d_15">
    <path d="M 64.31821 38.921569 
L 162.128309 52.108844 
L 259.938409 49.65663 
L 357.748508 193.189701 
" clip-path="url(#p62ac668042)" style="fill: none; stroke: #af6c91; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="line2d_16">
    <path d="M 64.31821 40.37434 
L 162.128309 62.398394 
L 259.938409 54.655651 
L 357.748508 173.254274 
" clip-path="url(#p62ac668042)" style="fill: none; stroke: #2d1e3e; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="line2d_17"/>
   <g id="line2d_18"/>
   <g id="line2d_19"/>
   <g id="line2d_20"/>
   <g id="line2d_21"/>
   <g id="patch_3">
    <path d="M 49.646695 318.04 
L 49.646695 24.72 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_4">
    <path d="M 49.646695 318.04 
L 372.420023 318.04 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="text_14">
    <!-- sigma_base = 10.0 -->
    <g transform="translate(163.010703 18.72)scale(0.1 -0.1)">
     <defs>
      <path id="DejaVuSans-67" d="M 2906 1791 
Q 2906 2416 2648 2759 
Q 2391 3103 1925 3103 
Q 1463 3103 1205 2759 
Q 947 2416 947 1791 
Q 947 1169 1205 825 
Q 1463 481 1925 481 
Q 2391 481 2648 825 
Q 2906 1169 2906 1791 
z
M 3481 434 
Q 3481 -459 3084 -895 
Q 2688 -1331 1869 -1331 
Q 1566 -1331 1297 -1286 
Q 1028 -1241 775 -1147 
L 775 -588 
Q 1028 -725 1275 -790 
Q 1522 -856 1778 -856 
Q 2344 -856 2625 -561 
Q 2906 -266 2906 331 
L 2906 616 
Q 2728 306 2450 153 
Q 2172 0 1784 0 
Q 1141 0 747 490 
Q 353 981 353 1791 
Q 353 2603 747 3093 
Q 1141 3584 1784 3584 
Q 2172 3584 2450 3431 
Q 2728 3278 2906 2969 
L 2906 3500 
L 3481 3500 
L 3481 434 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-6d" d="M 3328 2828 
Q 3544 3216 3844 3400 
Q 4144 3584 4550 3584 
Q 5097 3584 5394 3201 
Q 5691 2819 5691 2113 
L 5691 0 
L 5113 0 
L 5113 2094 
Q 5113 2597 4934 2840 
Q 4756 3084 4391 3084 
Q 3944 3084 3684 2787 
Q 3425 2491 3425 1978 
L 3425 0 
L 2847 0 
L 2847 2094 
Q 2847 2600 2669 2842 
Q 2491 3084 2119 3084 
Q 1678 3084 1418 2786 
Q 1159 2488 1159 1978 
L 1159 0 
L 581 0 
L 581 3500 
L 1159 3500 
L 1159 2956 
Q 1356 3278 1631 3431 
Q 1906 3584 2284 3584 
Q 2666 3584 2933 3390 
Q 3200 3197 3328 2828 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-5f" d="M 3263 -1063 
L 3263 -1509 
L -63 -1509 
L -63 -1063 
L 3263 -1063 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-62" d="M 3116 1747 
Q 3116 2381 2855 2742 
Q 2594 3103 2138 3103 
Q 1681 3103 1420 2742 
Q 1159 2381 1159 1747 
Q 1159 1113 1420 752 
Q 1681 391 2138 391 
Q 2594 391 2855 752 
Q 3116 1113 3116 1747 
z
M 1159 2969 
Q 1341 3281 1617 3432 
Q 1894 3584 2278 3584 
Q 2916 3584 3314 3078 
Q 3713 2572 3713 1747 
Q 3713 922 3314 415 
Q 2916 -91 2278 -91 
Q 1894 -91 1617 61 
Q 1341 213 1159 525 
L 1159 0 
L 581 0 
L 581 4863 
L 1159 4863 
L 1159 2969 
z
" transform="scale(0.015625)"/>
      <path id="DejaVuSans-3d" d="M 678 2906 
L 4684 2906 
L 4684 2381 
L 678 2381 
L 678 2906 
z
M 678 1631 
L 4684 1631 
L 4684 1100 
L 678 1100 
L 678 1631 
z
" transform="scale(0.015625)"/>
     </defs>
     <use xlink:href="#DejaVuSans-73"/>
     <use xlink:href="#DejaVuSans-69" x="52.099609"/>
     <use xlink:href="#DejaVuSans-67" x="79.882812"/>
     <use xlink:href="#DejaVuSans-6d" x="143.359375"/>
     <use xlink:href="#DejaVuSans-61" x="240.771484"/>
     <use xlink:href="#DejaVuSans-5f" x="302.050781"/>
     <use xlink:href="#DejaVuSans-62" x="352.050781"/>
     <use xlink:href="#DejaVuSans-61" x="415.527344"/>
     <use xlink:href="#DejaVuSans-73" x="476.806641"/>
     <use xlink:href="#DejaVuSans-65" x="528.90625"/>
     <use xlink:href="#DejaVuSans-20" x="590.429688"/>
     <use xlink:href="#DejaVuSans-3d" x="622.216797"/>
     <use xlink:href="#DejaVuSans-20" x="706.005859"/>
     <use xlink:href="#DejaVuSans-31" x="737.792969"/>
     <use xlink:href="#DejaVuSans-30" x="801.416016"/>
     <use xlink:href="#DejaVuSans-2e" x="865.039062"/>
     <use xlink:href="#DejaVuSans-30" x="896.826172"/>
    </g>
   </g>
  </g>
  <g id="axes_2">
   <g id="patch_5">
    <path d="M 400.512887 318.04 
L 723.286215 318.04 
L 723.286215 24.72 
L 400.512887 24.72 
z
" style="fill: #ffffff"/>
   </g>
   <g id="PolyCollection_6">
    <defs>
     <path id="me66d06e710" d="M 415.184402 -319.04447 
L 415.184402 -315.238616 
L 512.994502 -298.303536 
L 610.804601 -288.830261 
L 708.6147 -110.492717 
L 708.6147 -229.108509 
L 708.6147 -229.108509 
L 610.804601 -296.452113 
L 512.994502 -311.466523 
L 415.184402 -319.04447 
z
" style="stroke: #edd1cb; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#p650cd95429)">
     <use xlink:href="#me66d06e710" x="0" y="360" style="fill: #edd1cb; fill-opacity: 0.2; stroke: #edd1cb; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_7">
    <defs>
     <path id="m13a0abf345" d="M 415.184402 -320.101009 
L 415.184402 -316.976378 
L 512.994502 -297.465039 
L 610.804601 -282.542099 
L 708.6147 -187.197596 
L 708.6147 -269.437878 
L 708.6147 -269.437878 
L 610.804601 -291.278246 
L 512.994502 -311.347105 
L 415.184402 -320.101009 
z
" style="stroke: #e9c6c2; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#p650cd95429)">
     <use xlink:href="#m13a0abf345" x="0" y="360" style="fill: #e9c6c2; fill-opacity: 0.2; stroke: #e9c6c2; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_8">
    <defs>
     <path id="ma0914599c7" d="M 415.184402 -319.725869 
L 415.184402 -316.223406 
L 512.994502 -302.083611 
L 610.804601 -262.442058 
L 708.6147 -203.873045 
L 708.6147 -274.933196 
L 708.6147 -274.933196 
L 610.804601 -272.747858 
L 512.994502 -313.639622 
L 415.184402 -319.725869 
z
" style="stroke: #dfaeb2; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#p650cd95429)">
     <use xlink:href="#ma0914599c7" x="0" y="360" style="fill: #dfaeb2; fill-opacity: 0.2; stroke: #dfaeb2; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_9">
    <defs>
     <path id="m54c2f2742c" d="M 415.184402 -319.693062 
L 415.184402 -316.33276 
L 512.994502 -308.538726 
L 610.804601 -262.729492 
L 708.6147 -122.999824 
L 708.6147 -248.877807 
L 708.6147 -248.877807 
L 610.804601 -273.304434 
L 512.994502 -318.176259 
L 415.184402 -319.693062 
z
" style="stroke: #af6c91; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#p650cd95429)">
     <use xlink:href="#m54c2f2742c" x="0" y="360" style="fill: #af6c91; fill-opacity: 0.2; stroke: #af6c91; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_10">
    <defs>
     <path id="md9c949d157" d="M 415.184402 -318.222094 
L 415.184402 -314.323219 
L 512.994502 -301.929254 
L 610.804601 -240.160804 
L 708.6147 -133.489656 
L 708.6147 -230.759007 
L 708.6147 -230.759007 
L 610.804601 -251.917454 
L 512.994502 -314.577281 
L 415.184402 -318.222094 
z
" style="stroke: #2d1e3e; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#p650cd95429)">
     <use xlink:href="#md9c949d157" x="0" y="360" style="fill: #2d1e3e; fill-opacity: 0.2; stroke: #2d1e3e; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="matplotlib.axis_3">
    <g id="xtick_5">
     <g id="line2d_22">
      <g>
       <use xlink:href="#m2a2fc8578d" x="415.184402" y="318.04" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_15">
      <!-- Blue (5) -->
      <g transform="translate(395.447683 332.638438)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-42"/>
       <use xlink:href="#DejaVuSans-6c" x="68.603516"/>
       <use xlink:href="#DejaVuSans-75" x="96.386719"/>
       <use xlink:href="#DejaVuSans-65" x="159.765625"/>
       <use xlink:href="#DejaVuSans-20" x="221.289062"/>
       <use xlink:href="#DejaVuSans-28" x="253.076172"/>
       <use xlink:href="#DejaVuSans-35" x="292.089844"/>
       <use xlink:href="#DejaVuSans-29" x="355.712891"/>
      </g>
     </g>
    </g>
    <g id="xtick_6">
     <g id="line2d_23">
      <g>
       <use xlink:href="#m2a2fc8578d" x="512.994502" y="318.04" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_16">
      <!-- Green (6) -->
      <g transform="translate(489.183564 332.638438)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-47"/>
       <use xlink:href="#DejaVuSans-72" x="77.490234"/>
       <use xlink:href="#DejaVuSans-65" x="116.353516"/>
       <use xlink:href="#DejaVuSans-65" x="177.876953"/>
       <use xlink:href="#DejaVuSans-6e" x="239.400391"/>
       <use xlink:href="#DejaVuSans-20" x="302.779297"/>
       <use xlink:href="#DejaVuSans-28" x="334.566406"/>
       <use xlink:href="#DejaVuSans-36" x="373.580078"/>
       <use xlink:href="#DejaVuSans-29" x="437.203125"/>
      </g>
     </g>
    </g>
    <g id="xtick_7">
     <g id="line2d_24">
      <g>
       <use xlink:href="#m2a2fc8578d" x="610.804601" y="318.04" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_17">
      <!-- Red (16) -->
      <g transform="translate(589.451476 332.638438)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-52"/>
       <use xlink:href="#DejaVuSans-65" x="64.982422"/>
       <use xlink:href="#DejaVuSans-64" x="126.505859"/>
       <use xlink:href="#DejaVuSans-20" x="189.982422"/>
       <use xlink:href="#DejaVuSans-28" x="221.769531"/>
       <use xlink:href="#DejaVuSans-31" x="260.783203"/>
       <use xlink:href="#DejaVuSans-36" x="324.40625"/>
       <use xlink:href="#DejaVuSans-29" x="388.029297"/>
      </g>
     </g>
    </g>
    <g id="xtick_8">
     <g id="line2d_25">
      <g>
       <use xlink:href="#m2a2fc8578d" x="708.6147" y="318.04" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_18">
      <!-- Yellow (17) -->
      <g transform="translate(681.367044 332.638438)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-59"/>
       <use xlink:href="#DejaVuSans-65" x="47.833984"/>
       <use xlink:href="#DejaVuSans-6c" x="109.357422"/>
       <use xlink:href="#DejaVuSans-6c" x="137.140625"/>
       <use xlink:href="#DejaVuSans-6f" x="164.923828"/>
       <use xlink:href="#DejaVuSans-77" x="226.105469"/>
       <use xlink:href="#DejaVuSans-20" x="307.892578"/>
       <use xlink:href="#DejaVuSans-28" x="339.679688"/>
       <use xlink:href="#DejaVuSans-31" x="378.693359"/>
       <use xlink:href="#DejaVuSans-37" x="442.316406"/>
       <use xlink:href="#DejaVuSans-29" x="505.939453"/>
      </g>
     </g>
    </g>
    <g id="text_19">
     <!-- state -->
     <g transform="translate(549.233926 346.316563)scale(0.1 -0.1)">
      <use xlink:href="#DejaVuSans-73"/>
      <use xlink:href="#DejaVuSans-74" x="52.099609"/>
      <use xlink:href="#DejaVuSans-61" x="91.308594"/>
      <use xlink:href="#DejaVuSans-74" x="152.587891"/>
      <use xlink:href="#DejaVuSans-65" x="191.796875"/>
     </g>
    </g>
   </g>
   <g id="matplotlib.axis_4">
    <g id="ytick_8">
     <g id="line2d_26">
      <g>
       <use xlink:href="#m4d544a2910" x="400.512887" y="302.037134" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
    </g>
    <g id="ytick_9">
     <g id="line2d_27">
      <g>
       <use xlink:href="#m4d544a2910" x="400.512887" y="257.97984" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
    </g>
    <g id="ytick_10">
     <g id="line2d_28">
      <g>
       <use xlink:href="#m4d544a2910" x="400.512887" y="213.922546" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
    </g>
    <g id="ytick_11">
     <g id="line2d_29">
      <g>
       <use xlink:href="#m4d544a2910" x="400.512887" y="169.865251" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
    </g>
    <g id="ytick_12">
     <g id="line2d_30">
      <g>
       <use xlink:href="#m4d544a2910" x="400.512887" y="125.807957" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
    </g>
    <g id="ytick_13">
     <g id="line2d_31">
      <g>
       <use xlink:href="#m4d544a2910" x="400.512887" y="81.750663" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
    </g>
    <g id="ytick_14">
     <g id="line2d_32">
      <g>
       <use xlink:href="#m4d544a2910" x="400.512887" y="37.693369" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
    </g>
   </g>
   <g id="line2d_33">
    <path d="M 415.184402 42.767842 
L 512.994502 54.727824 
L 610.804601 67.433537 
L 708.6147 190.199387 
" clip-path="url(#p650cd95429)" style="fill: none; stroke: #edd1cb; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="line2d_34">
    <path d="M 415.184402 41.553207 
L 512.994502 55.228611 
L 610.804601 73.089828 
L 708.6147 131.682263 
" clip-path="url(#p650cd95429)" style="fill: none; stroke: #e9c6c2; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="line2d_35">
    <path d="M 415.184402 41.933193 
L 512.994502 52.138384 
L 610.804601 92.556598 
L 708.6147 118.228208 
" clip-path="url(#p650cd95429)" style="fill: none; stroke: #dfaeb2; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="line2d_36">
    <path d="M 415.184402 41.800405 
L 512.994502 46.642507 
L 610.804601 92.057508 
L 708.6147 174.061184 
" clip-path="url(#p650cd95429)" style="fill: none; stroke: #af6c91; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="line2d_37">
    <path d="M 415.184402 43.634513 
L 512.994502 51.746733 
L 610.804601 113.960871 
L 708.6147 175.014805 
" clip-path="url(#p650cd95429)" style="fill: none; stroke: #2d1e3e; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="patch_6">
    <path d="M 400.512887 318.04 
L 400.512887 24.72 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_7">
    <path d="M 400.512887 318.04 
L 723.286215 318.04 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="text_20">
    <!-- sigma_base = 20.0 -->
    <g transform="translate(513.876895 18.72)scale(0.1 -0.1)">
     <defs>
      <path id="DejaVuSans-32" d="M 1228 531 
L 3431 531 
L 3431 0 
L 469 0 
L 469 531 
Q 828 903 1448 1529 
Q 2069 2156 2228 2338 
Q 2531 2678 2651 2914 
Q 2772 3150 2772 3378 
Q 2772 3750 2511 3984 
Q 2250 4219 1831 4219 
Q 1534 4219 1204 4116 
Q 875 4013 500 3803 
L 500 4441 
Q 881 4594 1212 4672 
Q 1544 4750 1819 4750 
Q 2544 4750 2975 4387 
Q 3406 4025 3406 3419 
Q 3406 3131 3298 2873 
Q 3191 2616 2906 2266 
Q 2828 2175 2409 1742 
Q 1991 1309 1228 531 
z
" transform="scale(0.015625)"/>
     </defs>
     <use xlink:href="#DejaVuSans-73"/>
     <use xlink:href="#DejaVuSans-69" x="52.099609"/>
     <use xlink:href="#DejaVuSans-67" x="79.882812"/>
     <use xlink:href="#DejaVuSans-6d" x="143.359375"/>
     <use xlink:href="#DejaVuSans-61" x="240.771484"/>
     <use xlink:href="#DejaVuSans-5f" x="302.050781"/>
     <use xlink:href="#DejaVuSans-62" x="352.050781"/>
     <use xlink:href="#DejaVuSans-61" x="415.527344"/>
     <use xlink:href="#DejaVuSans-73" x="476.806641"/>
     <use xlink:href="#DejaVuSans-65" x="528.90625"/>
     <use xlink:href="#DejaVuSans-20" x="590.429688"/>
     <use xlink:href="#DejaVuSans-3d" x="622.216797"/>
     <use xlink:href="#DejaVuSans-20" x="706.005859"/>
     <use xlink:href="#DejaVuSans-32" x="737.792969"/>
     <use xlink:href="#DejaVuSans-30" x="801.416016"/>
     <use xlink:href="#DejaVuSans-2e" x="865.039062"/>
     <use xlink:href="#DejaVuSans-30" x="896.826172"/>
    </g>
   </g>
  </g>
  <g id="axes_3">
   <g id="patch_8">
    <path d="M 751.37908 318.04 
L 1074.152408 318.04 
L 1074.152408 24.72 
L 751.37908 24.72 
z
" style="fill: #ffffff"/>
   </g>
   <g id="PolyCollection_11">
    <defs>
     <path id="m6cba273577" d="M 766.050595 -286.16371 
L 766.050595 -276.340249 
L 863.860694 -274.200733 
L 961.670793 -205.252902 
L 1059.480893 -136.486751 
L 1059.480893 -205.419932 
L 1059.480893 -205.419932 
L 961.670793 -219.614375 
L 863.860694 -293.312264 
L 766.050595 -286.16371 
z
" style="stroke: #edd1cb; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#pcb11c65ebd)">
     <use xlink:href="#m6cba273577" x="0" y="360" style="fill: #edd1cb; fill-opacity: 0.2; stroke: #edd1cb; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_12">
    <defs>
     <path id="mafcd91e5d7" d="M 766.050595 -288.444564 
L 766.050595 -278.74354 
L 863.860694 -219.778695 
L 961.670793 -200.479737 
L 1059.480893 -209.64871 
L 1059.480893 -255.918927 
L 1059.480893 -255.918927 
L 961.670793 -215.213028 
L 863.860694 -248.753981 
L 766.050595 -288.444564 
z
" style="stroke: #e9c6c2; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#pcb11c65ebd)">
     <use xlink:href="#mafcd91e5d7" x="0" y="360" style="fill: #e9c6c2; fill-opacity: 0.2; stroke: #e9c6c2; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_13">
    <defs>
     <path id="m8c6b8c105e" d="M 766.050595 -279.546752 
L 766.050595 -269.313618 
L 863.860694 -234.790444 
L 961.670793 -204.975918 
L 1059.480893 -162.098289 
L 1059.480893 -223.511486 
L 1059.480893 -223.511486 
L 961.670793 -218.962361 
L 863.860694 -261.718501 
L 766.050595 -279.546752 
z
" style="stroke: #dfaeb2; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#pcb11c65ebd)">
     <use xlink:href="#m8c6b8c105e" x="0" y="360" style="fill: #dfaeb2; fill-opacity: 0.2; stroke: #dfaeb2; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_14">
    <defs>
     <path id="mcdf01c0ac6" d="M 766.050595 -286.489201 
L 766.050595 -275.910997 
L 863.860694 -252.631357 
L 961.670793 -200.456197 
L 1059.480893 -190.511307 
L 1059.480893 -239.463856 
L 1059.480893 -239.463856 
L 961.670793 -214.472979 
L 863.860694 -275.152658 
L 766.050595 -286.489201 
z
" style="stroke: #af6c91; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#pcb11c65ebd)">
     <use xlink:href="#mcdf01c0ac6" x="0" y="360" style="fill: #af6c91; fill-opacity: 0.2; stroke: #af6c91; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_15">
    <defs>
     <path id="ma0036e6d01" d="M 766.050595 -300.761829 
L 766.050595 -292.728174 
L 863.860694 -211.225208 
L 961.670793 -202.757411 
L 1059.480893 -172.476157 
L 1059.480893 -222.419648 
L 1059.480893 -222.419648 
L 961.670793 -217.648553 
L 863.860694 -239.746114 
L 766.050595 -300.761829 
z
" style="stroke: #2d1e3e; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#pcb11c65ebd)">
     <use xlink:href="#ma0036e6d01" x="0" y="360" style="fill: #2d1e3e; fill-opacity: 0.2; stroke: #2d1e3e; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="matplotlib.axis_5">
    <g id="xtick_9">
     <g id="line2d_38">
      <g>
       <use xlink:href="#m2a2fc8578d" x="766.050595" y="318.04" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_21">
      <!-- Blue (5) -->
      <g transform="translate(746.313876 332.638438)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-42"/>
       <use xlink:href="#DejaVuSans-6c" x="68.603516"/>
       <use xlink:href="#DejaVuSans-75" x="96.386719"/>
       <use xlink:href="#DejaVuSans-65" x="159.765625"/>
       <use xlink:href="#DejaVuSans-20" x="221.289062"/>
       <use xlink:href="#DejaVuSans-28" x="253.076172"/>
       <use xlink:href="#DejaVuSans-35" x="292.089844"/>
       <use xlink:href="#DejaVuSans-29" x="355.712891"/>
      </g>
     </g>
    </g>
    <g id="xtick_10">
     <g id="line2d_39">
      <g>
       <use xlink:href="#m2a2fc8578d" x="863.860694" y="318.04" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_22">
      <!-- Green (6) -->
      <g transform="translate(840.049756 332.638438)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-47"/>
       <use xlink:href="#DejaVuSans-72" x="77.490234"/>
       <use xlink:href="#DejaVuSans-65" x="116.353516"/>
       <use xlink:href="#DejaVuSans-65" x="177.876953"/>
       <use xlink:href="#DejaVuSans-6e" x="239.400391"/>
       <use xlink:href="#DejaVuSans-20" x="302.779297"/>
       <use xlink:href="#DejaVuSans-28" x="334.566406"/>
       <use xlink:href="#DejaVuSans-36" x="373.580078"/>
       <use xlink:href="#DejaVuSans-29" x="437.203125"/>
      </g>
     </g>
    </g>
    <g id="xtick_11">
     <g id="line2d_40">
      <g>
       <use xlink:href="#m2a2fc8578d" x="961.670793" y="318.04" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_23">
      <!-- Red (16) -->
      <g transform="translate(940.317668 332.638438)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-52"/>
       <use xlink:href="#DejaVuSans-65" x="64.982422"/>
       <use xlink:href="#DejaVuSans-64" x="126.505859"/>
       <use xlink:href="#DejaVuSans-20" x="189.982422"/>
       <use xlink:href="#DejaVuSans-28" x="221.769531"/>
       <use xlink:href="#DejaVuSans-31" x="260.783203"/>
       <use xlink:href="#DejaVuSans-36" x="324.40625"/>
       <use xlink:href="#DejaVuSans-29" x="388.029297"/>
      </g>
     </g>
    </g>
    <g id="xtick_12">
     <g id="line2d_41">
      <g>
       <use xlink:href="#m2a2fc8578d" x="1059.480893" y="318.04" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_24">
      <!-- Yellow (17) -->
      <g transform="translate(1032.233237 332.638438)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-59"/>
       <use xlink:href="#DejaVuSans-65" x="47.833984"/>
       <use xlink:href="#DejaVuSans-6c" x="109.357422"/>
       <use xlink:href="#DejaVuSans-6c" x="137.140625"/>
       <use xlink:href="#DejaVuSans-6f" x="164.923828"/>
       <use xlink:href="#DejaVuSans-77" x="226.105469"/>
       <use xlink:href="#DejaVuSans-20" x="307.892578"/>
       <use xlink:href="#DejaVuSans-28" x="339.679688"/>
       <use xlink:href="#DejaVuSans-31" x="378.693359"/>
       <use xlink:href="#DejaVuSans-37" x="442.316406"/>
       <use xlink:href="#DejaVuSans-29" x="505.939453"/>
      </g>
     </g>
    </g>
    <g id="text_25">
     <!-- state -->
     <g transform="translate(900.100119 346.316563)scale(0.1 -0.1)">
      <use xlink:href="#DejaVuSans-73"/>
      <use xlink:href="#DejaVuSans-74" x="52.099609"/>
      <use xlink:href="#DejaVuSans-61" x="91.308594"/>
      <use xlink:href="#DejaVuSans-74" x="152.587891"/>
      <use xlink:href="#DejaVuSans-65" x="191.796875"/>
     </g>
    </g>
   </g>
   <g id="matplotlib.axis_6">
    <g id="ytick_15">
     <g id="line2d_42">
      <g>
       <use xlink:href="#m4d544a2910" x="751.37908" y="302.037134" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
    </g>
    <g id="ytick_16">
     <g id="line2d_43">
      <g>
       <use xlink:href="#m4d544a2910" x="751.37908" y="257.97984" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
    </g>
    <g id="ytick_17">
     <g id="line2d_44">
      <g>
       <use xlink:href="#m4d544a2910" x="751.37908" y="213.922546" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
    </g>
    <g id="ytick_18">
     <g id="line2d_45">
      <g>
       <use xlink:href="#m4d544a2910" x="751.37908" y="169.865251" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
    </g>
    <g id="ytick_19">
     <g id="line2d_46">
      <g>
       <use xlink:href="#m4d544a2910" x="751.37908" y="125.807957" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
    </g>
    <g id="ytick_20">
     <g id="line2d_47">
      <g>
       <use xlink:href="#m4d544a2910" x="751.37908" y="81.750663" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
    </g>
    <g id="ytick_21">
     <g id="line2d_48">
      <g>
       <use xlink:href="#m4d544a2910" x="751.37908" y="37.693369" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
    </g>
   </g>
   <g id="line2d_49">
    <path d="M 766.050595 78.655346 
L 863.860694 75.881072 
L 961.670793 147.643574 
L 1059.480893 187.548111 
" clip-path="url(#pcb11c65ebd)" style="fill: none; stroke: #edd1cb; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="line2d_50">
    <path d="M 766.050595 75.948352 
L 863.860694 125.362184 
L 961.670793 152.232828 
L 1059.480893 126.210307 
" clip-path="url(#pcb11c65ebd)" style="fill: none; stroke: #e9c6c2; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="line2d_51">
    <path d="M 766.050595 85.569815 
L 863.860694 110.249524 
L 961.670793 147.875456 
L 1059.480893 168.530182 
" clip-path="url(#pcb11c65ebd)" style="fill: none; stroke: #dfaeb2; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="line2d_52">
    <path d="M 766.050595 78.892692 
L 863.860694 96.107992 
L 961.670793 152.216849 
L 1059.480893 143.129629 
" clip-path="url(#pcb11c65ebd)" style="fill: none; stroke: #af6c91; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="line2d_53">
    <path d="M 766.050595 63.254999 
L 863.860694 134.514339 
L 961.670793 149.396934 
L 1059.480893 162.552097 
" clip-path="url(#pcb11c65ebd)" style="fill: none; stroke: #2d1e3e; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="patch_9">
    <path d="M 751.37908 318.04 
L 751.37908 24.72 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_10">
    <path d="M 751.37908 318.04 
L 1074.152408 318.04 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="text_26">
    <!-- sigma_base = 30.0 -->
    <g transform="translate(864.743087 18.72)scale(0.1 -0.1)">
     <defs>
      <path id="DejaVuSans-33" d="M 2597 2516 
Q 3050 2419 3304 2112 
Q 3559 1806 3559 1356 
Q 3559 666 3084 287 
Q 2609 -91 1734 -91 
Q 1441 -91 1130 -33 
Q 819 25 488 141 
L 488 750 
Q 750 597 1062 519 
Q 1375 441 1716 441 
Q 2309 441 2620 675 
Q 2931 909 2931 1356 
Q 2931 1769 2642 2001 
Q 2353 2234 1838 2234 
L 1294 2234 
L 1294 2753 
L 1863 2753 
Q 2328 2753 2575 2939 
Q 2822 3125 2822 3475 
Q 2822 3834 2567 4026 
Q 2313 4219 1838 4219 
Q 1578 4219 1281 4162 
Q 984 4106 628 3988 
L 628 4550 
Q 988 4650 1302 4700 
Q 1616 4750 1894 4750 
Q 2613 4750 3031 4423 
Q 3450 4097 3450 3541 
Q 3450 3153 3228 2886 
Q 3006 2619 2597 2516 
z
" transform="scale(0.015625)"/>
     </defs>
     <use xlink:href="#DejaVuSans-73"/>
     <use xlink:href="#DejaVuSans-69" x="52.099609"/>
     <use xlink:href="#DejaVuSans-67" x="79.882812"/>
     <use xlink:href="#DejaVuSans-6d" x="143.359375"/>
     <use xlink:href="#DejaVuSans-61" x="240.771484"/>
     <use xlink:href="#DejaVuSans-5f" x="302.050781"/>
     <use xlink:href="#DejaVuSans-62" x="352.050781"/>
     <use xlink:href="#DejaVuSans-61" x="415.527344"/>
     <use xlink:href="#DejaVuSans-73" x="476.806641"/>
     <use xlink:href="#DejaVuSans-65" x="528.90625"/>
     <use xlink:href="#DejaVuSans-20" x="590.429688"/>
     <use xlink:href="#DejaVuSans-3d" x="622.216797"/>
     <use xlink:href="#DejaVuSans-20" x="706.005859"/>
     <use xlink:href="#DejaVuSans-33" x="737.792969"/>
     <use xlink:href="#DejaVuSans-30" x="801.416016"/>
     <use xlink:href="#DejaVuSans-2e" x="865.039062"/>
     <use xlink:href="#DejaVuSans-30" x="896.826172"/>
    </g>
   </g>
  </g>
  <g id="axes_4">
   <g id="patch_11">
    <path d="M 1102.245272 318.04 
L 1425.0186 318.04 
L 1425.0186 24.72 
L 1102.245272 24.72 
z
" style="fill: #ffffff"/>
   </g>
   <g id="PolyCollection_16">
    <defs>
     <path id="m47a7e6634d" d="M 1116.916787 -254.363316 
L 1116.916787 -241.077799 
L 1214.726886 -162.436281 
L 1312.536986 -166.648954 
L 1410.347085 -181.912428 
L 1410.347085 -217.798631 
L 1410.347085 -217.798631 
L 1312.536986 -182.934724 
L 1214.726886 -192.799254 
L 1116.916787 -254.363316 
z
" style="stroke: #edd1cb; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#pcd56c48886)">
     <use xlink:href="#m47a7e6634d" x="0" y="360" style="fill: #edd1cb; fill-opacity: 0.2; stroke: #edd1cb; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_17">
    <defs>
     <path id="me1a2eb475b" d="M 1116.916787 -242.508981 
L 1116.916787 -228.455416 
L 1214.726886 -203.307743 
L 1312.536986 -179.245686 
L 1410.347085 -166.613049 
L 1410.347085 -203.865939 
L 1410.347085 -203.865939 
L 1312.536986 -195.663507 
L 1214.726886 -231.217495 
L 1116.916787 -242.508981 
z
" style="stroke: #e9c6c2; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#pcd56c48886)">
     <use xlink:href="#me1a2eb475b" x="0" y="360" style="fill: #e9c6c2; fill-opacity: 0.2; stroke: #e9c6c2; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_18">
    <defs>
     <path id="m72770aeeb3" d="M 1116.916787 -254.643802 
L 1116.916787 -240.685954 
L 1214.726886 -190.451707 
L 1312.536986 -170.322476 
L 1410.347085 -162.303159 
L 1410.347085 -202.867419 
L 1410.347085 -202.867419 
L 1312.536986 -186.706757 
L 1214.726886 -219.611931 
L 1116.916787 -254.643802 
z
" style="stroke: #dfaeb2; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#pcd56c48886)">
     <use xlink:href="#m72770aeeb3" x="0" y="360" style="fill: #dfaeb2; fill-opacity: 0.2; stroke: #dfaeb2; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_19">
    <defs>
     <path id="me8c526fca8" d="M 1116.916787 -245.351038 
L 1116.916787 -232.031615 
L 1214.726886 -183.85843 
L 1312.536986 -172.704527 
L 1410.347085 -180.596571 
L 1410.347085 -215.138398 
L 1410.347085 -215.138398 
L 1312.536986 -188.820913 
L 1214.726886 -212.778721 
L 1116.916787 -245.351038 
z
" style="stroke: #af6c91; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#pcd56c48886)">
     <use xlink:href="#me8c526fca8" x="0" y="360" style="fill: #af6c91; fill-opacity: 0.2; stroke: #af6c91; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="PolyCollection_20">
    <defs>
     <path id="m8b1fdcf5ff" d="M 1116.916787 -250.646055 
L 1116.916787 -237.984469 
L 1214.726886 -203.985304 
L 1312.536986 -173.243217 
L 1410.347085 -182.635635 
L 1410.347085 -217.319036 
L 1410.347085 -217.319036 
L 1312.536986 -189.264051 
L 1214.726886 -233.913639 
L 1116.916787 -250.646055 
z
" style="stroke: #2d1e3e; stroke-opacity: 0.2"/>
    </defs>
    <g clip-path="url(#pcd56c48886)">
     <use xlink:href="#m8b1fdcf5ff" x="0" y="360" style="fill: #2d1e3e; fill-opacity: 0.2; stroke: #2d1e3e; stroke-opacity: 0.2"/>
    </g>
   </g>
   <g id="matplotlib.axis_7">
    <g id="xtick_13">
     <g id="line2d_54">
      <g>
       <use xlink:href="#m2a2fc8578d" x="1116.916787" y="318.04" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_27">
      <!-- Blue (5) -->
      <g transform="translate(1097.180068 332.638438)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-42"/>
       <use xlink:href="#DejaVuSans-6c" x="68.603516"/>
       <use xlink:href="#DejaVuSans-75" x="96.386719"/>
       <use xlink:href="#DejaVuSans-65" x="159.765625"/>
       <use xlink:href="#DejaVuSans-20" x="221.289062"/>
       <use xlink:href="#DejaVuSans-28" x="253.076172"/>
       <use xlink:href="#DejaVuSans-35" x="292.089844"/>
       <use xlink:href="#DejaVuSans-29" x="355.712891"/>
      </g>
     </g>
    </g>
    <g id="xtick_14">
     <g id="line2d_55">
      <g>
       <use xlink:href="#m2a2fc8578d" x="1214.726886" y="318.04" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_28">
      <!-- Green (6) -->
      <g transform="translate(1190.915949 332.638438)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-47"/>
       <use xlink:href="#DejaVuSans-72" x="77.490234"/>
       <use xlink:href="#DejaVuSans-65" x="116.353516"/>
       <use xlink:href="#DejaVuSans-65" x="177.876953"/>
       <use xlink:href="#DejaVuSans-6e" x="239.400391"/>
       <use xlink:href="#DejaVuSans-20" x="302.779297"/>
       <use xlink:href="#DejaVuSans-28" x="334.566406"/>
       <use xlink:href="#DejaVuSans-36" x="373.580078"/>
       <use xlink:href="#DejaVuSans-29" x="437.203125"/>
      </g>
     </g>
    </g>
    <g id="xtick_15">
     <g id="line2d_56">
      <g>
       <use xlink:href="#m2a2fc8578d" x="1312.536986" y="318.04" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_29">
      <!-- Red (16) -->
      <g transform="translate(1291.183861 332.638438)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-52"/>
       <use xlink:href="#DejaVuSans-65" x="64.982422"/>
       <use xlink:href="#DejaVuSans-64" x="126.505859"/>
       <use xlink:href="#DejaVuSans-20" x="189.982422"/>
       <use xlink:href="#DejaVuSans-28" x="221.769531"/>
       <use xlink:href="#DejaVuSans-31" x="260.783203"/>
       <use xlink:href="#DejaVuSans-36" x="324.40625"/>
       <use xlink:href="#DejaVuSans-29" x="388.029297"/>
      </g>
     </g>
    </g>
    <g id="xtick_16">
     <g id="line2d_57">
      <g>
       <use xlink:href="#m2a2fc8578d" x="1410.347085" y="318.04" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
     <g id="text_30">
      <!-- Yellow (17) -->
      <g transform="translate(1383.099429 332.638438)scale(0.1 -0.1)">
       <use xlink:href="#DejaVuSans-59"/>
       <use xlink:href="#DejaVuSans-65" x="47.833984"/>
       <use xlink:href="#DejaVuSans-6c" x="109.357422"/>
       <use xlink:href="#DejaVuSans-6c" x="137.140625"/>
       <use xlink:href="#DejaVuSans-6f" x="164.923828"/>
       <use xlink:href="#DejaVuSans-77" x="226.105469"/>
       <use xlink:href="#DejaVuSans-20" x="307.892578"/>
       <use xlink:href="#DejaVuSans-28" x="339.679688"/>
       <use xlink:href="#DejaVuSans-31" x="378.693359"/>
       <use xlink:href="#DejaVuSans-37" x="442.316406"/>
       <use xlink:href="#DejaVuSans-29" x="505.939453"/>
      </g>
     </g>
    </g>
    <g id="text_31">
     <!-- state -->
     <g transform="translate(1250.966311 346.316563)scale(0.1 -0.1)">
      <use xlink:href="#DejaVuSans-73"/>
      <use xlink:href="#DejaVuSans-74" x="52.099609"/>
      <use xlink:href="#DejaVuSans-61" x="91.308594"/>
      <use xlink:href="#DejaVuSans-74" x="152.587891"/>
      <use xlink:href="#DejaVuSans-65" x="191.796875"/>
     </g>
    </g>
   </g>
   <g id="matplotlib.axis_8">
    <g id="ytick_22">
     <g id="line2d_58">
      <g>
       <use xlink:href="#m4d544a2910" x="1102.245272" y="302.037134" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
    </g>
    <g id="ytick_23">
     <g id="line2d_59">
      <g>
       <use xlink:href="#m4d544a2910" x="1102.245272" y="257.97984" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
    </g>
    <g id="ytick_24">
     <g id="line2d_60">
      <g>
       <use xlink:href="#m4d544a2910" x="1102.245272" y="213.922546" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
    </g>
    <g id="ytick_25">
     <g id="line2d_61">
      <g>
       <use xlink:href="#m4d544a2910" x="1102.245272" y="169.865251" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
    </g>
    <g id="ytick_26">
     <g id="line2d_62">
      <g>
       <use xlink:href="#m4d544a2910" x="1102.245272" y="125.807957" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
    </g>
    <g id="ytick_27">
     <g id="line2d_63">
      <g>
       <use xlink:href="#m4d544a2910" x="1102.245272" y="81.750663" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
    </g>
    <g id="ytick_28">
     <g id="line2d_64">
      <g>
       <use xlink:href="#m4d544a2910" x="1102.245272" y="37.693369" style="stroke: #000000; stroke-width: 0.8"/>
      </g>
     </g>
    </g>
   </g>
   <g id="line2d_65">
    <path d="M 1116.916787 112.1808 
L 1214.726886 181.452754 
L 1312.536986 185.293876 
L 1410.347085 160.643957 
" clip-path="url(#pcd56c48886)" style="fill: none; stroke: #edd1cb; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="line2d_66">
    <path d="M 1116.916787 124.047164 
L 1214.726886 142.397432 
L 1312.536986 172.456857 
L 1410.347085 174.760506 
" clip-path="url(#pcd56c48886)" style="fill: none; stroke: #e9c6c2; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="line2d_67">
    <path d="M 1116.916787 112.428302 
L 1214.726886 153.700345 
L 1312.536986 181.061107 
L 1410.347085 177.414711 
" clip-path="url(#pcd56c48886)" style="fill: none; stroke: #dfaeb2; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="line2d_68">
    <path d="M 1116.916787 121.409944 
L 1214.726886 162.604413 
L 1312.536986 178.886924 
L 1410.347085 162.14387 
" clip-path="url(#pcd56c48886)" style="fill: none; stroke: #af6c91; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="line2d_69">
    <path d="M 1116.916787 115.868543 
L 1214.726886 141.398532 
L 1312.536986 178.920506 
L 1410.347085 159.55397 
" clip-path="url(#pcd56c48886)" style="fill: none; stroke: #2d1e3e; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="patch_12">
    <path d="M 1102.245272 318.04 
L 1102.245272 24.72 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="patch_13">
    <path d="M 1102.245272 318.04 
L 1425.0186 318.04 
" style="fill: none; stroke: #000000; stroke-width: 0.8; stroke-linejoin: miter; stroke-linecap: square"/>
   </g>
   <g id="text_32">
    <!-- sigma_base = 40.0 -->
    <g transform="translate(1215.60928 18.72)scale(0.1 -0.1)">
     <use xlink:href="#DejaVuSans-73"/>
     <use xlink:href="#DejaVuSans-69" x="52.099609"/>
     <use xlink:href="#DejaVuSans-67" x="79.882812"/>
     <use xlink:href="#DejaVuSans-6d" x="143.359375"/>
     <use xlink:href="#DejaVuSans-61" x="240.771484"/>
     <use xlink:href="#DejaVuSans-5f" x="302.050781"/>
     <use xlink:href="#DejaVuSans-62" x="352.050781"/>
     <use xlink:href="#DejaVuSans-61" x="415.527344"/>
     <use xlink:href="#DejaVuSans-73" x="476.806641"/>
     <use xlink:href="#DejaVuSans-65" x="528.90625"/>
     <use xlink:href="#DejaVuSans-20" x="590.429688"/>
     <use xlink:href="#DejaVuSans-3d" x="622.216797"/>
     <use xlink:href="#DejaVuSans-20" x="706.005859"/>
     <use xlink:href="#DejaVuSans-34" x="737.792969"/>
     <use xlink:href="#DejaVuSans-30" x="801.416016"/>
     <use xlink:href="#DejaVuSans-2e" x="865.039062"/>
     <use xlink:href="#DejaVuSans-30" x="896.826172"/>
    </g>
   </g>
  </g>
  <g id="text_33">
   <!-- DRA -->
   <g transform="translate(736.416563 0.118125)scale(0.12 -0.12)">
    <defs>
     <path id="DejaVuSans-44" d="M 1259 4147 
L 1259 519 
L 2022 519 
Q 2988 519 3436 956 
Q 3884 1394 3884 2338 
Q 3884 3275 3436 3711 
Q 2988 4147 2022 4147 
L 1259 4147 
z
M 628 4666 
L 1925 4666 
Q 3281 4666 3915 4102 
Q 4550 3538 4550 2338 
Q 4550 1131 3912 565 
Q 3275 0 1925 0 
L 628 0 
L 628 4666 
z
" transform="scale(0.015625)"/>
     <path id="DejaVuSans-41" d="M 2188 4044 
L 1331 1722 
L 3047 1722 
L 2188 4044 
z
M 1831 4666 
L 2547 4666 
L 4325 0 
L 3669 0 
L 3244 1197 
L 1141 1197 
L 716 0 
L 50 0 
L 1831 4666 
z
" transform="scale(0.015625)"/>
    </defs>
    <use xlink:href="#DejaVuSans-44"/>
    <use xlink:href="#DejaVuSans-52" x="77.001953"/>
    <use xlink:href="#DejaVuSans-41" x="142.484375"/>
   </g>
  </g>
  <g id="legend_1">
   <g id="text_34">
    <!-- lmda -->
    <g transform="translate(1451.509531 146.064063)scale(0.1 -0.1)">
     <use xlink:href="#DejaVuSans-6c"/>
     <use xlink:href="#DejaVuSans-6d" x="27.783203"/>
     <use xlink:href="#DejaVuSans-64" x="125.195312"/>
     <use xlink:href="#DejaVuSans-61" x="188.671875"/>
    </g>
   </g>
   <g id="line2d_70">
    <path d="M 1438.874375 157.242188 
L 1448.874375 157.242188 
L 1458.874375 157.242188 
" style="fill: none; stroke: #edd1cb; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="text_35">
    <!-- 0.5 -->
    <g transform="translate(1466.874375 160.742188)scale(0.1 -0.1)">
     <use xlink:href="#DejaVuSans-30"/>
     <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
     <use xlink:href="#DejaVuSans-35" x="95.410156"/>
    </g>
   </g>
   <g id="line2d_71">
    <path d="M 1438.874375 171.920313 
L 1448.874375 171.920313 
L 1458.874375 171.920313 
" style="fill: none; stroke: #e9c6c2; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="text_36">
    <!-- 1.0 -->
    <g transform="translate(1466.874375 175.420313)scale(0.1 -0.1)">
     <use xlink:href="#DejaVuSans-31"/>
     <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
     <use xlink:href="#DejaVuSans-30" x="95.410156"/>
    </g>
   </g>
   <g id="line2d_72">
    <path d="M 1438.874375 186.598438 
L 1448.874375 186.598438 
L 1458.874375 186.598438 
" style="fill: none; stroke: #dfaeb2; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="text_37">
    <!-- 2.0 -->
    <g transform="translate(1466.874375 190.098438)scale(0.1 -0.1)">
     <use xlink:href="#DejaVuSans-32"/>
     <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
     <use xlink:href="#DejaVuSans-30" x="95.410156"/>
    </g>
   </g>
   <g id="line2d_73">
    <path d="M 1438.874375 201.276563 
L 1448.874375 201.276563 
L 1458.874375 201.276563 
" style="fill: none; stroke: #af6c91; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="text_38">
    <!-- 5.0 -->
    <g transform="translate(1466.874375 204.776563)scale(0.1 -0.1)">
     <use xlink:href="#DejaVuSans-35"/>
     <use xlink:href="#DejaVuSans-2e" x="63.623047"/>
     <use xlink:href="#DejaVuSans-30" x="95.410156"/>
    </g>
   </g>
   <g id="line2d_74">
    <path d="M 1438.874375 215.954688 
L 1448.874375 215.954688 
L 1458.874375 215.954688 
" style="fill: none; stroke: #2d1e3e; stroke-width: 1.5; stroke-linecap: square"/>
   </g>
   <g id="text_39">
    <!-- 10.0 -->
    <g transform="translate(1466.874375 219.454688)scale(0.1 -0.1)">
     <use xlink:href="#DejaVuSans-31"/>
     <use xlink:href="#DejaVuSans-30" x="63.623047"/>
     <use xlink:href="#DejaVuSans-2e" x="127.246094"/>
     <use xlink:href="#DejaVuSans-30" x="159.033203"/>
    </g>
   </g>
  </g>
 </g>
 <defs>
  <clipPath id="p62ac668042">
   <rect x="49.646695" y="24.72" width="322.773328" height="293.32"/>
  </clipPath>
  <clipPath id="p650cd95429">
   <rect x="400.512887" y="24.72" width="322.773328" height="293.32"/>
  </clipPath>
  <clipPath id="pcb11c65ebd">
   <rect x="751.37908" y="24.72" width="322.773328" height="293.32"/>
  </clipPath>
  <clipPath id="pcd56c48886">
   <rect x="1102.245272" y="24.72" width="322.773328" height="293.32"/>
  </clipPath>
 </defs>
</svg>

