# Exercise Action Classification AI

**Project Period:** March 2022 - May 2022

This project focuses on developing an exercise action recognition algorithm using sensor data collected from 3-axis accelerometers and 3-axis gyroscopes. The goal is to classify 61 different exercise movements based on time-series sensor readings.

**Dacon Competition Link:** [https://dacon.io/competitions/official/235689/overview/description](https://dacon.io/competitions/official/235689/overview/description)


***Data Description***
Sensor data was collected for 3125 unique IDs, with measurements recorded every 0.02 seconds for 600 time steps, totaling 12 seconds of exercise measurement per ID.
The dataset contains a total of 61 distinct exercise action labels.


### 0\. Data Overview

-----

  * `train_features.csv`: Contains sensor data for each ID across 600 time steps.

      * Size: 3125 IDs x 600 time = 1,875,000 observations.
      * Variables (8): `id`, `time`, `acc_x`, `acc_y`, `acc_z` (accelerometer data), `gy_x`, `gy_y`, `gy_z` (gyroscope data).

  * `train_labels.csv`: Provides the corresponding exercise labels for each `id`.

      * Size: 3125 observations.
      * Variables (3): `id`, `label` (numerical label 0-60), `label_desc` (text description of the label).

  * `test_features.csv`: Sensor data for test IDs.

      * Size: 782 IDs x 600 time = 469,200 observations.
      * Variables (8): Same as `train_features.csv`.

  * `sample_submission.csv`: The required format for submission.


### 1\. Exploratory Data Analysis (EDA)

-----

Through visualization, we gained insights into the dataset's characteristics:

  * **Label Distribution**: Counted the number of IDs per label. It was observed that the data is *highly imbalanced*, with the 'Non Exercise' label having an overwhelmingly high count compared to other exercise labels.
  * **Time-Series Plots & Correlation Heatmaps**: Plotted time-series data for accelerometer (acc) and gyroscope (gy) for selected IDs (e.g., 5-6 IDs per label) and generated correlation heatmaps. This helped us:
      * Apprehend the unique trends and patterns in sensor readings for each exercise.
      * Determine whether there is a significant correlation between accelerometer and gyroscope measurements.
      * For example, we could visualize how `acc_x` movement might be significantly higher than other accelerometer axes for a specific exercise like 'Band Pull-Down Row', revealing its characteristic features.


### 2\. Data Preprocessing

-----

To prepare the sensor data for the deep learning model, we performed the following steps:

#### 2-1. Feature Engineering (Derived Variables)

  * `acc`: Calculated the magnitude of the acceleration vector: $\sqrt{acc_x^2 + acc_y^2 + acc_z^2}$
  * `gy`: Calculated the magnitude of the gyroscope vector: $\sqrt{gy_x^2 + gy_y^2 + gy_z^2}$

#### 2-2. Scaling

  * **Standard Scaling**: Applied standard scaling to all numerical features, transforming them to have a mean of 0 and a variance of 1. This helps in stabilizing the training of neural networks: $\frac{x-\mu}{\sigma}$

#### 2-3. Reshaping

The data was reshaped to fit the input requirements of a 1D Convolutional Neural Network. Each ID's 600 time steps of sensor data were treated as a single sequence.

  * `X` (Explanatory Variables):
      * Original `train_features`: (1,875,000 observations, 8 variables)
      * Reshaped to: (3125 IDs, 600 time steps, 8 features) – This forms a 3D input tensor where dimensions represent `[Batchsize, Width (time steps), Channels (features)]`.
  * `y` (Target Variable):
      * Original `train_labels`: (3125 observations, 3 variables)
      * Reshaped to: (3125 IDs, 61 classes) – Transformed into a one-hot encoded representation for the 61 exercise labels.
  * `X_test`:
      * Original `test_features`: (469,200 observations, 8 variables)
      * Reshaped to: (782 IDs, 600 time steps, 8 features)


### 3\. Model Architecture & Training

-----

We explored different 1D Convolutional Neural Network (CNN) architectures, which are well-suited for time-series data classification.

#### Model Structures:

  * **Model 1 Structure:**
    A sequential model consisting of repeated blocks of `Conv1D`, `Batch Normalization`, `MaxPooling1D`, and `Dropout` layers, followed by `Flatten`, `Dense`, and `Batch Normalization`/`Activation` layers.

  * **Model 2 Structure** (Preferred Model):
    This refined model structure incorporates:

      * Repeated blocks of `Conv1D`, `Batch Normalization`, and `Dropout` layers.
      * Followed by a `GlobalAveragePooling1D` layer (which averages across the time dimension).
      * Then `Dense` layers with `Activation` for classification.

#### Training Strategy:

  * Stratified K-Fold Cross-Validation: Utilized Stratified K-Fold (K=5) cross-validation. This is crucial for imbalanced datasets, as it ensures that each fold maintains the same proportion of observations for each class as the original dataset.
  * Epochs: 50
  * Batch Size: 64
  * Early Stopping: Employed Early Stopping to prevent overfitting. Training ceased if the validation loss did not improve for 8 consecutive epochs (`patience=8`).

#### Convolution Layer Hyperparameters:

Based on PyTorch's `Conv1d` (conceptually similar for TensorFlow/Keras):

  * `kernel_size`: 60 (meaning each convolution operation considers 60 time steps at a time)
  * `filters`: 256, 128, 64 (decreasing number of output filters in successive layers)
  * `strides`: 3 (the kernel moves 3 time steps at a time)
  * `activation`: ReLU (Rectified Linear Unit)

#### Output Layer & Compilation:

  * Output Layer (Dense): 61 units (corresponding to 61 exercise classes).
  * Activation: Softmax (for multi-class probability distribution).
  * Optimizer: Adam.
  * Learning Rate: 0.001.
  * Loss Function: Categorical Cross-Entropy (for multi-class classification).
  * Metrics: Accuracy.


### 4\. Prediction

-----

After the model was trained and validated, predictions were generated on the `test_features.csv` dataset and prepared for submission.


### (+) Weaknesses & Future Considerations in 1D CNN Model

-----

Despite the successful implementation, certain challenges and areas for improvement were identified:

  * **Training Time:** The models, particularly with complex layer configurations, required a substantial amount of time to train. Further hyperparameter tuning and model simplification might be necessary.
  * **Model Interpretability:** As with many deep learning models, the 1D CNN offers limited interpretability. It's challenging to explain *how* the model classifies specific actions based on sensor inputs.

#### Open Questions & Future Directions:

  * **Imbalanced Data Handling:** While `Stratified K-Fold` was used, the severe class imbalance, particularly the large 'Non Exercise' category, remains a significant challenge. We observed that applying data augmentation sometimes led to lower performance, indicating that traditional augmentation might not be suitable or needs careful application for such highly imbalanced time-series data. How can we effectively solve this problem?
  * **Hyperparameter Optimization:** How to systematically determine the optimal number of layers and hyperparameters (e.g., filter sizes, number of filters, strides) for time-series CNNs?
  * **Alternative Architectures:** Exploring other time-series specific models, such as *CNN-LSTM hybrid structures*, which can capture both local features (CNN) and long-term dependencies (LSTM), or even pure *Recurrent Neural Networks (RNNs)*.
  * **Functional Data Analysis (FDA):** Investigating the applicability of Functional Data Analysis techniques, which treat each time series as a single function, to potentially extract more meaningful features or model the data differently.