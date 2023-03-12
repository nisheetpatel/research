---
{"dg-publish":true,"permalink":"/results-for-slot-machines-pilot-v3/","created":"","updated":""}
---


[[Slides outline for slot machines pilot v3\|Slides outline for slot machines pilot v3]]

## Results

### Key figure

![[Pasted image 20230223155211.png\|Pasted image 20230223155211.png]]

### Logistic regression

##### Plot without bias correction

![[Logit-fits.png\|480]]

##### Comparison with Pilot v2

Pilot v2 had very similar slopes for slot machines 1-3, all of which were much higher than slot machine 4.

![[Logit_fits.png\|360]]
##### Details of each fit

###### Slot Machine 1

```
                           Logit Regression Results                           
==============================================================================
Dep. Variable:               Response   No. Observations:                 2712
Model:                          Logit   Df Residuals:                     2710
Method:                           MLE   Df Model:                            1
Date:                Thu, 23 Feb 2023   Pseudo R-squ.:                 0.05123
Time:                        12:45:55   Log-Likelihood:                -1780.2
converged:                       True   LL-Null:                       -1876.3
Covariance Type:            nonrobust   LLR p-value:                 1.032e-43
==============================================================================
                 coef    std err          z      P>|z|      [0.025      0.975]
------------------------------------------------------------------------------
Intercept      0.1072      0.040      2.688      0.007       0.029       0.185
Difficulty     0.3021      0.022     13.573      0.000       0.258       0.346
==============================================================================
```

###### Slot Machine 2

```
                           Logit Regression Results                           
==============================================================================
Dep. Variable:               Response   No. Observations:                 2712
Model:                          Logit   Df Residuals:                     2710
Method:                           MLE   Df Model:                            1
Date:                Thu, 23 Feb 2023   Pseudo R-squ.:                 0.01689
Time:                        12:45:55   Log-Likelihood:                -1843.4
converged:                       True   LL-Null:                       -1875.1
Covariance Type:            nonrobust   LLR p-value:                 1.725e-15
==============================================================================
                 coef    std err          z      P>|z|      [0.025      0.975]
------------------------------------------------------------------------------
Intercept     -0.1223      0.039     -3.140      0.002      -0.199      -0.046
Difficulty     0.2336      0.030      7.887      0.000       0.176       0.292
==============================================================================
```

###### Slot Machine 3

```
                           Logit Regression Results                           
==============================================================================
Dep. Variable:               Response   No. Observations:                  903
Model:                          Logit   Df Residuals:                      901
Method:                           MLE   Df Model:                            1
Date:                Thu, 23 Feb 2023   Pseudo R-squ.:                 0.03256
Time:                        12:45:55   Log-Likelihood:                -599.61
converged:                       True   LL-Null:                       -619.79
Covariance Type:            nonrobust   LLR p-value:                 2.110e-10
==============================================================================
                 coef    std err          z      P>|z|      [0.025      0.975]
------------------------------------------------------------------------------
Intercept      0.2451      0.069      3.571      0.000       0.111       0.380
Difficulty     0.2395      0.038      6.266      0.000       0.165       0.314
==============================================================================
```

###### Slot Machine 4

```
                           Logit Regression Results                           
==============================================================================
Dep. Variable:               Response   No. Observations:                  898
Model:                          Logit   Df Residuals:                      896
Method:                           MLE   Df Model:                            1
Date:                Thu, 23 Feb 2023   Pseudo R-squ.:                0.007146
Time:                        12:45:55   Log-Likelihood:                -615.59
converged:                       True   LL-Null:                       -620.02
Covariance Type:            nonrobust   LLR p-value:                  0.002913
==============================================================================
                 coef    std err          z      P>|z|      [0.025      0.975]
------------------------------------------------------------------------------
Intercept     -0.1481      0.067     -2.201      0.028      -0.280      -0.016
Difficulty     0.1510      0.051      2.965      0.003       0.051       0.251
==============================================================================
```

### Raw data psychometric curve

#### Pooled

![Resp-vs-Diff-by-SM_pooled.png|680](/img/user/images/Resp-vs-Diff-by-SM_pooled.png)

#### Individual

![Resp-vs-Diff-by-SM_individual.png](/img/user/images/Resp-vs-Diff-by-SM_individual.png)

### Choice accuracy

#### CA vs Difficulty (pooled)

![CA-vs-Diff-by-Block_pooled.png](/img/user/images/CA-vs-Diff-by-Block_pooled.png)

#### CA vs Slot Machine (pooled)

![CA-vs-SM-by-Block_pooled.png](/img/user/images/CA-vs-SM-by-Block_pooled.png)

#### CA vs Difficulty (individual)

![CA-vs-Diff-by-Block_individual.png](/img/user/images/CA-vs-Diff-by-Block_individual.png)

#### CA vs Slot Machine (individual)

![CA-vs-SM-by-Block_individual.png](/img/user/images/CA-vs-SM-by-Block_individual.png)

### Reaction times

#### RT vs Difficulty (pooled)

![RT-vs-Diff-by-Block_pooled.png](/img/user/images/RT-vs-Diff-by-Block_pooled.png)

#### RT vs Difficulty (individual)

![RT-vs-Diff-by-Block_individual 1.png](/img/user/images/RT-vs-Diff-by-Block_individual%201.png)

