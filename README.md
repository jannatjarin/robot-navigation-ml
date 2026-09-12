# Wall-Following Robot Navigation Using Machine Learning

## Project Overview

This project develops and evaluates machine-learning models for predicting the navigation action of a mobile robot using ultrasonic distance-sensor readings.

The project uses the **Wall-Following Robot Navigation Data** from the UCI Machine Learning Repository. Each observation contains readings from 24 ultrasonic sensors positioned around a SCITOS G5 mobile robot. The target variable represents the movement action performed by the robot.

The main research question is:

> **Which machine-learning model best predicts a mobile robot's navigation action from its ultrasonic distance-sensor readings?**

The project follows a complete classification workflow including data inspection, exploratory analysis, leakage-safe data splitting, baseline comparison, model development, hyperparameter selection, final evaluation, error analysis, and reproducibility documentation.

---

## Dataset

**Dataset:** Wall-Following Robot Navigation Data  
**Source:** UCI Machine Learning Repository  
**UCI Dataset ID:** 194  
**DOI:** 10.24432/C57C8W  
**License:** CC BY 4.0  
**Task:** Multiclass Classification  
**Number of Observations:** 5,456  
**Number of Input Features:** 24 ultrasonic sensor readings  
**Target Variable:** `Class`  
**Missing Values:** None reported and none found during the project audit  

The data were collected from a SCITOS G5 mobile robot performing wall-following navigation in a room. Measurements were recorded sequentially at approximately 9 samples per second while the robot completed four navigation rounds.

The project uses:

```text
data/raw/sensor_readings_24.data
```

The original raw dataset is kept unchanged.

A description of the variables is available in:

```text
data/data_dictionary.md
```

---

## Target Classes

The target variable contains four navigation actions:

- `Move-Forward`
- `Slight-Right-Turn`
- `Sharp-Right-Turn`
- `Slight-Left-Turn`

The dataset is imbalanced, so overall Accuracy is not used alone. Macro-averaged Precision, Recall, and F1 are also considered so that every navigation class contributes equally to the evaluation.

---

## Project Objective

Each row represents one robot state containing 24 ultrasonic distance-sensor readings.

The objective is to predict the correct navigation action associated with that sensor state.

A useful model should:

- perform substantially better than a majority-class baseline;
- classify all four navigation actions effectively;
- achieve strong Macro F1 as well as overall Accuracy;
- generalize to the reserved test observations without using the test set during model selection.

---

## Privacy and Ethical Considerations

The dataset contains robot sensor measurements and navigation-action labels rather than personal or sensitive human information. Therefore, there are no direct personal-privacy concerns.

However, navigation predictions may become safety-relevant if a trained model is used to control a physical robot. The models in this project should therefore be treated as experimental navigation-action predictors rather than complete safety-critical robot controllers.

Real-world deployment would require additional testing under different rooms, obstacles, sensors, hardware conditions, sensor failures, and environmental conditions.

---

## Project Workflow

The notebook follows the workflow below:

1. Problem and dataset description
2. Library imports
3. Dataset loading
4. Initial data inspection
5. Data audit and validation
6. Missing-value and duplicate checks
7. Class-distribution analysis
8. Exploratory data analysis
9. Feature and target preparation
10. Training, validation, and test split
11. Majority-class baseline
12. K-Nearest Neighbours classification
13. Decision Tree classification
14. Random Forest classification
15. Candidate-model comparison
16. Computational-cost comparison
17. Final-model selection
18. Final-model retraining
19. Final test evaluation
20. Confusion matrix and class-wise metrics
21. Error analysis
22. Assumptions and limitations
23. Reproducibility information
24. Final conclusion

---

## Data Audit and Preprocessing

The project checks:

- dataset dimensions;
- feature and target columns;
- data types;
- descriptive statistics;
- missing values;
- duplicate observations;
- valid class labels;
- impossible negative sensor values;
- sensor ranges;
- possible extreme observations;
- class imbalance.

The dataset contains no missing values, so no imputation is required.

No duplicate rows were found, so no duplicate removal is performed.

All 24 sensor features are numerical, so categorical feature encoding is not required.

### Scaling

K-Nearest Neighbours is distance-based and is sensitive to feature scale. Therefore, KNN is implemented using a scikit-learn `Pipeline` containing:

```text
StandardScaler
→
KNeighborsClassifier
```

The scaler is fitted only using training data.

Decision Tree and Random Forest do not require feature scaling, so the original numerical feature values are used for those models.

---

## Data Split Strategy

The dataset consists of sequential robot observations.

Because explicit navigation-round identifiers are not available in the selected data file, the original row order is preserved instead of randomly shuffling the observations.

The dataset is divided into:

```text
Training set:    60%
Validation set:  20%
Test set:        20%
```

Actual split sizes:

```text
Training observations:    3,273
Validation observations:  1,091
Test observations:        1,092
```

The split uses:

```python
shuffle=False
```

This preserves the original sequential ordering.

The validation set is used for model and hyperparameter selection.

The final test set remains untouched until the final model has already been selected.

---

## Baseline

A majority-class baseline is used as the reference model.

The most common training class is:

