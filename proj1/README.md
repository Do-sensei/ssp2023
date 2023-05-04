# DEEE725 Speech Signal Processing Lab

## 2023 Spring, Kyungpook National University

- Department: **Artificial Intelligence**
- ID number: **2021420905**
- Date: `2023-05-04`
    - ***! Will continue to update... !***
# Project 1. Isolated digit recognition in noisy environments

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
3. Segmenting *Test dataset* from using **EPD** (The End-Point Detection) with **Wiener Filter**
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
    - *Bakis* method

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

---

## Purpose

- This study's purpose is ***isolated digit recognition in noisy environments*** from continuous speaking digit ('1' to '10') audio data

## Methods

### Dataset

- The dataset is continuous speaking digit ('1' to '10') audio data by speaker

    - The clean data is orginal data
    - The noisy data
        - The type of noise are `nbsSNR` and `wbnSNR`
        - `nbs` (narrowband noise SNR)
            - Narrowband signals are signals that occupy a narrow range of frequencies or that have a small fractional bandwidth [(source by `WIKIPEDIA`)](https://en.wikipedia.org/wiki/Narrowband)
        - `wbn` (wideband noise SNR)
            - The wideband noise is a type noise that covers a borad range of frequncies within the audio sepctrum
        - `SNR` (Signal-to-Noise Ratio)
            - $SNR (dB) = 10 * log_{10} (\frac{<s^2>_T}{<n^2>_T})$
            - There are mixed from {-10, 0, 10} dB to each `nbs` and `wnb`
        - The noise are `nbs-10`, `nbs0`, `nbs10`, `wbn-10`, `wbn0` and `wbn10`

- It is seperated to *Training Dataset*, *Validataion Dataset* and *Test Dataset*


1. Training Dataset 
    - The training dataset is for **training** of model
    - There is the training dataset in `./segmented-train/` with *Six person*'s **Segmented** *clean* data
2. Validation Dataset
    - The validation dataset is for **validation** and **optimizing** of model
    - It is not using at the training
    - There is the validation dataset in `./segmented-val/` with *Two person*'s **Segmented** *clean* and *noisy* data
3. Test Dataset
    - The test dataset is for **test** of model
    - It is not using at the training
    - There is the test dataset in `./unsegmented-test/` with *One person*'s **Unsegmented** *clean* and *noisy* data

### Model

- The model is **GMM-HMM** (Gaussian Mixture Model Hidden Markov Model)
    - It is a combination of two probabilistic models the Gaussian Mixture Model (*GMM*) and the Hidden Markov Model (*HMM*)
    - **GMM-HMM** is widely used in speech recognition and other time-series data analysis task due to its ability to model the time-varying properties of signals
        - *GMM*
            - It is a generative probabilistic model used to represent the distribution of data points in a dataset
        - *HMM*
            - It is a statistical model that represents a system that transitions between a finite number of hidden states over time
    - **GMM-HMM** is trained on a dataset of speech signals to learn the parameters of the GMMs and the HMM transition probabilities


### Data pre-processing

1. Loading the audio files (`.wav`)
2. Extracting *MFCC* (`'extmfcc'`) features
    - *MFCC* (The Mel Frequency Cepstral Coefficients)
        - It is extreacted from the audio signals
3. Segmenting Data using *EPD* (The End-Point Detection, `'split_audio_using_epd'`) with *Wiener filter* (`'time_domain_wiener_filter'`)
    - For *test dataset* which is **Unsegmented** data
    - Before the test, It need to segment the data using *EPD* with *Wiener filter*
    - *EDP*
        - The *EDP* algorithm is used to identify the start and end points of the speech signal within an audio file
        - This helps in removing silence or noise segments before and after the actual speech content
    - *Wiener filter*
        - The *Wiener filter* is a noise reduction technique used to estimate and remove noise from a signal by minimizing the mean square error between the estimated clean signal and the actual clean signal

### Model training

1. Data preparation
    - *MFCC* features are extracted from the training data

2. Model initialization
    - The **GMM-HMM** models for each spoken digit are initializaed using *Bakis* method
    - The *Bakis* method is used to set the initial state transition probabilities and start state probabilities for the HMMs

3. Model training
    - The training data for each spoken digit is fed into the corresponding **GMM-HMM** model
    - interatively updates the model parameters to maximize the likelihood of the training data given the model
4. Model fitting
    - After training, the **GMM-HMM** models are fitted to the training date, which involves estimating the parameters of *GMMs* for each state in the *HMM*


### Prediction Metric

- $\text{accuracy} = \frac{\text{correct predictions}}{\text{total predictions}}$

## Experiments

### Baseline

#### Hyperparameters

1. *The number of MFCC* (feature dimension)
    - `num_mfcc`: **6**
2. *Parameters needed to train GMM-HMM*
    - `m_num_of_HMMstates`: **3** (The number of states)
    - `m_num_of_mixtures`: **2** (The number of mixtures for each hidden state)
    - `m_n_iter`: **10** (The number of iteration)
    - *Bakis* method


## Results

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

