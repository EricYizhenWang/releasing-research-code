# Robust Learning against Logical Adversaries

This repository is the official implementation of Neurips 2020 Submission 8862.

## Requirements

There are two main requirements to run the code -- setting up the environment and obtaining data set.

To setup the necessary dependencies, please use anaconda to install the packages specified in environment file nn_mal.yml

To obtain the Sleipnir data set, please fill in the request as shown in https://github.com/ALFA-group/robust-adv-malware-detection . 

Once the data set access is obtained, please unzip the data set under folder under ./help_files/data/.
So for example, the feature vector of a malware named mal.exe should have path ./help_files/data/sleipnir-dataset/malicious/mal.exe

## Running Experiment

### Step 1: Normalize Data

For our unified approach, the data inputs need to be normalized. When the data set is properly unzipped, run
```
python data_normalization.py
```
to create the normalized vectors. The normalized vectors will be stored in malicious_normalized and benign_normalized folder under sleipnir-dataset. So for a malware named mal.exe, its normalized vector will be in file ./help_files/data/sleipnir-dataset/malicious_normalized/mal.exe

### Step 2: Run Experiment

To run experiment, use the command with an integer argument i representing the index for the data split.
```
python run_experiments.py [i]
```
For example, python run_experiments.py 0 will run experiment over the 0-th data split.
The data split information is stored under ./result_files/[i]. If the folder does not exist, the script will automatically generate a new train/test/validation data split and create the corresponding folder.

The run_experiment.py script updates the parameters.ini file and call framework-lite.py script to run experiment using the specified parameters. The user can change the set of training methods and evasion methods used in the experiment. Notice that the experiments for normalized and un-normalized inputs are seperated. So please run the script twice by activating/commenting the corresponding codes as explained in the scripts.

### Step 3: Read Results

The result files will be created under ./result_files/[i] for the corresponding i-th split as well. There are three main types of files:
   - [training:training-method|evasion:evading-method]_run_experiments.json contains the experiment statistics including FNR and FPR with/without attack.
   - [training:training-method|evasion:evading-method]_run_experiments-model.pt saves the final model obtained from training.
   - [training:training-method|evasion:evading-method]_run_experiments.loss stores the training/validation losses over epoches as evidence for convergence.

## Nomenclature of Attacks

Last, we want to explain the naming of attacks in the experiment.

   - greedy_grad_all-[n_iter]x[n_transformation_per_iter] is the GreedyByGrad attack that uses both additive and equivalent relations, e.g. greedy_grad_all-5x10 means using GreedyByGrad attack with 5 iterations and 10 transformations in each iteration.
   - greedy_grad_sub-[n_iter]x[n_transformation_per_iter] is the GreedyByGrad attack that uses only equivalent relations.
   - ggroup_add-[n_iter_sub]x[n_max_instances]x[n_iter_add] is the GreedyByGroup attack that uses both additive and equivalent relations, e.g. ggroup_add-20x10x50 means using equivalent relation for 20 iterations, each iteration checks at most 2^10 combinations in each equivalent group, and finally using additive relation attacks for 50 iterations.
   - greedy_group[n_iter]x[n_max_instance] is the GreedyByGroup attack that only uses equivalent relation.
   - rfgsm_k attack is the additive attack baseline used by Al-Dujaili et al. We set number of iterations k=50.
   - normalization+adv_training is specific for normalized inputs. It uses rfgsm_k over normalized inputs.
