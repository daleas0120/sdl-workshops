<!-- All text in this repository follows ASD-STE100 Simplified Technical English.
     Keep sentences short and active. Use simple tenses. Use one word for one
     meaning. Do not use metaphors, idioms, or contractions. -->

# SDL Workshops

This repository holds two workshops about experiment design on a
self-driving laboratory (SDL). A self-driving laboratory is a system that
runs an experiment, measures the result, and selects the next experiment
with an algorithm. The workshops use a color-mixing platform built on an
[Opentrons OT-2](https://opentrons.com/) liquid handler, operated by the
[Acceleration Consortium](https://acceleration.utoronto.ca/).

**You must open the notebooks in Google Colab to run them.** Each notebook
has an "Open in Colab" link at the top. Click this link. Colab gives you a
free, ready-to-use Python environment in your browser. You do not need to
install Python or any library on your own computer.

To use Colab, you need a Google account. Sign in to
[Google Colab](https://colab.research.google.com/) with this account before
you open a notebook link.

---

## The workshops

| Folder | Question it answers | Notebook |
| --- | --- | --- |
| [`statistical_doe/`](./statistical_doe/) | How does this system operate? | [`DOE.ipynb`](./statistical_doe/DOE.ipynb) |
| [`bayesian_opt/`](./bayesian_opt/) | What is the best recipe? | [`bayesian_optimization_tutorial_RMSE_2026.ipynb`](./bayesian_opt/bayesian_optimization_tutorial_RMSE_2026.ipynb) |

**[`statistical_doe/`](./statistical_doe/README.md)** teaches the Design of
Experiments (DoE) method. You plan a full set of experiments before you
start. You then use the results to find the main effects and the
interactions between the red, yellow, and blue dyes. This workshop can run
against a simulator or against the real OT-2 robot.

**[`bayesian_opt/`](./bayesian_opt/README.md)** teaches Bayesian
optimization. You select one experiment at a time, and each new experiment
uses all prior measurements. The workshop compares this method against
random sampling and greedy coordinate search, on a simulator of the same
OT-2 platform. It does not need the real robot.

If you use your experiment budget on the wrong method, you get a good
answer to a question that you did not ask. Read the "Why you must design
the experiment" section in the
[`statistical_doe` README](./statistical_doe/README.md) for the full
comparison of the two methods.

---

## Before you start

1. Open the notebook you want in Google Colab. Use the link at the top of
   the notebook, or the links in this README.
2. Run the setup cells first. Some notebooks install one or more Python
   libraries with `pip`. This step can take one minute.
3. Read each task before you write code. Each workshop folder has its own
   README with the full instructions, the equipment description, and the
   list of tasks.

---

## License

This repository is under the MIT License. See [`LICENSE`](./LICENSE) for
the full text.
