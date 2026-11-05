# Inequality Persistence Simulator: User Guide

**The analytical framework of the simulator, its controls and figures, and twelve controlled experiments on the persistence of income shares.**

The guide has three parts. Sections 1 to 3 set out the research question, the framework and the simulated economy. Sections 4 and 5 document the interface. Sections 6 to 8 report controlled experiments, relate them to empirical work on income shares and propose exercises. Every number and screenshot comes from the simulator at its default seed (20251008), so each result can be reproduced exactly.

The simulator runs in any recent browser: open [`index.html`](../index.html) from this repository, or use the [online version](https://abdelhamidelhamrani777.github.io/Inequality-Persistence-Simulator/). Technical details on the implementation are collected in the [technical reference](TECHNICAL.md).

## Contents

1. [Research question](#1-research-question)
2. [Analytical framework](#2-analytical-framework)
3. [The simulated economy](#3-the-simulated-economy)
4. [Interface and controls](#4-interface-and-controls)
5. [Reading the figures](#5-reading-the-figures)
6. [Controlled experiments](#6-controlled-experiments)
7. [From the simulator to income-share data](#7-from-the-simulator-to-income-share-data)
8. [Exercises](#8-exercises)
9. [Frequently asked questions](#9-frequently-asked-questions)
10. [Notation](#10-notation)
11. [Limits of the model](#11-limits-of-the-model)
12. [References](#12-references)
13. [Credits](#13-credits)

---

## 1. Research question

Long-run studies of inequality in rich countries document diverging movements of income shares: the bottom groups have lost ground, the shares of the middle classes have declined and the share of the top percentile has risen since 1980 (Piketty and Saez, 2003; Piketty, Saez and Zucman, 2018; Alvaredo et al., 2018). These movements suggest that income shares at different points of the distribution follow different data-generating processes. Diverging trends alone do not establish it, since shares that sum to one must move in opposite directions. The question is whether the *stochastic* part of each share, what remains once its long-run path is removed, also differs across the distribution.

The feature of that stochastic part studied here is **persistence**: the extent to which a deviation of a share from its long-run path in one period is carried into the next. Persistence describes how a change in the distribution of income is propagated over time.

- A **flat** persistence profile means that low and high income shares have the same inertia.
- An **inverted-U** profile, for instance 0.5 at the bottom, 0.8 in the middle and 0.4 at the top, means that deviations persist in the middle and fade at the extremes. When inequality changes, the year-to-year adjustment then falls on the extremes, while the middle carries deviations forward for several years.
- A **U-shaped** profile implies the opposite: adjustments occur more often in the middle than in the lower and upper shares.

The working hypothesis is that persistence depends on the **composition of income by source**. Wages dominate the middle of the distribution, social insurance is a major source at the bottom and business income dominates the very top (Gomez, 2025). Labor income is set by contracts, wage schedules and institutions that change slowly. Capital income follows profits, asset prices and the timing of realizations, and transfers respond to eligibility rules and to the business cycle. If each source has its own dynamics, each income share inherits a different process. The statistical core of the argument is that a sum of autoregressive components with different coefficients does not have the persistence of any single component (Granger and Morris, 1976).

The simulator isolates this mechanism. It builds an economy in which composition is the only source of heterogeneity across individuals, and measures the persistence of each income share, the Lorenz curve and the Gini coefficient. It results from a research effort carried out within master's theses at the Faculty of Governance, Economics and Social Sciences (FGSES) of Mohammed VI Polytechnic University (UM6P), under the supervision of Professor Ramses Abul Naga.

## 2. Analytical framework

### 2.1 Income shares, the Lorenz curve and the Gini coefficient

With $`N = 5`$ individuals, the income share of individual $`i`$ in period $`t`$ is

```math
S_{i,t} = \frac{y_{i,t}}{\sum_{j=1}^{N} y_{j,t}} .
```

Ranking individuals by income in period $`t`$, the Lorenz curve is the set of points $`(k/N,\, L_t(k/N))`$, where $`L_t(k/N)`$ is the sum of the $`k`$ smallest shares. The Gini coefficient is twice the area between the diagonal and the Lorenz curve,

```math
G_t = 1 - \frac{1}{N}\sum_{k=1}^{N}\left[L_t\!\left(\tfrac{k}{N}\right) + L_t\!\left(\tfrac{k-1}{N}\right)\right],
```

and its maximum with five individuals is $`1 - 1/N = 0.8`$. Because each ordinate of the Lorenz curve is a sum of shares, its deviations are sums of the deviations of the shares below it, and their persistence is a variance-weighted combination of the persistence of those shares (section 2.7).

### 2.2 Persistence of an income source

Each source of income follows an autoregressive process of order one, augmented with a constant and a linear trend:

```math
x_t = \beta + \beta_{trend}\, t + \phi\, x_{t-1} + e_t, \qquad e_t \sim N(0, \sigma^2).
```

The coefficient $`\phi`$ is the fraction of this period's deviation that is still present next period. The process is stationary around its deterministic path when $`|\phi| < 1`$; without trend it then settles at the long-run level $`\beta/(1-\phi)`$, and with a trend on a line whose slope is $`\beta_{trend}/(1-\phi)`$. At $`\phi = 1`$ (a unit root) shocks never fade, and for $`\phi < 0`$ the process overshoots, a high period being followed by a low one. The half-life of a deviation, $`\ln 0.5/\ln\phi`$ for $`0 < \phi < 1`$, translates $`\phi`$ into periods:

| $`\phi`$ | 0.50 | 0.70 | 0.80 | 0.90 | 0.95 | 0.99 |
| --- | --- | --- | --- | --- | --- | --- |
| Half-life (periods) | 1.0 | 1.9 | 3.1 | 6.6 | 13.5 | 69.0 |

### 2.3 Persistence of an income share

The persistence of a share is measured by the first-order autocorrelation of its **deviation from the deterministic path**, that is, from the share the model would produce with every shock set to zero (the *no-shock path*). For each simulated sample,

```math
\hat\rho_i(1) = \frac{\sum_{t=1}^{T} \left(d_{i,t}-\bar d_i\right)\left(d_{i,t-1}-\bar d_i\right)}{\sum_{t=0}^{T} \left(d_{i,t}-\bar d_i\right)^2}, \qquad d_{i,t} = S_{i,t} - S^{0}_{i,t},
```

where $`S^{0}_{i,t}`$ is the no-shock share. Figure 1 reports the mean of $`\hat\rho_i(1)`$ over the simulated samples and its 5th and 95th percentiles. Removing the deterministic component prevents a trend, or the slow adjustment from the starting allocation to the long-run levels, from being counted as persistence, in the same way that unmodelled trends and breaks bias the persistence of observed series upward (Perron, 1989; Lamoureux and Lastrapes, 1990).

### 2.4 The composition mechanism

A first-order linearization of each share around the long-run levels $`m = \beta/(1-\phi)`$ gives the persistence of a share as a variance-weighted average of $`\phi_L`$ and $`\phi_K`$:

```math
\rho_i(1) \approx \frac{\sum_{j} c_{ij}^{2}\left(\phi_{L}\,\gamma_{L,j} + \phi_{K}\,\gamma_{K,j}\right)}{\sum_{j} c_{ij}^{2}\left(\gamma_{L,j} + \gamma_{K,j}\right)}, \qquad c_{ij} = \frac{\mathbf{1}[i=j] - s_i}{M},
```

where $`\gamma = \sigma^2/(1-\phi^2)`$ is the stationary variance of each source, $`s_i`$ the long-run share of individual $`i`$ and $`M`$ total long-run income. Three implications follow.

1. **Every share lies between $`\phi_K`$ and $`\phi_L`$.** An individual whose fluctuations come mainly from labor income inherits a persistence close to $`\phi_L`$; one whose fluctuations come mainly from capital income inherits a persistence close to $`\phi_K`$.
2. **The weights are contributions to fluctuations, not to the level of income.** A source that is small in level but volatile can dominate the movements of a share.
3. **A share depends on everyone's income.** The terms $`j \neq i`$ enter because a share also moves when the total moves, so no share reaches $`\phi_L`$ exactly, and a change in one individual's process affects every share (experiments 6.9 and 6.12).

At the default settings ($`\phi_L = 0.90`$, $`\phi_K = 0.50`$, shocks proportional to holdings), the ranking of persistence is exactly the ranking of the share of each individual's income variance that comes from labor:

| Individual | Capital share of income, period 0 | Labor's share of income variance | Simulated $`\hat\rho(1)`$ of the share |
| --- | --- | --- | --- |
| Bjørn | 0.19 | 98.5% | 0.831 |
| Alfred | 0.40 | 89.9% | 0.787 |
| Meryem | 0.41 | 89.2% | 0.773 |
| George | 0.69 | 43.6% | 0.688 |
| Violette | 0.83 | 13.6% | 0.588 |

The linearization is defined for fixed individuals when every $`|\phi|`$ is below 1, every trend is zero and total long-run income is positive. The simulator draws it as the *theory line*.

### 2.5 Persistence, volatility and mobility

Persistence is distinct from volatility (Engle, 2001). The former describes how long a deviation lasts; the latter measures its size. Both are distinct from mobility, the frequency with which individuals change rank.

| Dimension | Question | Where it appears |
| --- | --- | --- |
| Persistence | How long does a deviation last? | Figure 1 |
| Volatility | How large are the deviations? | Figures 3 and 4; the Gini path of Figure 2 |
| Mobility | How often do individuals change rank? | In figures strip; crossings in Figure 4 |

Persistence is invariant to a common rescaling of the shocks, whereas volatility scales with them and mobility depends on the size of shocks relative to the gaps between incomes. Experiment 6.5 separates the three.

### 2.6 Individuals and rank positions

Inequality statistics such as top income shares follow rank positions, not the same people over time. The persistence measured on a quantile group is the persistence of a group's claim on national income, which combines the dynamics of its members' incomes with entries into and exits from the group. Work on individual earnings dynamics (Gottschalk and Moffitt, 2009; Kopczuk, Saez and Song, 2010) studies individuals followed over time and therefore addresses a different object. The **Rank slots** mode of the simulator re-sorts individuals by income every period and measures the persistence of positions (experiment 6.6).

### 2.7 What static measures do not reveal

A Lorenz curve and a Gini coefficient summarise how income is divided at a point in time. They are silent on how long a change in that division will last and on where in the distribution it will last. Two economies with the same Lorenz curve can differ in whether a shock to the middle fades within a period or persists for several. The Lorenz curve itself can change shape at different speeds along its length: with an inverted-U profile, its lower part inherits the fast-fading deviations of the bottom shares, while the ordinates around the median carry the persistent deviations of the middle. Experiment 6.4 constructs five such economies. Persistence, alongside the level of inequality and the volatility of the shares, is therefore a separate dimension of a distributional assessment.

## 3. The simulated economy

### 3.1 Model

Five individuals, Violette, Meryem, Bjørn, Alfred and George, earn labor income $`w`$ and capital income $`f`$. Each source follows its own autoregressive process, and total income $`y`$ is their sum:

```math
\begin{aligned}
w_{i,t} &= \beta_{W,i} + \beta_{W,trend,i}\, t + \phi_L\, w_{i,t-1} + u_{i,t}, & u_{i,t} &\sim N\!\left(0,\sigma_{L,i}^2\right) &\quad (1)\\
f_{i,t} &= \beta_{F,i} + \beta_{F,trend,i}\, t + \phi_K\, f_{i,t-1} + v_{i,t}, & v_{i,t} &\sim N\!\left(0,\sigma_{K,i}^2\right) &\quad (2)\\
y_{i,t} &= w_{i,t} + f_{i,t}, & i &= 1,\dots,5, \quad t = 1,\dots,T &\quad (3)
\end{aligned}
```

Recursive substitution back to period 0 gives the closed form

```math
y_{i,t} = \phi_L^{t}\, w_{i,0} + \sum_{s=1}^{t} \phi_L^{t-s}\left(\beta_{W,i} + \beta_{W,trend,i}\, s + u_{i,s}\right) + \phi_K^{t}\, f_{i,0} + \sum_{s=1}^{t} \phi_K^{t-s}\left(\beta_{F,i} + \beta_{F,trend,i}\, s + v_{i,s}\right). \qquad (4)
```

Income in period $`t`$ is the starting value discounted by $`\phi^t`$ plus every past shock discounted by $`\phi^{t-s}`$, separately for each source. Shocks are independent across individuals, sources and periods. The persistence parameters can be common to all individuals or set by individual.

### 3.2 Calibration

| Element | Default |
| --- | --- |
| Persistence | $`\phi_L = 0.90`$, $`\phi_K = 0.50`$: labor more persistent than capital |
| Period-0 allocation | 200 units of labor and 200 units of capital (table below) |
| Constants | $`\beta = (1-\phi)\times`$ start, so that each income fluctuates around its own starting value |
| Trends | 0 |
| Shocks | $`\sigma = `$ start / 10, so shocks are proportional to holdings |
| Sample | $`T = 110`$ periods, the length of a century of annual data; 200 simulated samples |
| Random numbers | Seed 20251008; a separate stream for each individual, source and sample |

| Individual | Labor | Capital | Total | Capital share |
| --- | --- | --- | --- | --- |
| Violette | 4 | 20 | 24 | 0.83 |
| Meryem | 26 | 18 | 44 | 0.41 |
| Bjørn | 58 | 14 | 72 | 0.19 |
| Alfred | 66 | 44 | 110 | 0.40 |
| George | 46 | 104 | 150 | 0.69 |

The starting economy is labor-intensive in the middle and capital-intensive at both ends. Its Lorenz curve gives the poorest 40% 17.0% of income, and its Gini coefficient is 0.318. The five profile presets (section 4.4) keep these period-0 incomes and change only the split between labor and capital.

### 3.3 Working rules

Four properties of the implementation govern how experiments should be set up.

1. **Common random numbers.** Every change of parameters reuses the same shocks, so differences between two settings come from the parameters alone. **New draw** changes the seed; **Reset** restores every setting, including the seed. Every experiment below starts from Reset.
2. **Changing $`\phi`$ does not change $`\beta`$.** The long-run level $`\beta/(1-\phi)`$ therefore moves, and incomes drift towards it. To keep an income around its start, set $`\beta = (1-\phi)\times`$ start.
3. **The allocation sets period 0 only.** Editing the allocation changes starting values, not long-run levels; incomes return to their previous levels at the rate $`\phi`$ unless $`\beta`$ is changed as well (experiment 6.11).
4. **Presets re-anchor every parameter.** A profile preset sets the allocation and resets $`\beta = (1-\phi)\times`$ start, trend 0 and $`\sigma = `$ start / 10 for every income.

## 4. Interface and controls

![The interface, annotated](images/interface-annotated.png)

| # | Area | Content |
| --- | --- | --- |
| 1 | Run settings | Number of periods, units of analysis (individuals or rank slots), seed with **New draw**, **Reset**, and the **EN / FR** language switch |
| 2 | Persistence | $`\phi_L`$ and $`\phi_K`$, common to all or set by individual, with a stationarity tag for each source |
| 3 | Tabs | **Parameters** or **Allocation** in the lower left panel |
| 4 | Parameters by individual | Start, $`\phi`$, $`\beta`$, trend, $`\sigma`$ and long-run level of each income |
| 5 | Period bar | Selects the period $`t`$ used by the In figures strip and Figures 2 and 5 |
| 6 | In figures | Key statistics at period $`t`$ |
| 7 | The model | Equations (1) to (4) with the current values, and one card per individual |

Every change recomputes all figures immediately.

### 4.1 Run settings

- **Periods**: 110, 500 or 2000. Figure 1 averages over 200, 60 or 20 simulated samples respectively.
- **Units**: *Individuals* follows the same five people; *Rank slots* re-sorts them by total income every period and follows positions (section 2.6).
- **Seed** and **New draw**: the seed fixes the shocks; New draw draws a new seed.
- **Reset**: restores the default settings.
- **Language**: **EN** or **FR** switches every label, title and reading between English and French without changing settings or results. The choice is remembered, and the address `…/Inequality-Persistence-Simulator/?lang=fr` opens the French version. In French, numbers use a decimal comma.

### 4.2 Persistence

<img src="images/guide/persistence-card.png" alt="The Persistence card" width="410">

- **Shared**: one $`\phi_L`$ and one $`\phi_K`$ for all individuals, set with the sliders or typed in the fields.
- **By person**: individual values of $`\phi_L`$ and $`\phi_K`$, entered in the Parameters tab; the sliders are then inactive. Switching to By person copies the shared values to every individual.
- The tag under each slider reads **Stationary** when $`|\phi| < 1`$ and **Unit root** when $`|\phi| = 1`$.

### 4.3 Parameters by individual

<img src="images/guide/parameters-card.png" alt="The Parameters tab" width="410">

Each individual has a **w** row (labor) and an **f** row (capital).

| Column | Content |
| --- | --- |
| Start | Period-0 value, from the allocation (read only) |
| $`\phi`$ | Persistence; editable when persistence is set By person |
| $`\beta`$ | Constant |
| Trend | Coefficient on $`t`$; the income drifts by trend / $`(1-\phi)`$ per period |
| $`\sigma`$ | Standard deviation of the shock; flagged when larger than the long-run level |
| Long run | $`\beta/(1-\phi)`$, the level reached without shocks (at $`t = 0`$ when there is a trend) |

Values are typed and confirmed with Enter, or changed with the mouse wheel once a field is selected (Shift for steps ten times larger). Negative values are allowed. Selecting a name opens that individual's series in Figure 3.

### 4.4 Allocation and presets

<img src="images/guide/allocation-increasing.png" alt="The Allocation tab after choosing the Increasing preset" width="410">

- **Labor cap** and **Capital cap** set the total units of each source (200 by default); changing a cap rescales the allocation.
- **Units** or **Percent**: holdings are entered in units or as a percentage of each cap. In Percent mode, the Total % column sets an individual's share of all income and rescales the others.
- Each source is capped: an entry above the remaining units is clipped. To move units between individuals, lower the donor first.
- Rows re-sort by total income when the table is left or Enter is pressed, so that the table always runs from the poorest to the richest at period 0.
- **Profile presets** reproduce five economies in which every individual keeps the same period-0 income (24, 44, 72, 110 and 150 units with equal caps of 200), and therefore the same Lorenz curve and Gini coefficient, 0.318. Only the split between labor and capital differs. The more persistent source receives the pattern the shape requires, so the presets work whichever source is more persistent. The Inverted U preset is more polarized than the starting allocation.

### 4.5 Time

The period bar selects the period $`t`$ shown in the In figures strip and in Figures 2 and 5, and marks it on Figures 3 and 4. On the figure board, the left and right arrow keys step one period (Shift for ten), Home and End jump to the first and last periods, and Space plays or pauses.

## 5. Reading the figures

Each figure has a title that states the result at the current settings, a subtitle that gives the unit, a source line, the chart with its key values, and a **Reading** line that states the result in words. Hovering over a chart shows exact values.

### 5.1 In figures

![The In figures strip](images/guide/in-figures.png)

At the selected period: the Gini coefficient and its change since period 0, the labor share of total income, total income, the shape of the persistence profile, and the number of periods in which at least one individual changed rank, with the individual who changed most often.

### 5.2 The model panel

![The model panel](images/guide/model.png)

Equations (1) to (4) in four blocks (labor income, capital income, total income, closed form), with the current values of the persistence parameters. Below them, one card per individual gives the same equations with that individual's parameters and starting values; selecting a card opens the individual's series in Figure 3.

### 5.3 Figure 1: persistence profile

<img src="images/guide/fig1.png" alt="Figure 1 at the default settings" width="543">

For each individual, ordered from the poorest to the richest at period 0:

- **Red line and dots**: mean of $`\hat\rho(1)`$ over the simulated samples, with its value.
- **Grey whiskers**: 5th to 95th percentiles across samples, a measure of the sampling dispersion of $`\hat\rho(1)`$ with $`T`$ periods.
- **Grey dashed line with hollow diamonds**: the linearized theory of section 2.4.
- **Blue and ochre dashed lines**: $`\phi_L`$ and $`\phi_K`$, when persistence is shared.

The title classifies the profile as increasing, decreasing, flat (range below 0.03), U-shaped, inverted U or mixed. **Values by person** gives each individual's share and capital ratio at period 0, the simulated mean, the percentile band and the theoretical value.

### 5.4 Figure 2: Lorenz curve and Gini coefficient

<img src="images/guide/fig2.png" alt="Figure 2 at the default settings" width="543">

The red curve is the Lorenz curve at period $`t`$, with individuals re-ranked in that period and cumulative shares in percent at each point. The grey dashed curve is the period-0 curve, and the grey band the 5th to 95th percentile of each ordinate over the path. $`G`$ is the Gini coefficient at $`t`$; the lower chart traces it over all periods, with a dashed line at its period-0 value. At the defaults, $`G`$ is 0.318 at period 0 and 0.335 at period 110 and ranges from 0.276 to 0.380 along the path; the poorest 40% hold 17.0% of income at period 0 and 18.1% at period 110.

### 5.5 Figure 3: individual series

**One person** gives the selected individual's equations in four blocks, with the long-run level and the half-life of a shock for each source, the rank and capital share at period 0, and the closed form with the individual's own values.

![Figure 3, equations for George](images/guide/fig3-equations.png)

The chart below draws total income (red), labor income (blue), capital income (ochre) and the no-shock path of total income (grey, dashed).

![Figure 3, George's incomes](images/guide/fig3-chart.png)

George's labor income moves in long swings around its level and his capital income fluctuates rapidly around 104: the two persistence parameters are visible in the series themselves.

**Compare** draws one series (total, labor, capital or share) for several individuals, with their no-shock paths if selected. The table reports each individual's $`\phi_L`$ and $`\phi_K`$, value at $`t`$, mean, standard deviation, range, rank, number of rank changes and $`\hat\rho(1)`$ of the share.

![Figure 3, Compare view: capital income of Violette and George](images/guide/fig3-compare.png)

### 5.6 Figure 4: income shares over time

![Figure 4 at the default settings](images/guide/fig4.png)

The share of each individual along the displayed sample path. Crossings are rank changes; at the defaults, Meryem and Bjørn cross five times. In rank-slot mode each line is a position. The name chips are shared with the Compare view of Figure 3.

### 5.7 Figure 5: income by individual

<img src="images/guide/fig5.png" alt="Figure 5 at period 110" width="543">

Labor (blue) and capital (ochre) income at period $`t`$, ordered by total income, with a black tick at each individual's period-0 total.

---

## 6. Controlled experiments

Each experiment starts from **Reset** and changes one element of the calibration. Results are those of the default seed; another seed moves the numbers slightly without changing the conclusions.

| # | Experiment | Result |
| --- | --- | --- |
| 6.1 | Starting economy | Persistence follows the composition of income |
| 6.2 | Reversed source persistence | The profile reverses |
| 6.3 | Equal source persistence | Composition becomes irrelevant |
| 6.4 | Five economies with identical incomes | Composition generates every shape; the Lorenz curve and the Gini coefficient do not register it |
| 6.5 | Larger shocks | Volatility and mobility rise; persistence does not |
| 6.6 | Rank positions | High mobility compresses the persistence of positions |
| 6.7 | Deterministic trends | Trends move the level of inequality, not its persistence |
| 6.8 | Unit root | Deviations become permanent |
| 6.9 | Negative persistence | Overshooting in one source lowers every share's persistence |
| 6.10 | Sample length | Short samples bias persistence downward but preserve the ordering |
| 6.11 | One-off and permanent redistribution | A transfer lasts as long as the income it targets |
| 6.12 | A volatile top | Volatile, fast-reverting top income lowers top persistence and spills over |

### 6.1 Starting economy

**Design.** Default settings: $`\phi_L = 0.90`$, $`\phi_K = 0.50`$, the allocation of section 3.2.

<img src="images/guide/fig1.png" alt="Persistence peaks in the middle" width="543">

**Result.** The profile is an inverted U.

| | Violette | Meryem | Bjørn | Alfred | George |
| --- | --- | --- | --- | --- | --- |
| Simulated $`\hat\rho(1)`$ | 0.588 | 0.773 | 0.831 | 0.787 | 0.688 |
| Linearized theory | 0.635 | 0.844 | 0.878 | 0.840 | 0.753 |

The composition behind it is visible in Figure 5 at period 0: Bjørn, in the middle, holds 58 units of labor and 14 of capital; George, at the top, 104 units of capital; Violette, at the bottom, 20 units of capital and 4 of labor.

<img src="images/guide/fig5-t0.png" alt="Figure 5 at period 0" width="543">

**Interpretation.** The ranking of persistence is the ranking of labor's contribution to each individual's fluctuations (section 2.4). The middle, where labor income dominates, carries deviations forward; the bottom and the top, exposed to fast-reverting income, adjust quickly. A top share that moves rapidly is therefore not evidence of random top incomes; it is consistent with top incomes being made of fast-reverting sources.

### 6.2 Reversed source persistence

**Design.** Set $`\phi_L = 0.50`$ and $`\phi_K = 0.90`$.

<img src="images/guide/swap.png" alt="The profile reverses" width="543">

**Result.** The profile becomes U-shaped, from 0.540 for Bjørn to 0.821 for Violette, with George at 0.761. Because the constants were not changed (rule 2), the long-run levels move: labor incomes settle at a fifth of their start ($`0.1/(1-0.5) = 0.2`$) and capital incomes at five times their start ($`0.5/(1-0.9) = 5`$). Total income rises from 400 to 1,070, the labor share falls from 50% to 3%, and negative components appear in 22 of 111 periods.

To keep each income around its start, set $`\beta = (1-\phi)\times`$ start in every row:

| $`\beta`$ | Violette | Meryem | Bjørn | Alfred | George |
| --- | --- | --- | --- | --- | --- |
| w row (0.5 × start) | 2.00 | 13.00 | 29.00 | 33.00 | 23.00 |
| f row (0.1 × start) | 2.00 | 1.80 | 1.40 | 4.40 | 10.40 |

<img src="images/guide/swap-anchored.png" alt="The reversal with re-anchored constants" width="543">

The profile remains U-shaped, from 0.635 for Bjørn to 0.844 for Violette, the mirror image of the default; no income turns negative and ranks change in 6 periods.

**Interpretation.** Composition determines the profile only in interaction with the ordering of source persistence. The same allocation produces opposite profiles under opposite orderings.

### 6.3 Equal source persistence

**Design.** Set $`\phi_L = \phi_K = 0.70`$.

<img src="images/guide/equal-persistence.png" alt="The profile is flat" width="543">

**Result.** The profile is flat: every share lies between 0.649 and 0.671, and the theory line equals 0.700 for every individual. Re-anchoring $`\beta`$ at 0.3 × start gives 0.650 to 0.672.

**Interpretation.** This is the control case. With $`\phi_L = \phi_K = \phi`$, formula (2.4) reduces to $`\phi`$ for every individual, whatever the weights. Differences in persistence along the distribution are therefore informative about differences in persistence across sources.

### 6.4 Five economies with identical incomes

**Design.** Holding $`\phi_L = 0.90`$ and $`\phi_K = 0.50`$, open the **Allocation** tab and select each preset in turn. The five economies give every individual the same period-0 income (24, 44, 72, 110 and 150 units), hence the same Lorenz curve and the same Gini coefficient, 0.318, and differ only in the split between labor and capital.

| Economy | Labor units, individuals 1 to 5 | Capital units, individuals 1 to 5 |
| --- | --- | --- |
| Decreasing | 21, 39, 43, 53, 44 | 3, 5, 29, 57, 106 |
| Increasing | 3, 10, 24, 50, 113 | 21, 34, 48, 60, 37 |
| Flat | 12, 22, 36, 55, 75 | 12, 22, 36, 55, 75 |
| U-shaped | 21, 13, 8, 28, 130 | 3, 31, 64, 82, 20 |
| Inverted U | 3, 39, 64, 79, 15 | 21, 5, 8, 31, 135 |

<table>
<tr>
<td><img src="images/guide/preset-decreasing.png" alt="Decreasing economy"></td>
<td><img src="images/guide/preset-increasing.png" alt="Increasing economy"></td>
</tr>
<tr>
<td><img src="images/guide/preset-flat.png" alt="Flat economy"></td>
<td><img src="images/guide/preset-u-shaped.png" alt="U-shaped economy"></td>
</tr>
<tr>
<td><img src="images/guide/preset-inverted-u.png" alt="Inverted-U economy"></td>
<td></td>
</tr>
</table>

**Result.** Each economy produces the profile it is named after.

| Economy | Simulated $`\hat\rho(1)`$, individuals 1 to 5 | Theory, range | Gini, period 0 | Gini, mean over the path | Gini, 5th–95th percentile | Gini, period 110 | Periods with a rank change |
| --- | --- | --- | --- | --- | --- | --- | --- |
| Decreasing | 0.834, 0.824, 0.784, 0.740, 0.664 | 0.73–0.89 | 0.318 | 0.320 | 0.289–0.356 | 0.308 | 4 |
| Increasing | 0.607, 0.641, 0.704, 0.766, 0.817 | 0.65–0.87 | 0.318 | 0.317 | 0.276–0.352 | 0.317 | 20 |
| Flat | 0.761, 0.745, 0.762, 0.766, 0.758 | 0.82 | 0.318 | 0.319 | 0.289–0.348 | 0.314 | 6 |
| U-shaped | 0.839, 0.677, 0.626, 0.693, 0.819 | 0.68–0.89 | 0.318 | 0.317 | 0.275–0.357 | 0.290 | 12 |
| Inverted U | 0.575, 0.819, 0.831, 0.799, 0.631 | 0.62–0.88 | 0.318 | 0.322 | 0.281–0.364 | 0.343 | 12 |

Each shape follows the labor share across individuals. In the inverted-U economy, the poorest and the richest hold mostly capital (12.5% and 10% labor), while the three individuals in the middle hold 72% to 89% labor. When every individual holds labor and capital in equal proportions, the profile is flat at the theoretical value of 0.82. Simulated means lie about 0.05 to 0.07 below the theory line, the downward small-sample bias of the autocorrelation (experiment 6.10). Persistence is measured for fixed individuals; the difference with rank positions is small here, since ranks change in only 4 to 20 of the 110 periods.

**Interpretation.** The five economies share the same Lorenz curve at period 0, their mean Gini coefficients over 110 periods lie between 0.317 and 0.322, and their 5th–95th percentile ranges overlap almost entirely. At period 110 the Gini coefficient differs across economies, from 0.290 to 0.343, but this spread is smaller than the range each economy covers over time. Yet the linearized persistence of individual shares ranges from 0.62 to 0.88 in the inverted-U economy and is uniform at 0.82 in the flat one. The Lorenz curve and the Gini coefficient do not register this heterogeneity (section 2.7).

### 6.5 Larger shocks: volatility, persistence and mobility

**Design.** Multiply every $`\sigma`$ by 3 in the Parameters tab.

| $`\sigma`$ | Violette | Meryem | Bjørn | Alfred | George |
| --- | --- | --- | --- | --- | --- |
| w row | 1.20 | 7.80 | 17.40 | 19.80 | 13.80 |
| f row | 6.00 | 5.40 | 4.20 | 13.20 | 31.20 |

<table>
<tr>
<td><img src="images/guide/fig4.png" alt="Shares over time, default shocks"></td>
</tr>
<tr>
<td><img src="images/guide/shocks-series.png" alt="Shares over time, shocks multiplied by 3"></td>
</tr>
</table>

**Result.** The shares swing three times wider and cross repeatedly: ranks change in 64 periods instead of 8, Bjørn's alone 47 times, and the Gini coefficient ranges from 0.237 to 0.662 instead of 0.276 to 0.380. The persistence profile is essentially unchanged, an inverted U from 0.581 to 0.820 against 0.588 to 0.831, and the theory line is identical. Incomes turn negative in 27 periods.

<img src="images/guide/shocks-profile.png" alt="Figure 1 with shocks multiplied by 3" width="543">

**Interpretation.** A common rescaling of the shocks rescales every deviation but leaves its rate of decay unchanged; mobility, which depends on shocks relative to income gaps, rises sharply. A distribution can be highly volatile and mobile while each deviation lasts exactly as long as before, so the three dimensions must be measured separately.

### 6.6 Rank positions

**Design.** Keep the shocks of 6.5 and set **Units** to **Rank slots**.

<img src="images/guide/rank-profile.png" alt="Figure 1 in rank slots, with large shocks" width="543">

**Result.** Figure 1 now plots rank positions 1 to 5. The profile becomes *mixed*, with every position between 0.560 and 0.638, flatter and lower than the 0.581 to 0.820 of the same individuals. The theory line is not shown, since it describes fixed individuals. At the default shocks (Reset, then Rank slots), the position profile is an inverted U from 0.587 to 0.821, close to the individual profile, because ranks change in only 8 periods.

![Figure 4 in rank slots](images/guide/rank-series.png)

**Interpretation.** A rank position holds whoever occupies it. When mobility is low, a position is effectively an individual and its persistence reveals that individual's composition. When mobility is high, a position inherits a blend of everyone's sources and differences across positions are compressed. The persistence of quantile-group shares is a property of positions, and its interpretation requires information on entries into and exits from each group.

### 6.7 Deterministic trends

**Design A.** In George's **f** row, set the trend to **0.300**: his capital income grows by $`0.3/(1-0.5) = 0.6`$ units per period.

<img src="images/guide/trend-top-lorenz.png" alt="Figure 2 with a capital trend at the top" width="543">

**Result A.** The Gini coefficient rises from 0.318 to 0.400 at period 110, George's share from 37.5% to 47.2%, and total income from 400 to 468.5, while the labor share falls from 50.0% to 42.0%.

![In figures with a capital trend at the top](images/guide/trend-top-figures.png)

![Figure 4 with a capital trend at the top](images/guide/trend-top-series.png)

**Design B.** Reset; in Violette's and Meryem's **w** rows, set the trend to **0.030** (0.3 units per period).

<img src="images/guide/trend-bottom-lorenz.png" alt="Figure 2 with wage growth at the bottom" width="543">

**Result B.** The Gini coefficient falls from 0.318 to 0.242 at period 110; Violette's share rises from 6.0% to 11.6%, Meryem's from 11.0% to 17.6%, and the labor share from 50.0% to 55.5%. Ranks change in 23 periods as both catch up with Bjørn.

In both cases the profile remains an inverted U (0.580 to 0.833 in design A), and the theory line is not shown because it assumes constant long-run levels.

**Interpretation.** Trends and persistence act on different dimensions: a trend determines where the distribution is heading, persistence how deviations around that path behave. Capital income growing faster at the top raises the top share and the Gini coefficient and lowers the labor share, a stylized counterpart of the recent rise of top capital incomes documented by Gomez (2025). The removal of the deterministic component (section 2.3) is what keeps such trends out of the persistence measure.

### 6.8 Unit root

**Design.** Set $`\phi_K = 1.00`$, then set $`\beta = 0`$ in every **f** row. With $`\phi = 1`$ the constant becomes a drift added every period: left unchanged, it makes total income grow from 400 to 11,532 in 110 periods. With $`\beta = 0`$ capital income is a pure random walk.

![Figure 4 with a unit root in capital](images/guide/unit-root-series.png)

**Result.** Shares wander without reverting. Bjørn's share falls from 18.0% to 6.8% and Violette's rises from 6.0% to 10.6%; ranks change in 33 periods; the Gini coefficient moves between 0.262 and 0.422 without fluctuating around a level.

<img src="images/guide/unit-root-lorenz.png" alt="Figure 2 with a unit root in capital" width="543">

<img src="images/guide/unit-root-profile.png" alt="Figure 1 with a unit root in capital" width="543">

Measured persistence is high for everyone (0.887 to 0.932), with the capital-intensive bottom and top now the most persistent. The theory line is undefined, and incomes turn negative in 51 of 111 periods.

**Interpretation.** With a unit root a deviation is permanent and the distribution has no level to return to. Whether a share is stationary around a deterministic path, possibly with structural breaks, or contains a unit root is therefore a prior question to any measurement of persistence (section 7).

### 6.9 Negative persistence

**Design.** Choose **By person** in the Persistence card. In George's **f** row, set $`\phi = -0.50`$ and $`\beta = 156.00`$ ($`1.5\times 104`$, which keeps his capital income around 104). Select George's name to open his series.

![George's incomes with capital persistence −0.5](images/guide/negative-george.png)

**Result.** George's capital income alternates between high and low periods. The persistence of his share falls from 0.688 to 0.305, and that of every other share falls as well (Violette from 0.588 to 0.517, Bjørn from 0.831 to 0.786). George changes rank 12 times instead of 4.

<img src="images/guide/negative-profile.png" alt="Figure 1 with George's capital persistence at −0.5" width="543">

**Interpretation.** Negative persistence represents overshooting, as when income realized in one year depletes the next. Because shares are relative, one individual's dynamics enter every share through the total (section 2.4, implication 3).

### 6.10 Sample length

**Design.** Set **Periods** to **2000**, then to **500**.

<img src="images/guide/long-sample.png" alt="Figure 1 with 2000 periods" width="543">

**Result.**

| | Violette | Meryem | Bjørn | Alfred | George |
| --- | --- | --- | --- | --- | --- |
| Theory | 0.635 | 0.844 | 0.878 | 0.840 | 0.753 |
| Simulated, 2000 periods | 0.636 | 0.839 | 0.877 | 0.842 | 0.749 |
| Simulated, 500 periods | 0.620 | 0.829 | 0.869 | 0.828 | 0.733 |
| Simulated, 110 periods | 0.588 | 0.773 | 0.831 | 0.787 | 0.688 |

At 110 periods every estimate lies 0.05 to 0.07 below its theoretical value and the 5th–95th percentile band is 0.18 to 0.27 wide; at 2000 periods every estimate is within 0.006 of the theory.

**Interpretation.** The sample autocorrelation is biased toward zero in short samples, more so when persistence is high. With about a century of annual data, persistence estimates are both biased and imprecise, but the shape of the profile is the same at every sample length. Comparisons of persistence across the distribution are more robust than the level of any single estimate.

### 6.11 One-off and permanent redistribution

**Design A.** Open the **Allocation** tab, in Units, and type **40** in every Capital cell, starting with George (104), then Alfred (44), Bjørn, Meryem and Violette, so that no entry is clipped. On Enter, the rows re-sort, and George, now at 86 units, ranks third behind Bjørn (98) and Alfred (106).

<table>
<tr>
<td><img src="images/guide/redistribution-t0.png" alt="Figure 2 at period 0 after equalizing capital"></td>
<td><img src="images/guide/redistribution-one-off.png" alt="Figure 2 at period 110 after equalizing capital"></td>
</tr>
</table>

**Result A.** The Gini coefficient at period 0 falls from 0.318 to 0.156, and the poorest 40% hold 27.5% of income instead of 17.0%. The Gini path then returns quickly: 0.224 after one period, 0.254 after two, 0.295 after five, and 0.339 at period 10, back on the default path. The allocation sets period 0 only (rule 3), and with $`\phi_K = 0.5`$ half of the remaining gap closes every period.

**Design B.** Keep the allocation and set $`\beta = 20.00`$ in every **f** row ($`(1-0.5)\times 40`$), so that 40 becomes everyone's long-run capital income.

<img src="images/guide/redistribution-permanent.png" alt="Figure 2 after a permanent equalization of capital" width="543">

**Result B.** The Gini coefficient remains low: 0.168 at period 110, between 0.117 and 0.239 along the path. With incomes closer together, ranks change in 50 periods.

**Design C.** Reset and equalize **labor** income instead: type 40 in every Labor cell, lowering Alfred (66), Bjørn (58) and George (46) first.

<img src="images/guide/redistribution-labor.png" alt="Figure 2 after a one-off equalization of labor" width="543">

**Result C.** The Gini coefficient falls from 0.318 to 0.206 at period 0 and recovers far more slowly: 0.230 after five periods, 0.297 after ten, 0.322 after twenty. The half-life of a labor deviation is 6.6 periods, against 1 period for capital.

**Interpretation.** The effect of a one-off redistribution decays at the persistence of the income it targets. A transfer of fast-reverting income fades within a few periods, a change to persistent income lasts for years, and only a change in the underlying process (here $`\beta`$; in an economy, earning capacity, ownership or institutions) is permanent. In designs A and B, Figure 1 reads *no simple pattern*: each individual's persistence is unchanged, but George now ranks in the middle, and the profile is plotted in ranking order. The shape of a profile depends on who stands where.

### 6.12 A volatile top

**Design.** Suppose top capital income is more volatile and less persistent than other capital income, as when it is driven by asset prices.

- A: in George's **f** row, set $`\sigma = 20.80`$ (twice its default).
- B: in addition, choose **By person** and set George's capital $`\phi = 0.30`$ and $`\beta = 72.80`$ ($`0.7\times 104`$).

<img src="images/guide/volatile-top-params.png" alt="Parameters for a volatile top" width="410">

<img src="images/guide/volatile-top-profile.png" alt="Figure 1 with a volatile top" width="543">

**Result.**

| | Violette | Meryem | Bjørn | Alfred | George |
| --- | --- | --- | --- | --- | --- |
| Default | 0.588 | 0.773 | 0.831 | 0.787 | 0.688 |
| A: George's capital $`\sigma`$ doubled | 0.563 | 0.725 | 0.789 | 0.726 | 0.568 |
| B: and his capital $`\phi`$ at 0.30 | 0.529 | 0.701 | 0.772 | 0.693 | 0.452 |

George's persistence falls from 0.688 to 0.452, the largest change, and every other share's persistence falls as well.

**Interpretation.** Doubling a source's volatility raises its weight in the fluctuations of the share (section 2.4, implication 2), and lowering its persistence lowers the value it contributes. Volatile, fast-reverting income at the top therefore lowers the persistence of the top share and, through the total, that of every other share.

---

## 7. From the simulator to income-share data

Each experiment corresponds to a step in the empirical analysis of income-share dynamics, for instance on long-run series of pre-tax national income shares built from tax and national accounts data (Piketty and Saez, 2003; Piketty, Saez and Zucman, 2018).

| In the simulator | In income-share data |
| --- | --- |
| Five individuals | A partition of the population into income groups |
| Rank slots (6.6) | Quantile groups, whose members change over time |
| No-shock path (2.3) | A deterministic component: trends and structural breaks |
| Unit root (6.8) | Unit-root tests, with and without breaks |
| 110 periods (6.10) | About a century of annual observations |
| Composition at period 0 (6.1, 6.4) | Decomposition of each group's income by source |
| Shocks × 3 (6.5) | Volatility of each share, measured separately |

The simulator implies the following points of method.

- **Use non-overlapping groups.** Cumulative groups such as the Top 1%, which contains the Top 0.1%, mix the dynamics of their sub-groups. A partition of the distribution into non-overlapping bands assigns each part of the distribution to one series.
- **Remove the deterministic component before measuring persistence.** Shares bounded between zero and one can trend for decades, and institutions and historical shocks shift their level and slope at identifiable dates. Unmodelled trends and breaks bias persistence toward one and can masquerade as unit roots (Perron, 1989; Zivot and Andrews, 1992; Lamoureux and Lastrapes, 1990). The simulator's no-shock path is the exact counterpart of this step (6.7).
- **Test for unit roots on the stationarized series.** If a share contains a unit root, its autocorrelation does not measure persistence around a path (6.8).
- **Compare persistence across groups rather than in levels.** The level of persistence depends on the specification of the deterministic component and is biased downward in samples of about a century (6.10); the ordering across groups is more robust.
- **Consider higher-order dynamics.** With observed data, autoregressive models of order two or three, selected by an information criterion, can reveal overshooting where realized income in one year depletes the next (6.9).
- **Separate persistence from volatility.** The size of a share's fluctuations is not a reliable guide to how long they last (6.5, 6.12).
- **Relate persistence to composition.** Decompositions of each group's income into labor, business, capital and transfer income (Gomez, 2025) identify which source drives a group's fluctuations; as formula (2.4) shows, what matters is the source of fluctuations, not only the source of the level.
- **Distinguish intrinsic from inherited persistence.** A share may appear persistent only because it tracks a persistent driver, such as policy or macroeconomic conditions. Adding such covariates to the autoregressive model, after the same removal of a deterministic component, tests whether the profile is intrinsic to the shares.
- **Account for mobility.** The persistence of a top share combines the persistence of top incomes with entries into and exits from the top (6.6).

## 8. Exercises

Each exercise starts from Reset. Predict the result before running it.

**1. The most persistent share.** At the defaults, whose share is the most persistent, and which statistic of section 2.4 explains it?

<details><summary>Answer</summary>

Bjørn's, at 0.831: 98.5% of his income variance comes from labor income, whose persistence is 0.90, the highest proportion of the five.
</details>

**2. An increasing profile from the allocation alone.** How must the allocation be changed for persistence to rise from bottom to top?

<details><summary>Answer</summary>

The bottom must hold more capital relative to labor and the top more labor relative to capital, each source still summing to 200 units. The Increasing preset does this while keeping every period-0 income unchanged: labor 3, 10, 24, 50, 113 and capital 21, 34, 48, 60, 37. Editing the table by hand leaves the parameters anchored to the old allocation (rule 3); select the preset, or re-anchor $`\beta`$ and $`\sigma`$, to obtain the clean result.
</details>

**3. Equal persistence at 0.90.** Set $`\phi_K = 0.90`$ with $`\phi_L = 0.90`$. Which shape do you expect, and where is the theory line?

<details><summary>Answer</summary>

Flat, with the theory line at 0.90 for every individual (6.3). The simulated values lie between 0.832 and 0.851, below 0.90 because of the small-sample bias (6.10).
</details>

**4. Duration of a transfer.** Before running 6.11, predict which fades faster, a one-off equalization of capital or of labor, and by how much.

<details><summary>Answer</summary>

Capital. With $`\phi_K = 0.5`$ half of the remaining gap closes every period (half-life 1 period), against a half-life of 6.6 periods with $`\phi_L = 0.9`$. The Gini coefficient is back on its default path after about 10 periods for capital, and still below it after 20 periods for labor.
</details>

**5. Doubling every shock.** Multiply every $`\sigma`$ by 2. What happens to Figure 1, to the number of rank changes and to the Gini path?

<details><summary>Answer</summary>

Figure 1 barely changes (0.586 to 0.827) and the theory line is identical, since persistence is invariant to a common rescaling of the shocks. Rank changes rise from 8 to 44 periods and the Gini path widens to 0.251–0.481 (6.5).
</details>

**6. Rank positions.** In 6.6, why is the position profile flatter when shocks are large?

<details><summary>Answer</summary>

With large shocks, individuals move between positions often, so each position's share is made of different individuals' incomes over time. Each position inherits a blend of everyone's income sources, and differences in composition across positions are averaged out.
</details>

**7. Wage growth in the middle.** Set a trend of 0.030 in Bjørn's w row. What happens to the Gini coefficient and to the labor share?

<details><summary>Answer</summary>

Bjørn's labor income rises by 0.03 / (1 − 0.9) = 0.3 units per period, so his no-shock income grows from 72 to about 102 by period 110. He remains third but closes part of the gap with Alfred (110) and George (150). The labor share rises from 50.0% to 52.4%, and the Gini coefficient at period 110 is 0.310 instead of 0.335: income growth in the middle, toward the top, narrows inequality. A trend large enough for Bjørn to overtake George would widen it again.
</details>

**8. Drift under a unit root.** Set $`\phi_K = 1`$ without changing $`\beta`$. Why does total income explode, and why does the theory line disappear?

<details><summary>Answer</summary>

With $`\phi = 1`$ the equation becomes $`f_t = \beta + f_{t-1} + v_t`$: $`\beta`$ is a drift added every period, so capital income grows without bound (George's by 52 units per period). The linearization requires finite long-run levels and variances, which a unit root rules out (6.8).
</details>

## 9. Frequently asked questions

**The numbers differ slightly from the guide.** Click **Reset** to restore the default seed, 20251008. After **New draw**, every number moves slightly while the shapes remain.

**The theory line is not shown.** It requires fixed individuals (Units set to Individuals), $`|\phi| < 1`$ for every income, zero trends and positive long-run total income. The note under Figure 1 states which condition fails.

**A note reports negative components.** Some income became negative in that number of periods. This occurs when shocks are large relative to income (6.5), with a unit root (6.8), or when a change of $`\phi`$ moves the long-run levels (6.2).

**Incomes drift after a change of $`\phi`$.** The constant did not change, so the long-run level $`\beta/(1-\phi)`$ did (rule 2). Set $`\beta = (1-\phi)\times`$ start or select a preset.

**An allocation entry was reduced.** Each source is capped; lower another entry first or raise the cap.

**The persistence fields of the Parameters tab are inactive.** They are active only when persistence is set **By person**.

**Figure 1 reads "no simple pattern".** The profile is plotted from the poorest to the richest at period 0. An allocation that places a capital-intensive individual in the middle (6.11) produces a profile that is neither monotone nor U-shaped; **Values by person** gives the numbers.

**The page is slow at 2000 periods.** Each change recomputes 20 samples of 2000 periods; use 110 periods while exploring.

## 10. Notation

| Symbol or term | Definition |
| --- | --- |
| $`w_{i,t}`$, $`f_{i,t}`$, $`y_{i,t}`$ | Labor, capital and total income of individual $`i`$ in period $`t`$ |
| $`S_{i,t}`$ | Income share, $`y_{i,t}/\sum_j y_{j,t}`$ |
| $`\phi_L`$, $`\phi_K`$ | Persistence (autoregressive coefficient) of labor and capital income |
| $`\beta_{W,i}`$, $`\beta_{F,i}`$ | Constants of labor and capital income |
| $`\beta_{W,trend,i}`$, $`\beta_{F,trend,i}`$ | Coefficients on the time trend |
| $`u_{i,t}`$, $`v_{i,t}`$ | Normal shocks to labor and capital income |
| $`\sigma_{L,i}`$, $`\sigma_{K,i}`$ | Standard deviations of the shocks |
| $`\gamma`$ | Stationary variance of a source, $`\sigma^2/(1-\phi^2)`$ |
| $`\hat\rho_i(1)`$ | First-order sample autocorrelation of the deviation of a share from its no-shock path |
| $`\rho_i(1)`$ | Its linearized theoretical value, formula (2.4) |
| $`L_t(p)`$, $`G_t`$ | Lorenz curve and Gini coefficient in period $`t`$ |
| Long-run level | $`\beta/(1-\phi)`$ |
| Half-life | $`\ln 0.5/\ln\phi`$, the number of periods for half of a deviation to fade |
| Unit root | $`|\phi| = 1`$: deviations never fade |
| No-shock path | The path of income or of a share with every shock set to zero |
| Rank slot | A position in the distribution, whoever occupies it |
| Labor share | Labor income divided by total income |

## 11. Limits of the model

- The economy has five individuals, so the Lorenz curve has five segments and the Gini coefficient cannot exceed 0.8.
- Shocks are independent across individuals and sources: there is no aggregate shock and no correlation between an individual's labor and capital shocks.
- Each source follows an AR(1) process; higher-order dynamics and structural breaks are not modelled.
- Shocks are normal, so incomes can become negative.
- There is no behavior: individuals do not save, invest or respond to prices or taxes, and composition is set exogenously.
- Two sources stand for several. Transfers at the bottom and corporate profits at the top are both represented by the less persistent source, and pass-through business income, which may behave more like labor income, has no separate process.

These restrictions are deliberate: the simulator isolates the link between income composition and the persistence of income shares under controlled conditions, and estimates nothing from data.

## 12. References

- Alvaredo, F., Chancel, L., Piketty, T., Saez, E., and Zucman, G. (2018). *World Inequality Report 2018*. Belknap Press of Harvard University Press.
- Engle, R. F. (2001). GARCH 101: The use of ARCH/GARCH models in applied econometrics. *Journal of Economic Perspectives*, 15(4), 157–168.
- Gomez, M. (2025). Macro perspectives on income inequality. *Journal of Economic Perspectives*, 39(2), 127–148.
- Gottschalk, P., and Moffitt, R. (2009). The rising instability of U.S. earnings. *Journal of Economic Perspectives*, 23(4), 3–24.
- Granger, C. W. J., and Morris, M. J. (1976). Time series modelling and interpretation. *Journal of the Royal Statistical Society, Series A*, 139(2), 246–257.
- Kopczuk, W., Saez, E., and Song, J. (2010). Earnings inequality and mobility in the United States: Evidence from social security data since 1937. *Quarterly Journal of Economics*, 125(1), 91–128.
- Lamoureux, C. G., and Lastrapes, W. D. (1990). Persistence in variance, structural change, and the GARCH model. *Journal of Business & Economic Statistics*, 8(2), 225–234.
- Perron, P. (1989). The great crash, the oil price shock, and the unit root hypothesis. *Econometrica*, 57(6), 1361–1401.
- Piketty, T., and Saez, E. (2003). Income inequality in the United States, 1913–1998. *Quarterly Journal of Economics*, 118(1), 1–39.
- Piketty, T., Saez, E., and Zucman, G. (2018). Distributional national accounts: Methods and estimates for the United States. *Quarterly Journal of Economics*, 133(2), 553–609.
- Zivot, E., and Andrews, D. W. K. (1992). Further evidence on the great crash, the oil-price shock, and the unit-root hypothesis. *Journal of Business & Economic Statistics*, 10(3), 251–270.

## 13. Credits

The simulator was developed by **Abdelhamid El Hamrani** and **Hajar Hazmiri**, under the supervision of **Prof. Ramses Abul Naga**, at the Faculty of Governance, Economics and Social Sciences (FGSES) of Mohammed VI Polytechnic University (UM6P). The [README](../README.md) gives the citation, and the [technical reference](TECHNICAL.md) the implementation details.
