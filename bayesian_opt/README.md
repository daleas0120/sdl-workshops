<!-- All text in this folder follows ASD-STE100 Simplified Technical English.
     Keep sentences short and active. Use simple tenses. Use one word for one
     meaning. Do not use metaphors, idioms, or contractions. -->

# Bayesian Optimization for Experimental Design

**A workshop: use Bayesian optimization to find a target color on a
simulated color-mixing self-driving lab, and compare the result against two
simpler methods.**

The notebook does not need the real robot. It uses a simulator of an
[Opentrons OT-2](https://opentrons.com/) liquid handler, the same platform
that the [Acceleration Consortium](https://acceleration.utoronto.ca/)
operates for the [`statistical_doe/`](../statistical_doe/) workshop. The
simulator mixes red, yellow, and blue dye, and returns a simulated
eight-channel spectrum.

**You must open the notebook in Google Colab to run it.** Use the "Open in
Colab" link at the top of the notebook, or the link below. You do not need
to install Python or any library on your own computer.

| At a glance | |
| --- | --- |
| **Notebook** | [`bayesian_optimization_tutorial_RMSE_2026.ipynb`](./bayesian_optimization_tutorial_RMSE_2026.ipynb) |
| **You need** | Python basics: variables, loops, and functions. Some knowledge of `numpy`. |
| **Equipment** | None. The notebook contains a full simulator. |
| **Time** | About 1 to 2 hours. |

---

## The question this workshop answers

Design of Experiments (DoE) selects all experiments before you start. It
answers the question: how does this system operate? Bayesian optimization
selects one experiment at a time, and each new experiment uses every prior
measurement. It answers a different question: what is the best recipe?

This workshop gives you a fixed target spectrum and a budget of 26 simulated
experiments. Your task is to find the mixture of red, yellow, and blue dye
that produces this target spectrum, using as few experiments as possible.

## The three methods in the notebook

| Method | Learns from prior measurements | Models interactions between dyes |
| --- | --- | --- |
| Random sampling | No | No |
| Greedy coordinate search | Yes | No |
| Bayesian optimization | Yes | Yes |

**Random sampling** draws a new feasible mixture at each step. It gives a
baseline: how well can you do without a search strategy?

**Greedy coordinate search** changes one dye volume at a time, and keeps the
value with the smallest error. It learns from measurements, but it cannot
find an interaction between two dyes.

**Bayesian optimization** fits a Gaussian Process to every measurement so
far, and uses Expected Improvement to select the next mixture. It models
the full three-dye space at once, including interactions.

## The objective

The simulator returns eight sensor values. The notebook reduces this to one
number: the root mean squared error (RMSE) between the measured spectrum
and the target spectrum, normalized by a reference spectrum. A smaller RMSE
is a better match. The task is:

```text
minimize   RMSE(measured spectrum, target spectrum)
subject to   each dye volume >= 10 microliters
             total volume <= 300 microliters
```

## How to run the notebook

Open
[`bayesian_optimization_tutorial_RMSE_2026.ipynb` in Colab](https://colab.research.google.com/github/daleas0120/sdl-workshops/blob/main/bayesian_opt/bayesian_optimization_tutorial_RMSE_2026.ipynb).
Run the cells from top to bottom. The notebook needs `numpy`, `pandas`,
`matplotlib`, `scipy`, and `scikit-learn`. Colab provides all of these
libraries by default.

Some cells have `_` blanks. Replace each blank with the correct value or
expression. If you do not replace a blank, the cell stops with an error.

## The tasks in the notebook

| Section | Content |
| --- | --- |
| 1 | The OT-2 simulator, and one example simulated measurement |
| 2 | The optimization objective: the normalized spectral RMSE |
| 3 | Baseline A: random sampling |
| 4 | Baseline B: greedy coordinate search |
| 5 | The Bayesian optimization algorithm: the Gaussian Process and Expected Improvement |
| 6 | A comparison of the three methods, by best error found against number of experiments |
| 7 | Summary: what each method does, and does not, capture |
| 9 | Optional challenges: change the Gaussian Process kernel, the noise level, and the Expected Improvement exploration parameter |
| 10 | Final challenge: compare several optimizer variants under the same budget |

---

## References

> Baird, S. G.; Sparks, T. D. What Is a Minimal Working Example for a
> Self-Driving Laboratory? *Matter* **2022**, 5 (12), 4170-4178.
> <https://doi.org/10.1016/j.matt.2022.11.007>

- [Acceleration Consortium](https://acceleration.utoronto.ca/maps). This
  group operates the OT-2 platform, and it keeps a list of materials
  acceleration platforms.
- [scikit-learn Gaussian Process documentation](https://scikit-learn.org/stable/modules/gaussian_process.html).
  This is the library that fits the surrogate model in this notebook.
- [`statistical_doe/`](../statistical_doe/). This workshop uses the same
  color-mixing platform to teach the Design of Experiments method.
