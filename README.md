# UAI2020 submission 344
This repo is for the UAI submission number 344: ICE-BeeM: Identifiable Conditional Energy-Based Deep Models

This is the code to run the simulations presented in the manuscript as well as the transfer learning experiments (it also allows for additional experiments not discussed in the manuscript)

## Dependencies

This project was tested with the following versions:

- python 3.6 and 3.7
- pytorch 1.4
- tensorflow 1.14
- PyYAML 5.3.1
- seaborn 0.10
- scikit-learn 0.22.2
- scipy 1.4.1

## Running Simulations
To reproduce the simulations, run for e.g.:

```
python simulations.py --dataset IMCA --method icebeem
```

Type `python simulations.py --help` to learn about the arguments:

```
usage: simulations.py [-h] [--dataset DATASET] [--method METHOD]
                      [--config CONFIG] [--run RUN] [--nSims NSIMS] [--test]
                      [--plot]

optional arguments:
  -h, --help         show this help message and exit
  --dataset DATASET  Dataset to run experiments. Should be TCL or IMCA
  --method METHOD    Method to employ. Should be TCL, iVAE or ICE-BeeM
  --config CONFIG    Path to the config file
  --run RUN          Path for saving running related data.
  --nSims NSIMS      Number of simulations to run
  --test             Whether to evaluate the models from checkpoints
  --plot             Plot comparison of performances
```

The results of each simulation is saved in the value of the flag `--run` (defulat is `run/` folder).

## Running real data experiments
These experiments are run through the script `main.py`. To learn about its arguments type `python main.py --help`:

```
usage: main.py [-h] [--dataset DATASET] [--config CONFIG] [--run RUN] [--doc DOC] [--nSims NSIMS] [--SubsetSize SUBSETSIZE] [--seed SEED] [--all] [--baseline] [--semisupervised] [--transfer] [--plot]

optional arguments:
  -h, --help            show this help message and exit
  --dataset DATASET     Dataset to run experiments. Should be MNIST or CIFAR10, or FMNIST
  --config CONFIG       Path to the config file
  --run RUN             Path for saving running related data.
  --doc DOC             A string for documentation purpose
  --nSims NSIMS         Number of simulations to run
  --SubsetSize SUBSETSIZE
                        Number of data points per class to consider -- only relevant for transfer learning
  --seed SEED           Random seed
  --all                 Run transfer learning experiment for many seeds and subset sizes -- only relevant for transfer learning
  --baseline            Run the script for the baseline
  --semisupervised      Run semi-supervised experiments
  --transfer            Run the transfer learning experiments after pretraining
  --plot                Plot transfer learning experiment for the selected dataset
```

### Transfer learning
To run the experiment on MNIST:

```
# pretrain an ICE-BeeM on labels 0-7
python main.py --dataset MNIST --config mnist.yaml --doc mnist
# fix f and only learn g on labels 8-9 for many different dataset sizes and different seeds
python main.py --dataset MNIST --config mnist.yaml --doc mnist --transfer --all
# train an ICE-BeeM on labels 8-9 for many different dataset sizes and different seeds
python main.py --dataset MNIST --config mnist.yaml --doc mnist --transfer --baseline --all
```

The results are saved in the value of the flag `--run` (defulat is `run/` folder). To plot the comparison for MNIST after running the steps above:

```
python main.py --dataset MNIST --plot
```

The same can be done on CIFAR-10 by changing the value of the flag `--dataset` to `CIFAR10` and of the flag `--config` to `cifar.yaml`. Also make sure to change the value of `--doc` not to overwrite the mnist checkpoints.

### Semi-supervised learning
To run on MNIST:

```
# pretrain an ICE-BeeM on labels 0-7
python main.py --dataset MNIST --config mnist.yaml --doc mnist
# pretrain an unconditional EBM on labels 0-7
python main.py --dataset MNIST --config mnist.yaml --doc mnist --baseline
# classify labels 8-9 using the pretrained ICE-BeeM
python main.py --dataset MNIST --config mnist.yaml --doc mnist --semisupervised
# classify labels 8-9 using the pretrained unconditional EBM
python main.py --dataset MNIST --config mnist.yaml --doc mnist --semisupervised --baseline
```

The same can be done on CIFAR-10 by changing the value of the flag `--dataset` to `CIFAR10` and of the flag `--config` to `cifar.yaml`. Also make sure to change the value of `--doc` not to overwrite the mnist checkpoints.
