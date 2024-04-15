# Anomalous Sound Detection
## DCASE 2024 Challenge Task 2 and DCASE 2023 Challenge Task 2 Baseline Auto Encoder: dcase2023\_task2\_baseline\_ae

This is an autoencoder-based baseline for the [DCASE2024 Challenge Task 2 (DCASE2024T2)](https://dcase.community/challenge2024/) and the [DCASE2023 Challenge Task 2 (DCASE2023T2)](https://dcase.community/challenge2023/).

This source code is an example implementation of the baseline Auto Encoder of DCASE2024T2 and DCASE2023T2: First-Shot Unsupervised Anomalous Sound Detection for Machine Condition Monitoring.
This baseline implementation is based on the previous baseline, dcase2022\_baseline\_ae. The model parameter settings of this baseline AE are almost equivalent to those of the dcase2022\_task2\_baseline\_ae.

Differences between the previous dcase2022\_baseline\_ae and this version are as follows:

- The dcase2022\_baseline\_ae was implemented with Keras; however, this version is written in PyTorch.
- Data folder structure is updated to support DCASE2024T2 and DCASE2023T2 data sets.
- The system uses the MSE loss as a loss function for training, but for testing, two score functions depend on the testing modes (i.e., MSE for the Simple Autoencoder mode and Mahalanobis distance for the Selective Mahalanobis mode).
  
## Description

This system consists of three main scripts (01_train.sh, 02a_test.sh, and 02b_test.sh) with some helper scripts for DCASE2024T2 (For DCASE2023T2, see [README_legacy](README_legacy.md)):

- Helper scripts for DCASE2024T2
  - data\_download\_2024dev.sh
    - "Development dataset":
      - This script downloads development data files and puts them into "data/dcase2024t2/dev\_data/raw/train/" and "data/dcase2024t2/dev\_data/raw/test/". **Newly added!!**

- 01_train_2024t2.sh
  - "Development" mode:
    - This script trains a model for each machine type for each section ID by using the directory `data/dcase2024t2/dev_data/raw/<machine_type>/train/<section_id>`.  **Newly added!!**

- 02a_test_2024t2.sh (Use MSE as a score function for the Simple Autoencoder mode)
  - "Development" mode:
    - This script makes a CSV file for each section, including the anomaly scores for each WAV file in the directories `data/dcase2024t2/dev_data/raw/<machine_type>/test/`. **Newly added!!**
    - The CSV files will be stored in the directory `results/`.
    - It also makes a csv file including AUC, pAUC, precision, recall, and F1-score for each section.
  
- 02b_test_2024t2.sh (Use Mahalanobis distance as a score function for the Selective Mahalanobis mode)
  - "Development" mode:
    - This script makes a CSV file for each section, including the anomaly scores for each wav file in the directories `data/dcase2024t2/dev_data/raw/<machine_type>/test/`. **Newly added!!**
    - The CSV files will be stored in the directory `results/`.
    - It also makes a csv file including AUC, pAUC, precision, recall, and F1-score for each section.
  
- 03_summarize_results.sh
  - This script summarizes results into a CSV file.

C.f., for DCASE2023T2, see [README_legacy](README_legacy.md).


## Usage

### 1. Clone repository
   
Clone this repository from GitHub.

### 2. Download datasets

We will launch the datasets in three stages. Therefore, please download the datasets in each stage:

  + DCASE 2024 Challenge Task 2
    + "Development Dataset" **New! (2024/04/01)**
      + Download "dev\_data_<machine_type>.zip" from 
[https://zenodo.org/records/10902294](https://zenodo.org/records/10902294).

  + For DCASE 2023 Challenge Task 2
	(C.f., for DCASE2023T2, see [README_legacy](README_legacy.md))
    + "Development Dataset"
      + Download "dev\_data_<machine_type>.zip" from [https://zenodo.org/record/7882613](https://zenodo.org/record/7882613).
    + "Additional Training Dataset", i.e., the evaluation dataset for training 
      + Download "eval\_data_<machine_type>_train.zip" from [https://zenodo.org/record/7830345](https://zenodo.org/record/7830345).
    + "Evaluation Dataset", i.e., the evaluation dataset for test
      + Download "eval\_data_<machine_type>_test.zip" from [https://zenodo.org/record/7860847](https://zenodo.org/record/7860847).


### 3. Unzip the downloaded files and make the directory structure as follows:
   
  + dcase2023\_task2\_baseline\_ae
    + data/dcase2024t2/dev\_data/raw/
      + fan/
        + train (only normal clips)
          + section\_00\_source\_train\_normal\_0000\_.wav
          + ...
          + section\_00\_source\_train\_normal\_0989\_.wav
          + section\_00\_target\_train\_normal\_0000\_.wav
          + ...
          + section\_00\_target\_train\_normal\_0009\_.wav
        + test/
          + section\_00\_source\_test\_normal\_0000\_.wav
          + ...
          + section\_00\_source\_test\_normal\_0049\_.wav
          + section\_00\_source\_test\_anomaly\_0000\_.wav
          + ...
          + section\_00\_source\_test\_anomaly\_0049\_.wav
          + section\_00\_target\_test\_normal\_0000\_.wav 
          + ...
          + section\_00\_target\_test\_normal\_0049\_.wav 
          + section\_00\_target\_test\_anomaly\_0000\_.wav 
          + ...
          + section\_00\_target\_test\_anomaly\_0049\_.wav
        + attributes\_00.csv (attributes CSV for section 00)
     + gearbox/ (The other machine types have the same directory structure as fan.)
   + data/dcase2024t2/eval\_data/raw/

### 4. Change parameters

You can change parameters for feature extraction and model definition by editing `baseline.yaml`.
Note that if values are specified in the command line option, it will overwrite the parameter settings in `baseline.yaml`.

### 4.1. Enable Auto-download dataset

If you haven't yet downloaded the dataset yourself nor you have not run the download script (example, `data_download_2024dev.sh`) then you may want to use the auto download.
To enable the auto-downloading, set the parameter `--is_auto_download` (default: `False`) `True` in `baseline.yaml`. If `--is_auto_download` is `True`, then auto-download is executed.

### 5. Run the training script (for the development dataset)

Run the training script `01_train_2024t2.sh`. Use the option -d for the development dataset `data/dcase2024t2/dev_data/<machine_type>/raw/train/`.
`01_train_2024t2.sh` differs from `01_train_2024t2.sh` only in dataset;

```dotnetcli
# using DCASE2024 Task 2 Datasets
$ 01_train_2024t2.sh -d
```
The two operating modes of this baseline implementation, the simple Autoencoder, and the Selective Mahalanobis AE modes, share the common training process. By running the script `01_train_2024t2.sh`, all the model parameters for the simple Autoencoder and the selective Mahalanobis AE will be trained at the same time.
After the parameter update of the Autoencoder at the last epoch specified by either the yaml file or the command line option, the covariance matrixes for the Mahalanobis distance calculation will be set.

### 6. Run the test script (for the development dataset)

### 6.1. Testing with the Simple Autoencoder mode
Run the test script `02a_test_2024t2.sh`. Use the option `-d` for the development dataset `data/dcase2024t2/dev_data/<machine_type>/raw/test/`.
`02a_test_2024t2.sh` differs from `02a_test_2023t2.sh` only in dataset;

```dotnetcli
# using DCASE2024 Task 2 Datasets
$ 02a_test_2024t2.sh -d
```

The `02a_test_2024t2.sh` options are the same as those for `01_train_2024t2.sh`. `02a_test_2024t2.sh` calculates an anomaly score for each wav file in the directories `data/dcase2024t2/dev_data/raw/<machine_type>/test/` or `data/dcase2024t2/dev_data/raw/<machine_type>/source_test/` and `data/dcase2024t2/dev_data/raw/<machine_type>/target_test/`.
A CSV file for each section, including the anomaly scores, will be stored in the directory `results/`. If the mode is "development", the script also outputs another CSV file, including AUC, pAUC, precision, recall, and F1-score for each section.


### 6.2. Testing with the Selective Mahalanobis mode
Run the test script `02b_test_2024t2.sh`. Use the option `-d` for the development dataset `data/dcase2024t2/dev_data/<machine_type>/raw/test/`.
`02b_test_2024t2.sh` differs from `02b_test_2023t2.sh` only in dataset;

```dotnetcli
# using DCASE2024 Task 2 Datasets
$ 02b_test_2024t2.sh -d
```

The `02b_test_2024t2.sh` options are the same as those for `01_train_2024t2.sh`. `02b_test_2024t2.sh` calculates an anomaly score for each wav file in the directories `data/dcase2024t2/dev_data/raw/<machine_type>/test/` or `data/dcase2024t2/dev_data/raw/<machine_type>/source_test/` and `data/dcase2024t2/dev_data/raw/<machine_type>/target_test/`.
A CSV file for each section, including the anomaly scores, will be stored in the directory `results/`. If the mode is "development", the script also outputs another CSV file, including AUC, pAUC, precision, recall, and F1-score for each section.


### 7. Check results

You can check the anomaly scores in the CSV files `anomaly_score_<machine_type>_section_<section_index>_test.csv` in the directory `results/`.
Each anomaly score corresponds to a wav file in the directories `data/dcase2024t2/dev_data/<machine_type>/test/`.

`anomaly_score_<machine_type>_section_00_test.csv`

```dotnetcli
section_00_source_test_normal_0000_car_A2_spd_28V_mic_1_noise_1.wav,0.3084583878517151
section_00_source_test_normal_0001_car_A2_spd_28V_mic_1_noise_1.wav,0.31289517879486084
section_00_source_test_normal_0002_car_A2_spd_28V_mic_1_noise_1.wav,0.4160425364971161
section_00_source_test_normal_0003_car_A2_spd_28V_mic_1_noise_1.wav,0.25631701946258545

```

Also, anomaly detection results based on the corresponding threshold can be checked in the CSV files `decision_result_<machine_type>_section_<section_index>_test.csv`:

`decision_result_<machine_type>_section_<section_index>_test.csv`

```dotnetcli
section_00_source_test_normal_0000_car_A2_spd_28V_mic_1_noise_1.wav,0
section_00_source_test_normal_0001_car_A2_spd_28V_mic_1_noise_1.wav,0
section_00_source_test_normal_0002_car_A2_spd_28V_mic_1_noise_1.wav,0
section_00_source_test_normal_0003_car_A2_spd_28V_mic_1_noise_1.wav,0
...
```

In addition, you can check performance indicators such as AUC, pAUC, precision, recall, and F1-score:

`result.csv`

```dotnetcli
section, AUC (source), AUC (target), pAUC, pAUC (source), pAUC (target), precision (source), precision (target), recall (source), recall (target), F1 score (source), F1 score (target)
00,0.88,0.5078,0.5063157894736842,0.5536842105263158,0.4926315789473684,0.0,0.0,0.0,0.0,0.0,0.
arithmetic mean,00,0.88,0.5078,0.5063157894736842,0.5536842105263158,0.4926315789473684,0.0,0.0,0.0,0.0,0.0,0.
harmonic mean,00,0.88,0.5078,0.5063157894736842,0.5536842105263158,0.4926315789473684,0.0,0.0,0.0,0.0,0.0,0.
```

### 8. Run training script for the additional training dataset (after May 15, 2024)

After the additional training dataset is launched, download and unzip it. Move it to `data/dcase2024t2/eval_data/raw/<machine_type>/train/`. Run the training script `01_train_2024t2.sh` with the option `-e`.

```dotnetcli
$ 01_train_2024t2.sh -e
```

Models are trained by using the additional training dataset `data/dcase2024t2/raw/eval_data/<machine_type>/train/`.

### 9. Run the test script for the evaluation dataset (after June 1, 2024)

### 9.1. Testing with the Simple Autoencoder mode

After the evaluation dataset for the test is launched, download and unzip it. Move it to `data/dcase2024t2/eval_data/raw/<machine_type>/test/`. Run the test script `02a_test_2024t2.sh` with the option `-e`.

```dotnetcli
$ 02a_test_2024t2.sh -e
```

Anomaly scores are calculated using the evaluation dataset, i.e., `data/dcase2024t2/eval_data/raw/<machine_type>/test/`. The anomaly scores are stored as CSV files in the directory `results/`. You can submit the CSV files for the challenge. From the submitted CSV files, we will calculate AUC, pAUC, and your ranking.

### 9.2. Testing with the Selective Mahalanobis mode

After the evaluation dataset for the test is launched, download and unzip it. Move it to `data/dcase2024t2/eval_data/raw/<machine_type>/test/`. Run the `02b_test_2024t2.sh` test script with the option `-e`.

```dotnetcli
$ 02b_test_2024t2.sh -e
```

Anomaly scores are calculated using the evaluation dataset, i.e., `data/dcase2024t2/eval_data/raw/<machine_type>/test/`. The anomaly scores are stored as CSV files in the directory `results/`. You can submit the CSV files for the challenge. From the submitted CSV files, we will calculate AUC, pAUC, and your ranking.

### 10. Summarize results

After the executed `02a_test_2024t2.sh`, `02b_test_2024t2.sh`, or both. Run the summarize script `03_summarize_results.sh` with the option `DCASE2024T2 -d` or `DCASE2024T2 -e`.

```dotnetcli
# Summarize development dataset 2024
$ 03_summarize_results.sh DCASE2024T2 -d

# Summarize the evaluation dataset 2024
$ 03_summarize_results.sh DCASE2024T2 -e
```

After the summary, the results are exported in CSV format to `results/dev_data/baseline/summarize/DCASE2024T2` or `results/eval_data/baseline/summarize/DCASE2024T2`.

If you want to change, summarize results directory or export directory, edit `03_summarize_results.sh`.

## Legacy support

This version takes the legacy datasets provided in DCASE2020 task2, DCASE2021 task2, DCASE2022 task2, and DCASE2023 task2 dataset for inputs.
The Legacy support scripts are similar to the main scripts. These are in `tools` directory.

[learn more](README_legacy.md)

## Dependency

We developed and tested the source code on Ubuntu 18.04.6 LTS.

### Software package

- Python == 3.10.8
- cuda == 11.6
- libsndfile1

### Python packages

- Pytorch == 1.13.1
- torchvision == 0.14.1
- numpy == 1.22.3
- pyYAML == 6.0
- scipy == 1.10.1
- librosa == 0.9.2
- matplotlib == 3.7.0
- tqdm == 4.63
- seaborn == 0.12.2
- fasteners == 0.18

## Change Log

### [3.0.0](https://github.com/nttcslab/dcase2023_task2_baseline_ae/releases/tag/v3.0.0)

#### Added

- provides support for the datasets used in DCASE2024.

### [2.0.1](https://github.com/nttcslab/dcase2023_task2_baseline_ae/releases/tag/v2.0.1)

#### Fixed

- Fix anomaly score distribution.
- Decision threshold has changed, but AUC, pAUC, etc. have not changed.

### [2.0.0](https://github.com/nttcslab/dcase2023_task2_baseline_ae/releases/tag/v2.0.0)

#### Added

- provides support for the legacy datasets used in DCASE2020, 2021, 2022, and 2023.


## Citation

If you use this system, please cite all the following four papers:

+ Kota Dohi, Keisuke Imoto, Noboru Harada, Daisuke Niizumi, Yuma Koizumi, Tomoya Nishida, Harsh Purohit, Ryo Tanabe, Takashi Endo, and Yohei Kawaguchi, "Description and Discussion on DCASE 2023 Challenge Task 2: First-Shot Unsupervised Anomalous Sound Detection for Machine Condition Monitoring," in arXiv-eprints: 2305.07828, 2023. [URL](https://arxiv.org/abs/2305.07828)
+ Kota Dohi, Keisuke Imoto, Noboru Harada, Daisuke Niizumi, Yuma Koizumi, Tomoya Nishida, Harsh Purohit, Takashi Endo, Masaaki Yamamoto, and Yohei Kawaguchi, "Description and discussion on DCASE 2022 challenge task 2: unsupervised anomalous sound detection for machine condition monitoring applying domain generalization techniques," in Proc. DCASE 2022 Workshop, 2022. [URL](https://dcase.community/documents/workshop2022/proceedings/DCASE2022Workshop_Dohi_63.pdf)
+ Noboru Harada, Daisuke Niizumi, Daiki Takeuchi, Yasunori Ohishi, Masahiro Yasuda, Shoichiro Saito, "ToyADMOS2: Another Dataset of Miniature-Machine Operating Sounds for Anomalous Sound Detection under Domain Shift Conditions," in Proc. DCASE 2022 Workshop, 2022. [URL](https://dcase.community/documents/workshop2021/proceedings/DCASE2021Workshop_Harada_6.pdf)
+ Kota Dohi, Tomoya Nishida, Harsh Purohit, Ryo Tanabe, Takashi Endo, Masaaki Yamamoto, Yuki Nikaido, and Yohei Kawaguchi, "MIMII DG: sound dataset for malfunctioning industrial machine investigation and inspection for domain generalization task," in Proc. DCASE 2022 Workshop, 2022. [URL](https://dcase.community/documents/workshop2022/proceedings/DCASE2022Workshop_Dohi_62.pdf)
+ Noboru Harada, Daisuke Niizumi, Yasunori Ohishi, Daiki Takeuchi and Masahiro Yasuda, "First-Shot Anomaly Sound Detection for Machine Condition Monitoring: A Domain Generalization Baseline," 2023 31st European Signal Processing Conference (EUSIPCO), Helsinki, Finland, 2023, pp. 191-195, doi: 10.23919/EUSIPCO58844.2023.10289721. [URL](https://ieeexplore.ieee.org/document/10289721)