```text
Move-Forward
```

Approximate validation performance:

```text
Accuracy:  45.19%
Macro F1:  0.156
```

The low Macro F1 demonstrates why Accuracy alone would not provide a complete evaluation for this imbalanced multiclass problem.

---

## Candidate Models

Three machine-learning classifiers are evaluated.

### 1. K-Nearest Neighbours

KNN is included because observations with similar ultrasonic distance patterns may correspond to similar navigation actions.

Feature scaling is performed using `StandardScaler`.

Candidate values of `k` are evaluated using the validation set:

```text
1, 3, 5, 7, 9, 11, 15
```

The selected value is:

```text
k = 3
```

Approximate validation performance:

```text
Accuracy:  83.13%
Macro F1:  0.809
```

---

### 2. Decision Tree

Decision Tree is suitable because navigation decisions may depend on nonlinear threshold relationships among sensor readings.

Candidate maximum depths are evaluated:

```text
3, 5, 7, 10, 15, None
```

The selected configuration is:

```text
max_depth = 7
random_state = 42
```

Approximate validation performance:

```text
Accuracy:  99.08%
Macro F1:  0.976
```

---

### 3. Random Forest

Random Forest combines multiple decision trees and can model nonlinear relationships and interactions among the 24 sensor features.

The project evaluates different maximum depths and numbers of trees.

Candidate maximum depths:

```text
5, 7, 10, 15, None
```

Candidate numbers of trees:

```text
50, 100, 150, 200
```

The selected configuration is:

```text
n_estimators = 100
max_depth = 10
random_state = 42
```

The scikit-learn default value of `max_features="sqrt"` is retained.

Approximate validation performance:

```text
Accuracy:  99.45%
Macro F1:  0.985
```

Random Forest achieves perfect performance on the training observations and slightly lower performance on the validation observations. This indicates some difference between training and unseen validation performance, although validation performance remains very strong.

Feature importance is not used for feature selection. All 24 original sensor features are retained.

---

## Model Comparison

The main validation results are approximately:

| Model | Validation Accuracy | Macro F1 |
|---|---:|---:|
| Majority Baseline | 45.19% | 0.156 |
| K-Nearest Neighbours | 83.13% | 0.809 |
| Decision Tree | 99.08% | 0.976 |
| Random Forest | **99.45%** | **0.985** |

Macro F1 is used as the primary model-selection metric because the class distribution is imbalanced.

Based on validation performance, **Random Forest is selected as the final model**.

The final model and its hyperparameters are selected before final test performance is examined.

---

## Computational Cost

Training and validation prediction times are recorded for:

- K-Nearest Neighbours;
- Decision Tree;
- Random Forest.

Timing measurements are generated during notebook execution using `perf_counter`.

These measurements are treated as approximate computational evidence because execution time can vary depending on the computer, background processes, and software environment.

---

## Final Model

The selected model is:

```text
Random Forest
```

Selected configuration:

```text
n_estimators = 100
max_depth = 10
random_state = 42
max_features = sqrt
```

After model selection is completed, the selected Random Forest configuration is retrained using the complete development dataset:

```text
Training set + Validation set
```

The final test set is then used once for final evaluation.

---

## Final Test Results

Approximate final test performance:

| Metric | Result |
|---|---:|
| Accuracy | 98.53% |
| Macro Precision | 0.984 |
| Macro Recall | 0.984 |
| Macro F1 | 0.984 |

The final model makes approximately:

```text
1,076 correct predictions
16 incorrect predictions
1,092 total test observations
```

The approximate test error rate is:

```text
1.47%
```

The notebook also reports:

- class-wise Precision;
- class-wise Recall;
- class-wise F1;
- class support;
- final confusion matrix.

Exact values should be taken from the executed notebook.

---

## Error Analysis

The remaining test errors are concentrated in a small number of class combinations.

The most common errors are approximately:

```text
Move-Forward → Sharp-Right-Turn       6
Move-Forward → Slight-Right-Turn      6
Slight-Left-Turn → Move-Forward       2
Slight-Left-Turn → Sharp-Right-Turn   2
```

Most errors therefore occur between movement actions that may appear under similar sensor configurations.

The strong test performance should not be interpreted as proof that the model would perform equally well in every physical environment.

---

## Assumptions and Limitations

Important limitations include:

- The data contain sequential observations collected during four robot navigation rounds.
- Explicit round identifiers are not available in the selected dataset file.
- An ordered split is therefore used instead of a true round-based grouped split.
- Consecutive sensor observations may be correlated because measurements were collected at a high sampling rate.
- The data were collected using one robot in a particular experimental environment.
- The navigation classes are imbalanced.
- New rooms, obstacle layouts, robots, sensors, hardware differences, or sensor noise may reduce performance.
- The dataset may not represent every real-world navigation condition.
- Very strong performance on this dataset does not guarantee safe autonomous navigation in an unrestricted physical environment.

---

## Responsible Use

This project demonstrates machine-learning classification of robot navigation actions.

The selected model should not be treated as a complete autonomous-navigation safety system.

Before real-world deployment, additional evaluation would be required for:

