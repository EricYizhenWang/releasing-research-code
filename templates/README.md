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
-- [training:training-method|evasion:evading-method]_run_experiments.json contains the experiment statistics including FNR and FPR with/without attack.

## Training

To train the model(s) in the paper, run this command:

```train
python train.py --input-data <path_to_data> --alpha 10 --beta 20
```

> 📋Describe how to train the models, with example commands on how to train the models in your paper, including the full training procedure and appropriate hyperparameters.

## Evaluation

To evaluate my model on ImageNet, run:

```eval
python eval.py --model-file mymodel.pth --benchmark imagenet
```

> 📋Describe how to evaluate the trained models on benchmarks reported in the paper, give commands that produce the results (section below).

## Pre-trained Models

You can download pretrained models here:

- [My awesome model](https://drive.google.com/mymodel.pth) trained on ImageNet using parameters x,y,z. 

> 📋Give a link to where/how the pretrained models can be downloaded and how they were trained (if applicable).  Alternatively you can have an additional column in your results table with a link to the models.

## Results

Our model achieves the following performance on :

### [Image Classification on ImageNet](https://paperswithcode.com/sota/image-classification-on-imagenet)

| Model name         | Top 1 Accuracy  | Top 5 Accuracy |
| ------------------ |---------------- | -------------- |
| My awesome model   |     85%         |      95%       |

> 📋Include a table of results from your paper, and link back to the leaderboard for clarity and context. If your main result is a figure, include that figure and link to the command or notebook to reproduce it. 


## Contributing

> 📋Pick a licence and describe how to contribute to your code repository. 
