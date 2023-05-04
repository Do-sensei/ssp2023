# DEEE725 Speech Signal Processing Lab
- ***! Will continue to update... !***

## 2023 Spring, Kyungpook National University

- Department: **Artificial Intelligence**
- ID number: **2021420905**
- Date: `2023-05-04`

# Project 1 Isolated digit recognition in noisy environments

- Original [code](https://github.com/gjang7/ssp2023/blob/main/proj1/proj1_nidr.ipynb) is by https://github.com/gjang7/ssp2023/blob/main/proj1/proj1_nidr.ipynb
## Overview
- Attached *'Project 1'* jupyter notebook: [`proj1_nidr_do.ipynb`](proj1_nidr_do.ipynb)

- Additional Tookit
    - `pydub`: [source](https://github.com/jiaaro/pydub) by https://github.com/jiaaro/pydub
        - There is ```!pip3 install pydub``` command in the code

### Dataset
- It will be download dataset from checking `LMS of KNU`
    - After downloading, change folder name from `org` to `clean` both in `./segmented-val/` and `./unsegmented-test/`
1. Train Dataset 
    - `./segmented-train/`
    - *Six person* data in train dataset
    - **Segmented** data
2. Valid Dataset
    - `./segmented-val/`
    - *Two person* data in valid dataset
    - **Segmented** data
3. Test Dataset
    - `./unsegmented-test/`
    - *One person* data in test dataset
    - **Unsegmented** data
### Model
- **GMM-HMM** (Gaussian Mixture Model Hidden Markov Model)

### Experiment process

1. Training **GMM-HMM** model using the *Train dataset*
2. Validated **GMM-HMM** model using the *validate dataset* and Modified the *hyperparameters*
3. Segmenting *Test dataset* from using **EPD** (End-Point Detection) with **Wiener Filter**
    - When running [code](proj1_nidr_do.ipynb), saved the segmented *Test dataset* in `./out/`
4. Testing **GMM-HMM** model using from segmented *Test dataset* in `./out/`

### Baseline

#### Hyperparameters

1. *The number of MFCC* (feature dimension)
    - `num_mfcc`: **6**
2. *Parameters needed to train GMM-HMM*
    - `m_num_of_HMMstates`: **3** (The number of states)
    - `m_num_of_mixtures`: **2** (The number of mixtures for each hidden state)
    - `m_n_iter`: **10** (The number of iteration)

#### Baseline's Result
1. Accuracy **Clean Data** between Train and Valid dataset

| Data | Train | Valid |
|:------:|:-----:|:-----:|
| `base` | 46.6 | 40.5 |

2. Accuracy **Noise Data** of Valid dataset

| Noise | `nbnSNR10` | `nbnSNR0`| `nbnSNR-10`| `wbnSNR10`| `nbnSNR0`| `nbnSNR-10`|
|:------:|:----:|:----:|:----:|:----:|:----:|:----:|
| `base` | 50.0 | 28.0 | 12.5 | 25.0 | 18.0 | 10.0 |

3. Accuracy **Noise Data** of Test dataset

| Noise | `nbnSNR10` | `nbnSNR0`| `nbnSNR-10`| `wbnSNR10`| `nbnSNR0`| `nbnSNR-10`|
|:------:|:----:|:----:|:----:|:----:|:----:|:----:|
| `base` | 46.0 | 43.0 | 31.5 | 42.0 | 37.0 | 16.0 |
