---
layout: default
title:  "Snowball Pricing under Bachelier Model"
parent: Blog
nav_order: 2
---

## Bachelier Model

Underlying asset price follows the stochastic differential equation below:

$$
dS = bdt + \sigma dW
$$

where $b$, $\sigma$ are constants and $dW$ is a Wiener process.


## Snowball with IR 

Interest rates are often modeled using Bachelier model. The **Optimus** library supports pricing snowball under Bachelier model by the Monte Carlo algorithm and the numerical algorithm.

## The Standard Snowball 

|snowball contrat| ||
|--|--|--|
| start date |2022-08-03|not included|
| end date| 2023-08-03| included|
|initial price| 2.8| unit: 1%|
|ki observation| daily| every trading day|
|ki level| 2.5
|ki payoff| -$\text{Max}(S_0 - S_T, 0)$|sell atm put|
|ko observation| monthly| the dates are listed below|
|ko level| 2.8|
|coupon rate| 13%| coupon = days * 1/365 * 13% * $S_0$|
|type| up out down in||


---
The ko dates are :

2022-09-05, 2022-10-10, 2022-11-03, 2022-12-05, 2023-01-04, 2023-02-03

2023-03-03, 2023-04-06, 2023-05-04, 2023-06-05, 2023-07-03, 2023-08-03

---

### Pricing test

**Test case A**

Pricing parameters
|||
|-|-|
|valuation date| 2022-08-03|
|risk free rate| 3.5%|
|b|0|
|$\sigma$|37%|

Pricing results

|spot|pv (numerical)| pv (Monte Carlo 10M)| error |
|-|-|-|-|
| 2.60|-0.14076|-0.140702 $\pm$ 0.000228114|0.00004|
|2.65|-0.0953018|-0.0953423 $\pm$ 0.000280013|0.0000405|
|2.70|-0.0560917|-0.0560293 $\pm$ 0.00018445|0.0000624|
|2.75|-0.0244132|-0.0244985 $\pm$ 0.00019487|0.0000853|
|2.80|-0.000862508|-0.000894907 $\pm$ 0.000241678|0.0000324|
|2.85|0.0149573|0.0150063 $\pm$ 0.000161044|0.0000490|


**Test case B**

Pricing parameters
|||
|-|-|
|valuation date| 2023-01-05|
|risk free rate| 3.5%|
|b|0|
|$\sigma$|37%|

Pricing results B1: not knocked in yet

|spot|pv (numerical)| pv (Monte Carlo 10M)| error |
|-|-|-|-|
| 2.60|-0.0888529604| -0.088794379 $\pm$ 0.000216304|0.0000585814|
|2.65|-0.0185016902|-0.018424296 $\pm$ 0.0000867508 |0.0000774|
|2.70|0.0434230288|0.043322532 $\pm$ 0.000176352 |0.000100496|
|2.75|0.0944763677| 0.094465385 $\pm$ 0.000259307 |0.0000109827|
|2.80|0.1329056418|0.132901795 $\pm$ 0.000214749 |0.0000038468|
|2.85|0.1584253908| 0.158442759 $\pm$ 0.000136431 |0.0000173682|

Pricing results B2: already knocked in

|spot|pv (numerical)| pv (Monte Carlo 10M)| error |
|-|-|-|-|
| 2.60|-0.1218493442| -0.121865607 $\pm$ 1.01E-4 |1.62E-5|
|2.65|-0.0581569799| -0.05819372 $\pm$ 1.35E-4  |3.67E-5|
|2.70|0.0038919514| 0.003968294 $\pm$ 2.42E-4  |7.63E-5|
|2.75|0.0611796482| 0.061216594 $\pm$ 2.62E-4 |3.69E-5|
|2.80|0.1093742564| 0.109442123 $\pm$ 1.54E-4 |6.79E-5|
|2.85|0.1446896284| 0.144722012 $\pm$ 1.69E-4 |3.24E-5|

## The Reverse Snowball

