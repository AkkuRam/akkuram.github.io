---
title: "Spectroscopy"
permalink: /projects/spectroscopy/
layout: single
classes: wide
---
            
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

These images represent the preprocessing steps of the signal. For 7 plastic types there are many samples, hence there are multiple signal lines. In addition, once the signals are preprocessed, if for each plastic type the samples are averaged, it should represent a distinct signal for each plastic type, which makes it identifiable which plastic type is which signal.

Referring to the images below, the first image represents the signals after being oversampled. There is clear background noise, therefore baseline correction is applied to isolate the peaks and flatten out the noise by using a quadratic polynomial (image 2). Hereafter, normalization is applied to bring them in a standard scale (image 3). Then the final step is to apply a Savitzky-Golay filter, which mainly smoothens out the signal (image 4).

<p align="center">
  <img src="/assets/images/signal_1.png" width="45%" />
  <img src="/assets/images/signal_2.png" width="45%" />
</p>

<p align="center">
  <img src="/assets/images/signal_3.png" width="45%" />
  <img src="/assets/images/signal_4.png" width="45%" />
</p>

## Mid-level fusion

- Extracting features using PCA + Categorical features

From the previous preprocessing steps, now the data is ready to use for the models. PCA is applied to reduce the dimensionality, since we are dealing with ~300 columns, where the PCA plot of 2 components below depicts a reasonable separation of the 7 classes. Moreover, from literature a threshold of keeping 90% of the variance was used which selected top 5 components in our case. The elbow method was not used, since this chose < 5 components, and the results (i.e. accuracy, precision, recall, f1score) were suboptimal.

<p align="center">
  <img src="/assets/images/pca.png" width="45%" />
  <img src="/assets/images/pca_thresholded.png" width="45%" />
</p>

With the following selections for mid-level fusion, it lead to the following results for the below metrics. Overall, the best performing models in terms of accuracy was XGBoost (78%), but in terms of other metrics, but XGBoost and RF were stable.