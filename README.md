---

# Heart Disease Classification Analysis

Heart disease is one of the leading causes of death worldwide. Early detection and prediction can play an important role in supporting diagnosis and treatment. In this project, I analysed a dataset of patient health records to predict the presence of heart disease. I trained and compared two machine learning models — **Logistic Regression (LR)** and **Random Forest (RF)** — and evaluated them using accuracy, confusion matrices, classification reports, ROC–AUC curves, and feature interpretation.

---

## Dataset Overview

The dataset, stored in `heart_database.csv`, contains clinical features such as age, sex, chest pain type, blood pressure, cholesterol, maximum heart rate, and exercise response. The target variable is `condition`, where 1 represents heart disease and 0 represents no heart disease.

The dataset was already well-prepared: there were no missing values or duplicates. The only issue I encountered was hidden spaces in some column names, which caused errors when running plots or model training. This was fixed with `df.columns.str.strip()`.

To get an initial understanding of the data, I plotted histograms to view feature distributions and a correlation heatmap to examine relationships between variables. These visualisations provided a clear overview before moving on to modelling.

<p align="center">
  <img src="https://github.com/user-attachments/assets/52220d69-5afc-42dd-a64d-fa9927e35af8" alt="Heatmap" width="45%" />
  <img src="https://github.com/user-attachments/assets/e1b70392-c398-4625-b584-d47205e81791" alt="Histogram" width="45%" />
</p>

---

## Preprocessing

Before training, I prepared the data with the following steps:

* One-hot encoded categorical variables (`cp`, `restecg`, `slope`, `thal`)
* Scaled numerical features (`age`, `trestbps`, `chol`, `thalach`, `oldpeak`) using `StandardScaler`
* Split the data into training (80%) and testing (20%) sets, with stratification to maintain the balance of classes

The class distribution was fairly balanced, with about 54% no-disease cases and 46% disease cases in the training set, and similar proportions in the test set.

---

## Logistic Regression

The Logistic Regression model performed strongly, achieving an accuracy of **91.67%**.

* **Confusion Matrix:**

  ```
  [[32  0]
   [ 5 23]]
  ```

This shows that the model correctly identified all 32 healthy patients (no false positives) and 23 patients with heart disease, but it missed 5 diseased patients (false negatives).

The classification report confirms this behaviour: precision was 0.86 for class 0 and 1.00 for class 1, while recall was 1.00 for class 0 and 0.82 for class 1. In other words, Logistic Regression was very cautious, never labelling a healthy patient as sick, but at the cost of missing a few actual cases of heart disease.

---

## Random Forest

The Random Forest model reached an accuracy of **85.00%**.

* **Confusion Matrix:**

  ```
  [[30  2]
   [ 7 21]]
  ```

This model correctly detected most healthy patients, but it made 2 false positive predictions and missed 7 diseased patients. Precision for class 0 was 0.81, and for class 1 it was 0.91, while recall was 0.94 for class 0 and 0.75 for class 1.

Compared to Logistic Regression, the Random Forest was slightly less accurate overall. However, it excelled in recognising healthy patients, achieving a recall of 94%. The trade-off was that it missed more patients with heart disease.

---

## ROC and AUC

Receiver Operating Characteristic (ROC) curves were plotted for both models. The results showed that both Logistic Regression and Random Forest performed well, but Logistic Regression achieved a higher AUC. This confirmed its stronger generalisation and higher reliability on this dataset.

*(Visuals: ROC curve comparison between LR and RF)*

---

## Feature Importance

Understanding which features influence predictions is just as important as the predictions themselves.

* **Logistic Regression (coefficients):**

  * `ca` (+1.03)
  * `cp_3` (+0.92)
  * `sex` (+0.84)
  * `exang` (+0.72)
  * `cp_0` (-0.68)

Positive coefficients indicate features that increase the likelihood of heart disease, while negative ones reduce it. For example, more major vessels (`ca`) and exercise-induced angina (`exang`) both increase risk.

* **Random Forest (Gini importance):**

  * `oldpeak` (0.12)
  * `thalach` (0.12)
  * `ca` (0.10)
  * `age` (0.09)
  * `chol` (0.08)

Random Forest highlighted exercise-related measures such as ST depression (`oldpeak`) and maximum heart rate (`thalach`) as the strongest predictors, while both models agreed that `ca` is highly important.

*(Visuals: Bar charts of LR coefficients and RF feature importances)*

<p align="center">
  <img src="https://github.com/user-attachments/assets/a535620b-36a8-43f3-a1b2-1f4b3e2be7c0" alt="Logistic Regression Feature Importance" width="45%" />
  <img src="https://github.com/user-attachments/assets/4e5267b8-a958-48c4-b3ee-db219f866a60" alt="Random Forest Feature Importance" width="45%" />
</p>

---
## Conclusion

In summary, **Logistic Regression outperformed Random Forest** on this dataset, achieving higher accuracy (92% vs 85%) and stronger ROC–AUC performance. Logistic Regression was conservative, producing no false positives, while Random Forest was better at identifying healthy patients but missed more cases of disease.

Despite their differences, both models identified consistent and clinically meaningful features. The number of major vessels (`ca`), ST depression (`oldpeak`), maximum heart rate (`thalach`), chest pain type, and cholesterol levels all stood out as strong predictors of heart disease.

This project demonstrates the full process of working with health data: from exploration and preprocessing, through model training and evaluation, to interpretation of results.

---
