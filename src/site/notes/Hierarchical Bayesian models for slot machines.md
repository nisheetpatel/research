---
{"dg-publish":true,"permalink":"/hierarchical-bayesian-models-for-slot-machines/","created":"","updated":""}
---


## Models

#### Model

$p\left(y_n|g\left(\alpha_0 + \sum_s\mathbb{I}_{n,SM=s}\left(\alpha_{n,s} + {X}_{n,s}^T {\beta}_{n,s}\right)\right),\theta\right)$

where:
- $n\in\{1,...,N\}$ indexes the participant
- $s\in\{1,2,3,4\}$ indexes the slot machine

- $p(y|g(\cdot),\theta): bernoulli$
- $g: logit^{-1}$

- ${X}_{n,s}\in\mathbb{R}$ is the expected reward for each of the 4 slot machines $s$
- $y_n\in\{0,1\}$ is the boolean response 

##### Model parameters

- $\theta=\{\mu_\beta, \Sigma_\beta, \mu_\alpha, \mu_{\beta_s}, \sigma_\alpha, \Sigma_{\beta_s}, \beta_{n,s}, \alpha_n\}$

Participants' parameters are drawn from the group-level priors:
- $\beta_{n,s} \sim \mathcal{N}\left(\mu_{\beta_s}, \Sigma_{\beta_s}\right)\in\mathbb{R}$ is the slope of the fit for participant $n$, slot machine $s$
- $\alpha_n \sim \mathcal{N}\left(\mu_\alpha, \sigma_\alpha\right)\in\mathbb{R}$ is the intercept of the fit for participant $n$

Group-level priors are themselves drawn from hyper-priors:




![[Pasted image 20230224140356.png\|Pasted image 20230224140356.png]]

[Source](https://www.youtube.com/watch?v=uSjsJg8fcwY):
- In case of sparse data, we should use non-centered parametrization

## Stan code for models

### Individual simple logistic

```
data {
	int<lower=1> N; // number of training samples
	int<lower=0> K; // number of predictors
	matrix[K, N] X; // matrix of predictors
	int<lower=0, upper=1> y[N]; // response
}

parameters {
	real alpha;
	vector beta[K];
}

model {
	beta ~ normal(0,10);
	alpha ~ gamma(0,10);

	y ~ bernoulli_logit(X' * beta + alpha);
}
```

### Hierarchical models

#### Hierarchical $\alpha$ and $\beta$

```
data {
	int<lower=1> N; // number of training samples
	int<lower=0> K; // number of predictors (4 SMs)
	int<lower=1> L; // number of subjects
	
	int<lower=1, upper=L> ll[N] // subject id {1,...,L}
	
	row_vector[K] X[N];         // predictors
	int<lower=0, upper=1> y[N]; // response
}

parameters {
	vector[K] beta[L];  // individual slope
	vector[K] mu_beta;  // Hierarchical mean for slope
	vector<lower=0>[K] sigma_beta; // h std for slope
	
	vector[K] alpha[L]; // individual intercept
	vector[K] mu_alpha;   // Hierarchical mean for intercept
	vector<lower=0>[K] sigma_alpha; // h std for intercept
}

model {
	mu_beta ~ normal(0.25,0.25);
	sigma_beta ~ cauchy(0,0.5);
		
	mu_alpha ~ normal(0, 0.25);
	sigma_alpha ~ cauchy(0, 0.5);
	
	for (l in 1:L)
		beta[l] ~ normal(mu_beta, sigma_beta)
		alpha[l] ~ normal(mu_alpha, sigma_alpha);
	
	for (n in 1:N)
		y[n] ~ bernoulli_logit(x[n] * beta[ll[n]] + alpha[ll[n]]);
}
```

#### Hierarchical $\alpha$ shared $\beta$

```
data {
	int<lower=1> N; // number of training samples
	int<lower=0> K; // number of predictors (4)
	int<lower=1> L; // number of levels/subjects
	matrix[K, N] X; // matrix of predictors
	int<lower=0, upper=1> y[N]; // response
}

parameters {
	vector[K] beta; // same beta for all participants
	vector[L] alpha; // only one intercept for each individual
	real mu_alpha; // Hierarchical mean
	real<lower=0> sigma_alpha; // Hierarchical deviation
}

model {
	beta ~ normal(0, 10);
	alpha ~ normal(mu_alpha, sigma_alpha);
	mu_alpha ~ normal(0, 10);
	sigma_alpha ~ cauchy(0, 10); // half-cauchy bec > 0

	y ~ bernoulli_logit(X' * beta + alpha);
}
```

#### Model with 1 predictor only no $\alpha$

The one implemented for the [[Slot machines pilot v3 meeting\|Slot machines pilot v3 meeting]].

```
data {
	int<lower=1> N; // number of training samples
	int<lower=0> K; // number of predictors
	int<lower=1> L; // number of levels/subjects
	
	array[N] int<lower=1, upper=L> ll; // subject id
	array[N] int<lower=0, upper=1> y;
	array[N] row_vector[K] x;
}

parameters {
	array[K] real mu;
	array[K] real<lower=0> sigma;
	array[L] vector[K] beta;
}

model {
	mu ~ normal(0, 10);
	sigma ~ normal(0, 10);
	
	for (l in 1:L) {
		beta[l] ~ normal(mu, sigma);
	}
	
	vector[N] x_beta_ll;
	
	for (n in 1:N) {
		x_beta_ll[n] = x[n] * beta[ll[n]];
	}
	y ~ bernoulli_logit(x_beta_ll);
}

```

Older model:

```
data {
	int<lower=1> N; // number of training samples
	int<lower=0> K; // number of predictors
	int<lower=1> L; // number of levels/subjects
	int<lower=1, upper=L> ll[N]; // subject id
	row_vector[K] x[N]; // matrix of predictors
	int<lower=0, upper=1> y[N]; // response
}

parameters {
	real mu[K];
	real<lower=0.01> sigma[K];
	vector[K] beta[L];
}

model {
	vector[N] x_beta_ll;
	
	for (n in 1:N) {
		x_beta_ll[n] = x[n] * beta[ll[n]];
	}
	
	mu ~ normal(1.5,5);
	sigma ~ gamma(2,5);
	
	for (l in 1:L)
		beta[l] ~ normal(mu, sigma);
	
	y ~ bernoulli_logit(x_beta_ll);
}

generated quantities {
	real mu_ca[K];
	real s_ca[K];
	for (k in 1:K) {
		mu_ca[k] = inv_logit(mu[k]);
		s_ca[k] = inv_logit(sigma[k]);
	}
}
```