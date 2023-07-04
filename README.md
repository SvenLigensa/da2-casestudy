# DA2 Case Study: Tree Height Estimation

## Folder Structure
- `0_Preprocessing`: Steps to transform the raw data into data we can use to train our models.
- `1_DataExploration`: Making ourselves familiar with the data by plotting images and masks and computing statistics of the features
- `2_FeatureEngineering`: Transform the data to hopefully train better models
- `3_ModelTraining`: Training of different models and model architectures
- `4_ModelValidation`: Validating the models
- `5_ModelDeployment`: Applying models to new incoming data
- `data`: Data used to train the models
- `models`: Files storing the models parameters

## ML Workflow

### 1. Understand
- Regression Task: Estimate height of trees
- Given data: *Satellite imagery*
    - 10 bands (each `int16`): B2, B3, B4, B5, B6, B7, B8, B8A, B11, B12
    - 1px = 20m
    - Sparse labels

### 2. Collect
- Already done

### 3. Merge & Clean
- Extract training instances from the raw satellite data and the labels (`0_0_Preprocessing`)
    - 38863 instances (90% training: 34976, 10% test: 3887)

### 4. Explore
- Visualize the RGB channels of the images (`1_0_PlotImages`)
- Visualize the distribution of the labels for the images (`1_1_PlotMasks`)
- Compute statistics on all bands visualize them via boxplots (`1_2_ComputeStatistics`)
- Visualize the cumulative distribution function of the labels for training and test data (`1_2_ComputeStatistics`)
- Visualize the distribution of the bands via histograms (`1_2_ComputeStatistics`)

### 4.5. Feature Engineering
- Necessity to get creative because of very sparse labels

1. Add 12 indices found in a Remote Sensing paper (https://www.mdpi.com/1789042) (`2_0_Indices`)
2. Evaluate feature importances via a Random Forest trained on all features (`2_1_FeatureImportances`)
3. Propagating label to surrounding pixels to increase the number of training instances
4. Propagating labels to surrounding pixels and weighting them according to a Gaussian filter to combat the potential label shift of up to 2px
5. Weighting the instances proportionally to their label -> Higher trees are picket with higher probability than the smaller ones -> Combat right skewed distribution
    - Not implemented due to time constraints

- Effectiveness evaluated via standardized XGBoost model
    - Only indices yielded improvement

### 5. Models
- Set aside 10% of the data as the test set to evaluate the performance of the models
- Train various model architectures on different datasets
    - Multiple Linear Regression
    - K-Nearest Neighbors Regressor
    - Decision Tree
    - Random Forest
    - Extra Trees
    - XGBoost
    - MLP
    - CNN
        - Model was made more complex when it underfitted, and less complex or regularized when it overfitted
        - Progress was monitored using TensorBoard
- The best-performing models were chosen by a grid search over a reasonable set of hyperparameters which was iteratively refined
    - Using Cross Validation with 5 or 3 folds

### 6. Validate
- Compare the different models and model architectures
    - MAE
- Visualize the results with graphics
    - Label vs. Prediction

### 7. Deploy
- Apply the model to new satellite images to predict the tree heights
- Grahic: RGB Image vs. Predictions of the best model on the whole image

### 8. Monitor
- Not necessary
