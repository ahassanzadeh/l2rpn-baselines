# Contribute to L2RPN_Baselines

This document is a handbook to writing a new baseline.

We provide this guide and a [Template baseline](/l2rpn_baselines/Template) to help you getting started.

# Handbook Menu
*   [1. One baseline, one submodule](#one-baseline-one-submodule)
*   [2. Submodule requirements](#submodule-requirements)
    *   [2.1. MyContrib\/\_\_init\_\_.py](#mycontrib__init__py)
    *   [2.2. MyContrib.MyContribBaseline_eval](#mycontribmycontribbaseline_eval)
    *   [2.3. MyContrib.MyContribBaseline_train (optional)](#mycontribmycontribbaseline_train-optional)
    *   [2.4. MyContrib\/MyContrib.md (optional)](#mycontribmycontribmd-optional)

# One baseline, one submodule
As shown by the [Template baseline](/l2rpn_baselines/Template), it is expected from baselines to take the form a python submodule
```bash
tree ./l2rpn_baselines/Template/


./l2rpn_baselines/Template/
├── evaluate.py
├── __init__.py
├── TemplateBaseline.py
├── Template.md
└── train.py

0 directories, 5 files
```

# Submodule requirements

## MyContrib\/\_\_init\_\_.py
In the `__init__.py` file, is used to export the entry points of your baseline.

You MUST export the following entry points in the \_\_init\_\_.py file:

- `MyContribBaseline` [**required**] :
   The class of your baseline, implementing `grid2op.Agent.BaseAgent` or `grid2op.Agent.AgentWithConverter`
- `MyContribBaseline_eval` [**required**] :
   the `evaluate` function as described in the next section
- `MyContribBaseline_train` [**optional**]:
   the `train` function as described in the second-next section

```python
__all__ = [
    "MyContribBaseline",
    "MyContribBaseline_eval",
    "MyContribBaseline_train"
]

from l2rpn_baselines.MyContrib.MyContribBaseline import MyContribBaseline
from l2rpn_baselines.MyContrib.evaluate import evaluate as MyContribBaseline_eval
from l2rpn_baselines.MyContrib.train import train as MyContribBaseline_train
```

## MyContrib.MyContribBaseline_eval

This is the exported name of a function used to evaluate the performances of your baseline.

The function MUST respect the signature provided below.

We recommend using the runner and placing this function in a separate file.

```python
def evaluate(env,
             load_path=".",
             logs_path=None,
             nb_episode=1,
             nb_process=1,
             max_steps=-1,
             verbose=False,
             save_gif=False,
             **kwargs)
```

However, you CAN change the default values of the arguments.

 - env:`grid2op.Environment.Environment` The environment on which the baseline will be evaluated.
 - load_path: `str` The path where the model is stored. This is used by the agent when calling `agent.load`
 - logs_path: `str` The path where the agents results will be stored.
 - nb_episode: `int` Number of episodes to run for the assessment of the performance.
 - nb_process: `int` Number of process to be used for the assessment of the performance.
 - max_steps: `int` Maximum number of timestep each episode can last. It should be a positive integer or -1.
        -1 means that the entire episode is run (until the chronics is out of data or until a game over).
 - verbose: `bool` verbosity of the output
 - save_gif: `bool` Whether or not to save a gif into each episode folder corresponding to the representation of the said episode.
 - kwargs: Other key words arguments that you are free to use.

## MyContrib.MyContribBaseline_train (optional)
  
This is the exported name of a function used to train your baseline.

If you choose to provide it, the function MUST respect the signature provided below. 

We also recommend placing this function in a separate file.

```python
def train(env,
          name="MyContrib",
          iterations=1,
          save_path=None,
          load_path=None,
          **kwargs)
```
However, you CAN change the default values of the arguments.

 - env: `grid2op.Environment.Environment` The environmnent on which the baseline will be trained
 - name: `str` Fancy name you give to this baseline.
 - iterations: `int` Number of training iterations to perform
 - save_path: `str` The path where the baseline will be saved during / at the end of the training.
 - load_path: ``str`` Path where to look for reloading the model. Use ``None`` if no model should be loaded.
 - kwargs: Other key-word arguments that you might use for training.

## MyContrib\/MyContrib.md (optional)

It is encouraged to provide a markdown file at the root of your baseline submodule containing:

 - The authors names
 - A contact email address
 - Some information on the baseline's performances
 - A reference to a paper (if applicable)
 - Training time (if applicable)
 - Training enviroment (if applicable)
 - Number of training iterations (if applicable)
 - Values of hyperparameters (if applicable)