---
title: "Spectroscopy"
permalink: /projects/spectroscopy/
layout: single
---

Objective: Fuse near-infrared spectroscopy signal measurements with categorical descriptors on a dataset obtained from household plastic in a group of 4 people
            
## Dataset description
    - Size: 373 rows, 998 columns
    - Range of signals: ~700nm to ~1000nm
    - Numerical columns: "spectrum_k", "sample_raw_k", "wr_raw_k" are 331 columns each
    - Categorical columns: Color & Transparency
    - Target variable: 1: PET, 2: HDPE, 3: PVC, 4: LDPE, 5: PP, 6: PS, 7: Other, 8: Unknown

    The column "spectrum_k" are the columns of interest for us, since it is obtained by (spectrum_k = sample_raw_k / wr_raw_k), 
    essentially dividing the raw signal by the white reference. Hence our total columns are 331 + 2 categorical columns to predict the target 
    variable consisting of 7 different plastic types. We discarded the 8th plastic type, since it was unknown and it was only 10 samples.
                
    Contributions:
    - Preprocessing: Oversampling -> Baseline Correction -> Normalization -> Savitzy Golay
    - High-level fusion: Bayesian Consensus
    - Mid-level fusion: PCA + Models (XGBoost, AdaBoost, Multilayer Perceptron (MLP), Random Forest (RF)) 

## Preprocessing

Oversampling was perfoming, this there was a class imbalance in the target variable. BorderlineSMOTE was used to balance 
the class imbalance. It uses the basic implementation of SMOTE and improves the problem, where the synthetic samples from
SMOTE are too similar to the existing minority samples. Hence, BorderlineSMOTE generates new samples that are near the borderline between the minority and majority class majority classes. It is easier view a visualization from the imbalanced-learn docs, where the 3 different colors/classes are better oversampled with BorderlineSMOTE.

Now with BorderlineSMOTE, the first image below represents the imbalanced classes, where the second image represents the oversampled classes.

<p align="center">
  <img src="/assets/images/class_imb.png" width="45%" />
  <img src="/assets/images/class_bal.png" width="45%" />
</p>