<!-- All text in this folder follows ASD-STE100 Simplified Technical English.
     Keep sentences short and active. Use simple tenses. Use one word for one
     meaning. Do not use metaphors, idioms, or contractions. -->

# Statistical Design of Experiments

**A workshop: design, do, and analyze a factorial experiment on a color mixing
self-driving lab.**

You get a robot, three dyes, and 26 experiments. You cannot get more. This limit
controls every decision in the workshop. When experiments are expensive, the
selection of the experiments becomes the most important step.

The equipment is an [Opentrons OT-2](https://opentrons.com/) liquid handler. The
[Acceleration Consortium](https://acceleration.utoronto.ca/) operates it. The
robot puts red, yellow, and blue dye in a well, mixes them, and measures the
mixture with a light sensor that has 8 channels. You use the equipment through
the internet. [`BO.ipynb`](./BO.ipynb) in this folder uses the same platform.

| At a glance | |
| --- | --- |
| **Notebook** | [`DOE.ipynb`](./DOE.ipynb). Fill in the blanks. 10 tasks. |
| **You need** | Python basics: variables, loops, and functions. Some knowledge of `pandas`. |
| **Offline?** | Yes. The notebook contains a simulator of the equipment. |
| **Time** | 2 to 3 hours in simulation, and one hour on the robot. |

---

## Why you must design the experiment

You want to know how each dye changes the color. The usual method is **one
factor at a time** (OFAT). You keep yellow and blue constant and change red.
Then you keep red and blue constant and change yellow. This method looks
careful.

OFAT has one large problem. The effect of red can change with the quantity of
blue, because the two dyes absorb the same light. OFAT tests red at one level of
blue only. Therefore OFAT cannot find this behavior. OFAT measures **main
effects**, and it is blind to **interactions**.

A **factorial design** changes all the factors together, in a balanced pattern.
Each experiment then gives data about each effect.

| Method | What it measures | Experiments for 3 factors at 2 levels |
| --- | --- | --- |
| OFAT | 3 main effects | about 7 |
| **2³ full factorial** | 3 main effects, 3 two-way interactions, 1 three-way interaction | **8** |

The two methods use almost the same number of experiments. The factorial design
gives more data. The difference increases when you add more factors. This is the
primary result of the DoE method.

### DoE and Bayesian optimization answer different questions

This folder has one workshop for each method, on the same equipment:

| Item | [`DOE.ipynb`](./DOE.ipynb) | [`BO.ipynb`](./BO.ipynb) |
| --- | --- | --- |
| **Question** | How does this system operate? | What is the best recipe? |
| **Experiment selection** | All experiments, before you start | One experiment at a time |
| **Result** | Effect sizes, p-values, a response surface | One best recipe |
| **Use it when** | You must understand the system, or write a report | You must find one target |

If you use your budget on the wrong method, you get a good answer to a question
that you did not ask. The [`bayesian_opt/`](../bayesian_opt/) workshop gives more
data about the second method.

---

## The equipment

<p align="center">
<img src="https://github.com/sparks-baird/self-driving-lab-demo/blob/main/notebooks/map-diagram-2.png?raw=true" width="340">
</p>

A **self-driving laboratory** closes the loop. The equipment does an experiment.
The sensors measure the result. An algorithm selects the next experiment. Then
the loop starts again. The color mixing OT-2 is a small example of this loop. It
is small enough to teach with, and real enough to make errors.

| Component | Detail |
| --- | --- |
| **Robot** | Opentrons OT-2 pipetting platform |
| **Factors** | The volume of the red, the yellow, and the blue dye. 1 µL to 299 µL each. |
| **Sensor** | 8 channels, near 410, 440, 470, 510, 550, 583, 620, and 670 nm |
| **Interface** | Gradio API. It receives `(student_id, r_vol, y_vol, b_vol)` and returns 8 values. |
| **Speed** | About 2 minutes for each job. The jobs go in a queue. |

Three limits control all the steps that come after:

1. **26 submissions.** You get 25, and one more to test your code.
2. **Use 10 µL or more.** The API accepts 1 µL. But below 10 µL the pipette
   error is large. This error can be larger than the effects that you measure.
3. **Stray light that changes with the well position.** Ambient light in the
   OT-2 goes into the sensor. This light is not random. It stays the same for
   the same well. Therefore it is a **systematic error**, and replication does
   not remove it. To find this error is one of the goals of the workshop.

The background text and the figures in Part 0 come from
[`self-driving-lab-demo`](https://github.com/sparks-baird/self-driving-lab-demo)
by Sparks and Baird. The notebook
[`4_2_paho_mqtt_colab_sdl_demo_test.ipynb`](./4_2_paho_mqtt_colab_sdl_demo_test.ipynb)
in this folder shows the light mixing version of the same platform.

---

## The experiment

The design has three factors and two levels. It also has some more experiments
for safety:

| Block | Experiments | What it gives you |
| --- | ---: | --- |
| 2³ factorial corners, two replicates | 16 | all main effects, all interactions, and **pure error** |
| Center points | 4 | a **curvature** test that a two-level design cannot do |
| Validation points | 5 | an independent test of the predictions from your model |
| Pipeline test | 1 | proof that your code operates correctly |
| **Total** | **26** | |

The default levels are 30 µL (low) and 90 µL (high). At these levels, each
volume is more than the 10 µL accuracy limit. The largest total volume is
3 × 90 = 270 µL, and this quantity stays in the well.

The sequence of the experiments is random, with seed 403. A slow drift during
the hour can look like a dye effect. Randomization prevents this.

**The notebook makes these items:**

- **Figure 1** — a ternary diagram of the sample positions
- **Figure 2** — a ternary response surface of the measured data
- **Figure 3** — the expected color against the measured color
- **Figure 4** — a normal probability plot of the residuals
- **Table** — a three-factor factorial ANOVA, with all main effects and all
  interactions
- **CSV** — the RYB inputs and the 8 responses, for use again later

---

## How to run the notebook

Open [`DOE.ipynb`](./DOE.ipynb) in
[Colab](https://colab.research.google.com/github/daleas0120/sdl-workshops/blob/main/statistical_doe/DOE.ipynb),
or on your computer with `jupyter lab`. You need `numpy`, `pandas`,
`statsmodels`, `plotly`, `scipy`, and `matplotlib`. You need `gradio_client` for
the robot only.

**The notebook does not run from the first cell to the last cell. This is
deliberate.** Each task cell has `___` blanks. If you do not replace a blank,
the cell stops with a `NameError`. Then it is your turn to write the code.

Each task has a brief, a `▸ Hints` block, and a ✅ check cell. The check cell
tells you if your code is correct. The Solutions appendix at the end has the
full answers. Do the task first. You learn from the blanks, and not from the
answers.

### Use the simulator first, and the robot after

The notebook contains a Beer-Lambert simulator of the equipment. The simulator
has the two error sources of the real equipment. Write and test your code
against the simulator. It is free:

```python
USE_HARDWARE = False     # simulation. Run it as many times as necessary.
STUDENT_ID = "test1"
SEED = 403
```

Change one line when your time slot starts and all the checks pass:

```python
USE_HARDWARE = True      # each experiment now uses your quota
```

The notebook writes each result to a CSV file immediately, and not at the end.
If the queue stops at experiment 19, you keep 18 measurements. You do not lose
the full hour.

> ⚠️ **Prepare before your time slot.** Make your design, run all the checks
> against the simulator, and select your RYB combinations. You get one slot
> only.

---

## The tasks in the notebook

| Section | Task | What you write |
| --- | --- | --- |
| **Part 0** | Background | Nothing. Read about self-driving labs, the equipment, and the DoE terms. |
| **Part 1** | Setup | Nothing. Constants, the API client, the simulator, and support functions. |
| **Part 2** | 1 · Make the design matrix | Levels, corners, replicates, center points, and randomization |
| | 2 · Calculate the expected color | The Beer-Lambert model, and the color square |
| **Part 3** | 3 · Do the experiments | The submit loop, the data, and the log file |
| **Part 4** | 4 · Make the response | Absorbance, `A_total`, and the RMSE to the prediction |
| **Part 5** | 5 · Calculate the effects manually | Coded ±1 columns, main effects, and interactions |
| | 6 · ANOVA | The `statsmodels` model and table. It confirms your arithmetic. |
| **Part 6** | 7 · Ternary diagrams | Figure 1 and Figure 2 |
| **Part 7** | 8 · Examine the residuals | Figure 4, the drift test, and the position test |
| | 9 · Expected against measured | Figure 3, the parity plot, and the color squares |
| | 10 · Calibrate the dyes *(optional)* | A least squares fit of the absorptivities |
| **Part 8** | Write the report | Effects, error analysis, methods to decrease the errors, summary |
| **Part 9** | Checklist | Each item in the assignment, and its position |

The notebook also prepares three results for you to find:

- A response on the wrong scale makes interactions that are not real.
- `(30, 30, 30)` and `(90, 90, 90)` can be the same color.
- The difference between your expected color and your measured color has a sign.
  That sign tells you the mechanism of the error.

---

## References

> Baird, S. G.; Sparks, T. D. What Is a Minimal Working Example for a
> Self-Driving Laboratory? *Matter* **2022**, 5 (12), 4170–4178.
> <https://doi.org/10.1016/j.matt.2022.11.007>

> Baird, S. G.; Sparks, T. D. Building a "Hello World" for Self-Driving Labs:
> The Closed-Loop Spectroscopy Lab Light-Mixing Demo. *STAR Protocols* **2023**,
> 4 (2), 102329. <https://doi.org/10.1016/j.xpro.2023.102329>

- Montgomery, D. C. *Design and Analysis of Experiments*. This is the standard
  text. Chapters 5 and 6 give the theory of factorial designs and 2ᵏ analysis.
- [NIST/SEMATECH e-Handbook of Statistical Methods, chapter 5](https://www.itl.nist.gov/div898/handbook/pri/pri.htm).
  This handbook is free, and it covers DoE fully.
- [`self-driving-lab-demo`](https://github.com/sparks-baird/self-driving-lab-demo).
  This is the open-source demo that gives the background in Part 0.
- [Acceleration Consortium](https://acceleration.utoronto.ca/maps). This group
  operates the OT-2 platform, and it keeps a list of materials acceleration
  platforms.
- [Ax](https://ax.dev/) and [BoTorch](https://botorch.org/). These libraries do
  the Bayesian optimization in [`BO.ipynb`](./BO.ipynb).
