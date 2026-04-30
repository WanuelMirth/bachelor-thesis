# Compensation of Overestimation Bias: A Comparison of DQN and DDQN

This repository contains the complete source code and experimental data for the bachelor thesis "Compensation of Overestimation Bias in Reinforcement Learning: A Comparison of DQN and DDQN in a Racing game Environment".

The project investigates whether the mechanism introduced by Double Deep Q-Networks (DDQN) for reducing Q-value overestimation leads to measurably superior performance compared to the standard Deep Q-Network (DQN) in a feature-based, deterministic racing game environment.

## Brief Explanation

The project consists of two main parts:
1.  **A custom 2D racing game environment** developed with Pygame. The environment provides the agent with an 18-dimensional state vector, consisting of 16 distance sensors as well as the linear and angular velocity of the vehicle.
2.  **An implementation of the DQN and DDQN algorithms** using TensorFlow and Keras. Both agents use exactly the same network architecture and hyperparameters to enable a fair and controlled comparison.

All experiments, configurations, and results are automatically saved in the `experiments/` directory.

## Installation Instructions

The project was developed with Python 3.x. To install the necessary packages, using a virtual environment (e.g. with `venv`) is recommended.

### 1. Cloning the Repository:

```bash
git clone https://github.com/WanuelMirth/bachelor-thesis.git
cd bachelor-thesis
```

### 2. Creating and Activating a Virtual Environment (optional, but recommended):

#### Windows:

```bash
python -m venv venv
.\venv\Scripts\activate
```

#### macOS / Linux:

```bash
python3 -m venv venv
source venv/bin/activate
```

### 3. Installation of Dependencies:

```bash
pip install tensorflow
pip install pygame
pip install numpy
```

## Training the Agents

The training is controlled via the main script `main.py`.

### Easy Start

To start a training run with default parameters, execute the following command:

```bash
python main.py
```

By default, the DQN agent is trained. To train the DDQN agent:

```bash
python main.py --agent ddqn
```

Results, logs, and model checkpoints are automatically saved in a new folder under `experiments/[agent-name]/[timestamp]/`.

### Reproduction of Thesis Results

For the results presented in the thesis, a fixed set of 5 seeds was used to ensure statistical robustness. The seeds used are:

`[0, 1, 2, 3, 4]`

To reproduce a specific run (e.g. DQN with Seed 3), set the `PYTHONHASHSEED` environment variable before starting:

#### macOS / Linux:

```bash
PYTHONHASHSEED=3 python main.py --agent dqn
```

#### Windows (PowerShell):

```powershell
$env:PYTHONHASHSEED=3; python main.py --agent dqn
```

## Monitoring and Evaluation

The results of each training run are logged in two ways to enable analysis:

### 1. Live Monitoring with TensorBoard

All important metrics are written to TensorBoard logs in real-time. To monitor training progress live or analyze it after completion, start TensorBoard and point it to the `experiments` directory:

```bash
tensorboard --logdir experiments
```
Then open the URL displayed in the console (usually `http://localhost:6006`) in your browser. There you can compare the learning curves for different runs.

### 2. Raw Data as CSV

For a detailed statistical analysis, all metrics are additionally saved in a `metrics.csv` file within the respective run directory. These files form the basis for the aggregated graphs and further analyses created in the thesis.

### 3. Results
The following figures show the aggregated results from the experiments:

**Training progress over all seeds** 
![Training progress](results/score_comparison.png)

**Comparison of estimated Q-values** 
![Q values](results/qvalue_comparison.png)

---

## Optional Parameters

The script `main.py` accepts the following command-line arguments to control hyperparameters:

| Argument                 | Default Value | Explanation                                                          |
| ------------------------ | ------------ | ------------------------------------------------------------------ |
| `--agent`                | "dqn"        | Selects the agent type (`dqn` or `ddqn`).                     |
| `--layers`               | 18 18 18     | Number of neurons in the hidden layers.                  |
| `--lr`                   | 0.0001       | Learning rate ($\alpha$) for the Adam optimizer.                        |
| `--gamma`                | 0.99         | Discount factor ($\gamma$) for future rewards.               |
| `--batch_size`           | 512          | Number of transitions per training batch.                       |
| `--mem_size`             | 250000       | Maximum number of transitions in the replay buffer.                  |
| `--exploration_steps`    | 12500        | Number of random steps at the beginning to fill the buffer.   |
| `--replace_target_steps` | 10000        | Frequency at which the target network is updated.         |
| `--save_interval`        | 100          | Saves the model every X episodes.                              |
| `--epsilon_start`        | 1.0          | Starting value for the $\epsilon$-greedy strategy.                     |
| `--epsilon_end`          | 0.1          | Minimum value for $\epsilon$.                                     |
| `--epsilon_decay_steps`  | 250000       | Number of steps for linear epsilon reduction.                |
| `--render_freq`          | 0            | Rendering frequency of the environment. `0` means no rendering.            |
| `--max_steps`            | 50000000     | Total number of training steps.                                |