|snowball contrat| ||
|--|--|--|
| start date |2022-08-03|not included|
| end date| 2023-08-03| included|
|initial price| 2.8| unit: 1%|
|ki observation| daily| every trading day|
|ki level| 3.1
|ki payoff| -$\text{Max}(S_T - S_0, 0)$| sell atm call|
|ko observation| monthly| the dates are listed below|
|ko level| 2.8|
|coupon rate| 13%| coupon = days * 1/365 * 13% * $S_0$|
|type| down out up in||


---
The ko dates are :

2022-09-05, 2022-10-10, 2022-11-03, 2022-12-05, 2023-01-04, 2023-02-03

2023-03-03, 2023-04-06, 2023-05-04, 2023-06-05, 2023-07-03, 2023-08-03

---

### Pricing test

**Test case C**

Pricing parameters
|||
|-|-|
|valuation date| 2022-08-03|
|risk free rate| 3.5%|
|b|0|
|$\sigma$|37%|

Pricing results

|spot|pv (numerical)| pv (Monte Carlo 10M)| error |
|-|-|-|-|
| 2.60|0.0315553859| 0.03156290135 $\pm$ 4.49E-5|7.52E-6|
|2.65|0.0293281096|0.02933058550 $\pm$ 7.26E-5 |2.48E-6|
|2.70|0.0243972439|0.02436691371 $\pm$ 1.35E-4 |3.03E-5|
|2.75|0.0149572978| 0.01491755096 $\pm$ 2.0E-4 |3.97E-5|
|2.80|-0.0008625083| -0.00086654658 $\pm$ 2.1E-4 |4.04E-6|
|2.85|-0.0244131811| -0.0244118966 $\pm$ 1.62E-4 |1.28E-6|


**Test case D**

Pricing parameters
|||
|-|-|
|valuation date| 2023-01-05|
|risk free rate| 3.5%|
|b|0|
|$\sigma$|37%|

Pricing results D1: not knocked in yet

|spot|pv (numerical)| pv (Monte Carlo 10M)| error |
|-|-|-|-|
|2.60|0.1821098177 | 0.18211293732 $\pm$ 2.66E-5 | 3.12E-6|
|2.65|0.1795803920 | 0.17957582348 $\pm$ 6.88E-5  |4.57E-6|
|2.70|0.1728512258 | 0.17282269534 $\pm$ 1.1E-4  |2.85E-5|
|2.75|0.1584253908 | 0.15831875173 $\pm$ 1.48E-4  |1.07E-4|
|2.80|0.1329056418 | 0.13288333807 $\pm$ 2.68E-4  |2.23E-5|
|2.85|0.0944763677 | 0.09461358916 $\pm$ 2.32E-4  |1.37E-4|

Pricing results D2: already knocked in

|spot|pv (numerical)| pv (Monte Carlo 10M)| error |
|-|-|-|-|
| 2.60|0.1813703426| 0.1813699445 $\pm$ 3.04E-5  |3.98E-7|
|2.65|0.1771119785| 0.1771097215 $\pm$ 4.22E-5  |2.26E-6|
|2.70|0.1663405214|  0.1663955068 $\pm$ 8.56E-5  | 5.50E-5|
|2.75|0.1446896284| 0.1446675810 $\pm$ 1.09E-4  |2.20E-5|
|2.80|0.1093742564|  0.1093563185 $\pm$ 1.51E-4 |1.79E-5|
|2.85|0.0611796482|  0.0612094090 $\pm$ 1.21E-4 |2.97E-5|

**_NOTE_**

    1, the  error  = |pv by numerical - pv by monte carlo| ;
    2, according to the central limit theorem, pv's calculated by monte carlo are normally distributed. The pv's are calculated by 10 times of 1M monte carlo,  take the average of the 10 times calculation as the final pv (equivalent to 10M Monte Carlo), and the standard deviation as the error term;
    3, the average time used for each calculation : 15 ms for numerical method, 250 ms for 1M Monte Carlo. 

