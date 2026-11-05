<a id="english"></a>

# Inequality Persistence Simulator

**English** · [Français](#francais)

**A free, interactive tool that shows why some people's share of income changes slowly while others' jumps up and down, and what this means for inequality.**

![The Inequality Persistence Simulator at its starting settings](docs/images/simulator.png)

**Open it:** use the [online version](https://abdelhamidelhamrani777.github.io/Inequality-Persistence-Simulator/), with nothing to install, or download [`index.html`](index.html) and open it in your browser. The **EN / FR** switch in the top bar changes the language.

**Learn with it:** the [user guide](docs/GUIDE.md) explains every button and chart, then walks through twelve experiments step by step.

## What is this?

Inequality is usually described with a snapshot: how income is shared out in a given year. This simulator looks at how that picture moves over time. When a group's share of income goes up, does it stay up, or does it soon fall back?

The simulator follows five imaginary people, Violette, Meryem, Bjørn, Alfred and George, over many periods (think of them as years). Each person has two kinds of income:

- **labor income**: wages and salaries, the income from work;
- **capital income**: the income from what they own, such as rents, dividends and interest.

Every year, both incomes are hit by random ups and downs. You decide how long those ups and downs last for each kind of income, how much each person starts with and how large the shocks are. The simulator then shows how each person's share of total income moves, and how inequality as a whole evolves.

The project grew out of research carried out within master's theses at the Faculty of Governance, Economics and Social Sciences (FGSES) of Mohammed VI Polytechnic University (UM6P), under the supervision of Professor Ramses Abul Naga.

## Three words to know

- **Income share**: a person's income divided by the income of everyone. If George earns 150 and the five people earn 400 together, his share is 37.5%.
- **Persistence**: how long a change lasts, measured by a number up to 1. Near 1, a change lasts a long time: a share that jumps up this year is still high next year. Near 0, a change disappears almost at once. At 0.9, half of a change is still there after about seven years; at 0.5, half of it is gone after one year.
- **Gini coefficient**: the usual summary measure of inequality. It is 0 when everyone has the same income and rises as income becomes more concentrated.

## What the simulator shows

Each result below comes from an example you can rerun in the simulator. The number in brackets points to the experiment in the [user guide](docs/GUIDE.md) that gives the steps.

1. **Where your income comes from decides how fast your share moves.** In the starting example, a change in wages lasts a long time (persistence 0.9) and a change in capital income fades fast (0.5). The people in the middle live mostly on wages, so their share moves slowly. The poorest and the richest rely more on capital income, so their shares jump and fall back quickly. Persistence is therefore highest in the middle (0.83) and lowest at the two ends (0.59 at the bottom, 0.69 at the top): a hill shape. *(6.1)*
2. **This only happens because the two kinds of income behave differently.** If capital income becomes the slow one and wages the fast one, the pattern flips: the middle's share now moves fastest. If both kinds of income behave the same way, every share moves at the same pace. *(6.2 and 6.3)*
3. **Two economies can be just as unequal and still behave very differently over time.** Five examples with similar inequality (a Gini coefficient between 0.28 and 0.37) give five different patterns: persistence that rises from bottom to top, falls, stays flat, dips in the middle or peaks in the middle. A snapshot of inequality cannot tell you how it will move. *(6.4)*
4. **How long a change lasts, how big the swings are and how often people overtake each other are three different things.** Making every random shock three times bigger makes shares swing much more and makes people change places far more often (in 64 years instead of 8), yet each change lasts about as long as before. *(6.5)*
5. **Following groups is not the same as following people.** Figures such as "the income share of the top 10%" follow a position in the distribution, not the same people. When people move up and down a lot, a position mixes people whose incomes behave differently, and the differences between them fade from view. *(6.6)*
6. **A trend changes how unequal the economy becomes, not how fast shares bounce back.** If the capital income of the richest person grows steadily, inequality keeps rising (the Gini coefficient goes from 0.32 to 0.40 over 110 years), but the hill shape of persistence stays. *(6.7)*
7. **A one-off redistribution lasts only as long as the income it touches.** Sharing capital income out equally, once, halves inequality, but within about ten years the economy is back where it started, because capital income quickly returns to its usual level. Doing the same with wages lasts much longer. Only a lasting change in each person's usual income, such as their skills or what they own, has a permanent effect. *(6.11)*

The simulator is a teaching and research tool built on five imaginary people. It shows how mechanisms work; it does not measure anything in real data.

## Quick start

1. Open the simulator. It starts with the example of point 1 above.
2. Look at **Figure 1**. Its title reads **Persistence peaks in the middle of the distribution**.
3. In the **Persistence** box, move the labor slider ($`\phi_L`$). Figure 1 changes at once.
4. Open the **Allocation** tab and pick a shape, for example **Increasing**. The simulator shares income out again to produce that pattern.
5. Press the play button, or the Space bar, to watch the years go by.

**Reset** puts everything back to the start.

## The model in one minute

Each person's labor income $`w`$ and capital income $`f`$ follow the same simple rule: this year's income equals a constant, plus a trend, plus a fraction $`\phi`$ of last year's income, plus a random shock. Total income $`y`$ is the sum of the two.

```math
w_{i,t} = \beta_{W,i} + \beta_{W,trend,i}\, t + \phi_{L}\, w_{i,t-1} + u_{i,t}
```

```math
f_{i,t} = \beta_{F,i} + \beta_{F,trend,i}\, t + \phi_{K}\, f_{i,t-1} + v_{i,t}
```

```math
y_{i,t} = w_{i,t} + f_{i,t}
```

The fraction $`\phi`$ is the persistence: $`\phi_L`$ for labor income and $`\phi_K`$ for capital income. The shocks $`u`$ and $`v`$ are random draws from a normal distribution. A person's income share is their total income $`y`$ divided by the total income of the five people.

The [technical reference](docs/TECHNICAL.md) gives the full model, every control, how each figure is built and the method notes.

## Authors

- **Abdelhamid El Hamrani**, Faculty of Governance, Economics and Social Sciences, Mohammed VI Polytechnic University
- **Hajar Hazmiri**, Faculty of Governance, Economics and Social Sciences, Mohammed VI Polytechnic University
- **Ramses Abul Naga**, supervisor, Faculty of Governance, Economics and Social Sciences, Mohammed VI Polytechnic University

## How to cite

If you use the simulator in teaching or research, please cite it as:

> El Hamrani, A., Hazmiri, H., & Abul Naga, R. (2026). *Inequality Persistence Simulator* (Version 1.2) [Computer software]. <https://github.com/abdelhamidelhamrani777/Inequality-Persistence-Simulator>

```bibtex
@software{inequality_persistence_simulator_2026,
  author  = {El Hamrani, Abdelhamid and Hazmiri, Hajar and Abul Naga, Ramses},
  title   = {Inequality Persistence Simulator},
  version = {1.2},
  year    = {2026},
  url     = {https://github.com/abdelhamidelhamrani777/Inequality-Persistence-Simulator}
}
```

## Related work

- Gomez, M. (2025). Macro perspectives on income inequality. *Journal of Economic Perspectives*, 39(2), 127–148.

---

<a id="francais"></a>

# Simulateur de persistance des inégalités

[English](#english) · **Français**

**Un outil interactif et gratuit qui montre pourquoi la part de revenu de certaines personnes évolue lentement alors que celle d'autres monte et descend brusquement, et ce que cela change pour les inégalités.**

**L'ouvrir :** utilisez la [version en ligne en français](https://abdelhamidelhamrani777.github.io/Inequality-Persistence-Simulator/?lang=fr), sans rien installer, ou téléchargez [`index.html`](index.html) et ouvrez-le dans votre navigateur. Le bouton **EN / FR** de la barre du haut change la langue.

**Apprendre avec :** le [guide d'utilisation](docs/GUIDE.md), en anglais, explique chaque bouton et chaque graphique, puis déroule douze expériences pas à pas.

## De quoi s'agit-il ?

On décrit généralement les inégalités par une photographie : la façon dont le revenu est partagé une année donnée. Ce simulateur regarde comment cette photographie évolue dans le temps. Quand la part de revenu d'un groupe augmente, reste-t-elle élevée, ou retombe-t-elle rapidement ?

Le simulateur suit cinq personnes imaginaires, Violette, Meryem, Bjørn, Alfred et George, sur de nombreuses périodes (pensez à des années). Chacune a deux sortes de revenu :

- **le revenu du travail** : salaires et traitements ;
- **le revenu du capital** : ce que rapporte ce que l'on possède, comme les loyers, les dividendes et les intérêts.

Chaque année, ces deux revenus subissent des hausses et des baisses aléatoires. Vous choisissez combien de temps ces variations durent pour chaque sorte de revenu, ce que chaque personne possède au départ et l'ampleur des chocs. Le simulateur montre alors comment évolue la part de chacun dans le revenu total, et comment évoluent les inégalités dans leur ensemble.

Le projet est issu d'un travail de recherche mené dans le cadre de mémoires de master à la Faculté de Gouvernance, Sciences Économiques et Sociales (FGSES) de l'Université Mohammed VI Polytechnique (UM6P), sous la direction du professeur Ramses Abul Naga.

## Trois mots à connaître

- **Part de revenu** : le revenu d'une personne divisé par le revenu de tous. Si George gagne 150 et que les cinq personnes gagnent 400 ensemble, sa part est de 37,5 %.
- **Persistance** : la durée pendant laquelle un changement se maintient, mesurée par un nombre qui va jusqu'à 1. Près de 1, un changement dure longtemps : une part qui augmente cette année reste élevée l'année suivante. Près de 0, il disparaît presque aussitôt. À 0,9, la moitié d'un changement est encore là après environ sept ans ; à 0,5, la moitié a disparu au bout d'un an.
- **Coefficient de Gini** : la mesure habituelle des inégalités. Il vaut 0 quand tout le monde a le même revenu et augmente à mesure que le revenu se concentre.

## Ce que montre le simulateur

Chaque résultat ci-dessous vient d'un exemple que vous pouvez refaire dans le simulateur. Le numéro entre parenthèses renvoie à l'expérience du [guide d'utilisation](docs/GUIDE.md) qui en donne les étapes.

1. **L'origine de vos revenus détermine la vitesse à laquelle votre part évolue.** Dans l'exemple de départ, un changement de salaire dure longtemps (persistance 0,9) et un changement de revenu du capital s'efface vite (0,5). Les personnes du milieu vivent surtout de leur salaire, donc leur part évolue lentement. Les plus pauvres et les plus riches dépendent davantage du revenu du capital, donc leur part monte et retombe vite. La persistance est donc la plus forte au milieu (0,83) et la plus faible aux deux extrémités (0,59 en bas, 0,69 en haut) : une forme en cloche. *(6.1)*
2. **Cela ne se produit que parce que les deux sortes de revenu se comportent différemment.** Si le revenu du capital devient le plus lent et le salaire le plus rapide, la forme s'inverse : c'est maintenant la part du milieu qui bouge le plus vite. Si les deux sortes de revenu se comportent de la même façon, toutes les parts évoluent au même rythme. *(6.2 et 6.3)*
3. **Deux économies aussi inégales l'une que l'autre peuvent évoluer très différemment.** Cinq exemples aux inégalités proches (un coefficient de Gini entre 0,28 et 0,37) donnent cinq formes différentes : une persistance qui augmente du bas vers le haut, qui diminue, qui reste plate, qui se creuse au milieu ou qui culmine au milieu. Une photographie des inégalités ne dit pas comment elles vont évoluer. *(6.4)*
4. **La durée d'un changement, l'ampleur des variations et la fréquence des dépassements sont trois choses différentes.** Rendre chaque choc aléatoire trois fois plus fort fait beaucoup plus varier les parts et fait changer les personnes de place bien plus souvent (en 64 années au lieu de 8), mais chaque changement dure à peu près aussi longtemps qu'avant. *(6.5)*
5. **Suivre des groupes, ce n'est pas suivre des personnes.** Des chiffres comme « la part de revenu des 10 % les plus riches » suivent une position dans la distribution, pas les mêmes personnes. Quand les gens montent et descendent souvent, une position mélange des personnes dont les revenus se comportent différemment, et leurs différences s'effacent. *(6.6)*
6. **Une tendance change le niveau des inégalités, pas la vitesse à laquelle les parts reviennent à la normale.** Si le revenu du capital de la personne la plus riche augmente régulièrement, les inégalités ne cessent de croître (le coefficient de Gini passe de 0,32 à 0,40 en 110 ans), mais la forme en cloche de la persistance demeure. *(6.7)*
7. **Une redistribution ponctuelle ne dure que le temps du revenu qu'elle touche.** Partager une seule fois le revenu du capital à parts égales divise les inégalités par deux, mais en une dizaine d'années l'économie revient à son point de départ, car le revenu du capital retrouve vite son niveau habituel. Faire de même avec les salaires dure beaucoup plus longtemps. Seul un changement durable du revenu habituel de chacun, comme ses compétences ou ce qu'il possède, a un effet permanent. *(6.11)*

Le simulateur est un outil d'enseignement et de recherche fondé sur cinq personnes imaginaires. Il montre comment fonctionnent des mécanismes ; il ne mesure rien sur des données réelles.

## Prise en main

1. Ouvrez le simulateur. Il démarre sur l'exemple du point 1 ci-dessus.
2. Regardez la **figure 1**. Son titre est **La persistance culmine au milieu de la distribution**.
3. Dans l'encadré **Persistance**, déplacez le curseur du travail ($`\phi_L`$). La figure 1 change aussitôt.
4. Ouvrez l'onglet **Répartition** et choisissez une forme, par exemple **Croissant**. Le simulateur répartit à nouveau les revenus pour produire cette forme.
5. Appuyez sur le bouton de lecture, ou sur la barre d'espace, pour voir défiler les années.

**Réinitialiser** remet tout à l'état de départ.

## Le modèle en une minute

Le revenu du travail $`w`$ et le revenu du capital $`f`$ de chaque personne suivent la même règle simple : le revenu de cette année est égal à une constante, plus une tendance, plus une fraction $`\phi`$ du revenu de l'année précédente, plus un choc aléatoire. Le revenu total $`y`$ est la somme des deux.

```math
w_{i,t} = \beta_{W,i} + \beta_{W,trend,i}\, t + \phi_{L}\, w_{i,t-1} + u_{i,t}
```

```math
f_{i,t} = \beta_{F,i} + \beta_{F,trend,i}\, t + \phi_{K}\, f_{i,t-1} + v_{i,t}
```

```math
y_{i,t} = w_{i,t} + f_{i,t}
```

La fraction $`\phi`$ est la persistance : $`\phi_L`$ pour le revenu du travail et $`\phi_K`$ pour le revenu du capital. Les chocs $`u`$ et $`v`$ sont tirés au hasard dans une loi normale. La part de revenu d'une personne est son revenu total $`y`$ divisé par le revenu total des cinq personnes.

La [référence technique](docs/TECHNICAL.md), en anglais, détaille le modèle complet, chaque réglage, la construction de chaque figure et les notes de méthode.

## Auteurs

- **Abdelhamid El Hamrani**, Faculté de Gouvernance, Sciences Économiques et Sociales, Université Mohammed VI Polytechnique
- **Hajar Hazmiri**, Faculté de Gouvernance, Sciences Économiques et Sociales, Université Mohammed VI Polytechnique
- **Ramses Abul Naga**, encadrant, Faculté de Gouvernance, Sciences Économiques et Sociales, Université Mohammed VI Polytechnique

## Citer le simulateur

Si vous utilisez le simulateur dans un cours ou une recherche, merci de le citer ainsi :

> El Hamrani, A., Hazmiri, H., & Abul Naga, R. (2026). *Inequality Persistence Simulator* (version 1.2) [logiciel]. <https://github.com/abdelhamidelhamrani777/Inequality-Persistence-Simulator>

La référence BibTeX figure dans la partie anglaise ci-dessus.