- unseen environments;
- different obstacle arrangements;
- different robots;
- sensor noise;
- sensor failures;
- hardware variation;
- unexpected operating conditions;
- real-world safety mechanisms.

---

## Project Structure

```text
robot-navigation-ml/
│
├── data/
│   ├── data_dictionary.md
│   └── raw/
│       └── sensor_readings_24.data
│
├── figures/
│
├── notebooks/
│   └── robot_navigation.ipynb
│
├── presentation/
│
├── report/
│
├── .gitignore
├── README.md
└── requirements.txt
```

### Important Files

`data/raw/sensor_readings_24.data`  
Contains the unchanged raw dataset used by the project.

`data/data_dictionary.md`  
Documents the 24 ultrasonic sensor features and target variable.

`notebooks/robot_navigation.ipynb`  
Contains the complete data-science workflow from data loading through final evaluation.

`requirements.txt`  
Contains the Python package versions required to reproduce the project environment.

`report/`  
Contains the final project report when completed.

`presentation/`  
Contains the final project presentation when completed.

---

## Environment Setup

### 1. Clone or Download the Repository

Open the project folder in VS Code.

The following commands should be executed from the root folder:

```text
robot-navigation-ml
```

---

### 2. Create a Virtual Environment

On Windows PowerShell:

```powershell
python -m venv .venv
```

---

### 3. Activate the Virtual Environment

```powershell
.\.venv\Scripts\Activate.ps1
```

After activation, the terminal should begin with:

```text
(.venv)
```

---

### 4. Install the Required Packages

```powershell
python -m pip install -r requirements.txt
```

The exact package versions used by the project are recorded in:

```text
requirements.txt
```

---

## Data Placement

The required raw dataset is already stored in:

```text
data/raw/sensor_readings_24.data
```

The notebook expects this file to remain in that location.

Do not manually modify the raw dataset.

---

## Running the Project

Open:

```text
notebooks/robot_navigation.ipynb
```

In VS Code:

1. Open the notebook.
2. Click the kernel selector in the top-right corner.
3. Select the Python interpreter from the project's `.venv`.
4. Confirm that the interpreter path ends with:

```text
robot-navigation-ml\.venv\Scripts\python.exe
```

5. Restart the notebook kernel.
6. Select **Run All**.
7. Allow the notebook to execute from the first cell to the final cell.

The notebook is designed to reproduce the complete workflow in order.

Do not manually run the final test section before the earlier model-selection sections when demonstrating the intended workflow.

---

## Expected Output

A successful full notebook execution should reproduce:

- dataset shape and schema information;
- missing-value and duplicate checks;
- descriptive statistics;
- class distribution;
- exploratory visualizations;
- training, validation, and test split information;
- majority-class baseline results;
- KNN parameter comparison;
- Decision Tree parameter comparison;
- Random Forest parameter comparison;
- validation model-comparison table;
- validation comparison visualization;
- approximate computational timings;
- selected final model;
- final test Accuracy;
- final Macro Precision;
- final Macro Recall;
- final Macro F1;
- class-wise classification report;
- final confusion matrix;
- error analysis;
- assumptions and limitations;
- Python and package-version information.

Small differences in computational timing are expected between computers.

---

## Reproducibility

The project supports reproducibility through:

- an unchanged raw dataset;
- a documented data dictionary;
- fixed `random_state=42` for stochastic models;
- an ordered and documented data split;
- leakage-safe KNN scaling using a pipeline;
- validation-based hyperparameter selection;
- an untouched final test set during model development;
- recorded package versions in `requirements.txt`;
- executable notebook cells in a clear order;
- documented environment-creation and execution instructions.

The notebook should be executed from a clean kernel using **Run All** before final submission.

---

## Technologies Used

- Python
- NumPy
- Pandas
- Matplotlib
- Seaborn
- scikit-learn
- Jupyter Notebook
- Visual Studio Code
- Git
- GitHub

---

## Main Python Libraries

The project uses:

```python
numpy
pandas
matplotlib
seaborn
scikit-learn
jupyter
```

Exact versions are available in:

```text
requirements.txt
```

---

## Conclusion

This project compares a majority-class baseline with K-Nearest Neighbours, Decision Tree, and Random Forest classifiers for predicting mobile-robot navigation actions from 24 ultrasonic sensor readings.

KNN substantially improves upon the baseline, while the two tree-based models achieve much stronger validation performance.

Random Forest achieves the strongest validation Macro F1 among the tested candidate models and is therefore selected before final test evaluation.

After retraining the selected Random Forest configuration using the complete development dataset, the model achieves approximately **98.53% final test Accuracy** and **0.984 Macro F1**.

The results demonstrate that the ultrasonic sensor readings contain strong information for predicting the recorded navigation actions. However, the results are specific to the available dataset and experimental setting. Additional evaluation with different robots, sensors, rooms, obstacles, and operating conditions would be required before real-world deployment.

---

## References

UCI Machine Learning Repository.  
**Wall-Following Robot Navigation Data.**  
Dataset ID: 194.  
DOI: 10.24432/C57C8W.  
License: CC BY 4.0.

The original dataset source and license information should be retained whenever the dataset or project is redistributed.