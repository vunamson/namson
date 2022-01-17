# WSDM 2022 CUP - Cross-Market Recommendation - Starter Kit 
This repository provides a sample code for training a simple Generalized Matrix Factorization (GMF) model over several markets. We provide loading data from zero to a few source markets to augment the target market data, which can help the recommendation performance in the target market. Please read through this note and take a look at `train_baseline.py` and `getting_started.ipynb` for understanding our expectations.

We highly recommend following the structure of our sample code for your own model design, as we ask every team to submit their code along with their submission and share the implementation with the organizers. In the case we are not able to reproduce your results, your submission will be removed from our leaderboard. Please reach out to us if you encounter any problem with using this code or any other questions / feedback. 

For further information please refer to the competition website: [WSDM_2022_CUP](https://xmrec.github.io/wsdmcup/)

## Requirements:
We use conda for our experimentations. You can use `environment.yml` to create the environment (use `conda env create -f environment.yml`) or install the below list of requirements on your own environment. 

- python 3.7 
- pandas & numpy (pandas-1.3.3, numpy-1.21.2)
- torch==1.9.1
- [pytrec_eval](https://github.com/cvangysel/pytrec_eval)




## Train the baseline GMF++ model:
`train_baseline.py` is the script for training our simple GMF++ model that is taking one target market and zero to a few source markets for augmenting with the target market. We implemented our dataloader such that it loads all the data and samples equally from each market in the training phase. You can use ConcatDataset from `torch.utils.data` to concatenate your torch Datasets. 


Here is a sample train script using two source markets:

    python train_baseline.py --tgt_market t1 --src_markets s1-s2 --tgt_market_valid DATA/t1/valid_run.tsv --tgt_market_test DATA/t1/test_run.tsv --exp_name toytest --num_epoch 5 --cuda
    
Here is a sample train script using zero source market (only train on the target data):

    python train_baseline.py --tgt_market t1 --src_markets none --tgt_market_valid DATA/t1/valid_run.tsv --tgt_market_test DATA/t1/test_run.tsv --exp_name toytest --num_epoch 5 --cuda


After training your model, the scripts prints the directories of model and index checkpoints as well as the run files for the validation and test data as below. You can load the model for other usage and evaluate the validation run file. See the notebook `getting_started.ipynb` for a sample code on these. 

    Model is trained! and saved at:
    --model: checkpoints/t1_s1-s2_toytest.model
    --id_bank: checkpoints/t1_s1-s2_toytest.pickle
    Run output files:
    --validation: valid_t1_s1-s2_toytest.tsv
    --test: test_t1_s1-s2_toytest.tsv
    
You will need to upload the test run output file (.tsv file format) for both target markets to Codalab for our evaluation and leaderboard entry. This output file contains ranked items for each user with their score. Our final evaluation metric is based on nDCG@10 on both target markets.   



## DATA
The `DATA` folder in this repository contains the two target markets data (train ratings, validation run and qrel, and test run) that we conduct our evaluation on them and three source markets that you can use as desired to augment with the target markets. It is ultimately your choice on how to use the provided data (or any other additional data, if you will) to improve the recommendation performance on target markets. 

