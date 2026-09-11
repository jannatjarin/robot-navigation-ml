# Robot Navigation Action Classification

Programming in Python Final-Term Group Project.

This project will use machine-learning models to classify the navigation action of a mobile robot using numerical ultrasonic sensor readings.

## Team Members

1. Anika Sultana Anu
2. Fatematuz Johora
3. Jannat Jarin
4. Nusrat Jahan Sumaiya

## Dataset

This project uses the Wall-Following Robot Navigation Data from the UCI Machine Learning Repository.

The data were collected while a SCITOS G5 mobile robot followed a wall around a room using 24 ultrasonic sensors.

The dataset is sequential. The robot navigated the room clockwise for four rounds, with sensor readings sampled at approximately 9 observations per second.

The dataset contains 5,456 observations, 24 numerical sensor features, and one target variable.

The project uses the `sensor_readings_24.data` file, which contains 24 numerical ultrasonic sensor readings and one target class representing the robot's navigation action.

The four target classes are:

- Move-Forward
- Slight-Right-Turn
- Sharp-Right-Turn
- Slight-Left-Turn

Dataset citation:

Freire, A., Veloso, M., & Barreto, G. (2009). Wall-Following Robot Navigation Data. UCI Machine Learning Repository.

DOI: 10.24432/C57C8W

License: CC BY 4.0

A detailed description of the variables is available in `data/data_dictionary.md`.

## Project Workflow

The project follows an end-to-end classification workflow:

1. Dataset loading and provenance documentation
2. Data audit and cleaning
3. Exploratory data analysis
4. Feature and target preparation
5. Ordered training, validation, and test split
6. Majority-class baseline
7. K-Nearest Neighbours classification
8. Decision Tree classification
9. Random Forest classification
10. Validation-based model comparison
11. Final model selection
12. Final test evaluation
13. Error analysis and limitations

## Models

The following approaches are compared:

- Majority-class baseline
- K-Nearest Neighbours
- Decision Tree
- Random Forest

The final model is selected using validation Macro F1. The final test set is reserved until model selection is complete.

## Running the Project

Create a virtual environment:

```powershell
python -m venv .venv