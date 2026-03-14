# 🎓 Placement Prediction using Logistic Regression

<div align="center">

![Python](https://img.shields.io/badge/Python-3.8+-3776AB?style=for-the-badge&logo=python&logoColor=white)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-F7931E?style=for-the-badge&logo=scikit-learn&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?style=for-the-badge&logo=jupyter&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-150458?style=for-the-badge&logo=pandas&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=for-the-badge&logo=numpy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557c?style=for-the-badge&logo=python&logoColor=white)

![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen?style=flat-square)
![ML Model](https://img.shields.io/badge/Model-Logistic%20Regression-blueviolet?style=flat-square)

</div>

---

## 📌 About The Project

This project builds a **Machine Learning model** that predicts whether a student will get placed or not based on their academic performance.

The model uses **Logistic Regression** and is trained using student attributes such as **CGPA** and **IQ score**.

This project demonstrates the **end-to-end machine learning workflow**, including data loading, preprocessing, visualization, model training, evaluation, and saving the trained model.

---

## 🧩 Project Overview

Campus placements play a significant role in a student's career. Educational institutions often want to analyze the factors that influence student placements.

In this project, a machine learning model is built to analyze patterns in student data and predict whether a student is likely to get placed.

> 💡 The goal of this project is to demonstrate how **machine learning can be applied to real-world educational data to make predictive decisions.**

---

## 📊 Dataset Information

The dataset contains the following features:

| Feature | Type | Description |
|---------|------|-------------|
| `CGPA` | Float | Student's academic performance score |
| `IQ` | Integer | Intelligence quotient score |
| `Placement` | Binary | Target variable — `0` = Not Placed, `1` = Placed |

> 📁 **Dataset file:** `placement.csv`

---

## 🛠️ Technologies Used

| Technology | Purpose |
|------------|---------|
| ![Python](https://img.shields.io/badge/-Python-3776AB?logo=python&logoColor=white&style=flat-square) **Python** | Core programming language |
| ![NumPy](https://img.shields.io/badge/-NumPy-013243?logo=numpy&logoColor=white&style=flat-square) **NumPy** | Numerical computing |
| ![Pandas](https://img.shields.io/badge/-Pandas-150458?logo=pandas&logoColor=white&style=flat-square) **Pandas** | Data manipulation & analysis |
| ![Matplotlib](https://img.shields.io/badge/-Matplotlib-11557c?logo=python&logoColor=white&style=flat-square) **Matplotlib** | Data visualization |
| ![Scikit-learn](https://img.shields.io/badge/-Scikit--learn-F7931E?logo=scikit-learn&logoColor=white&style=flat-square) **Scikit-learn** | Machine learning model |
| ![Jupyter](https://img.shields.io/badge/-Jupyter-F37626?logo=jupyter&logoColor=white&style=flat-square) **Jupyter Notebook** | Interactive development |
| **Pickle** | Model serialization & saving |

---

## 🤖 Machine Learning Model

### Logistic Regression

Logistic Regression is used for **binary classification problems**, where the output variable has two possible outcomes.

```
Output Classes:
  0  →  Not Placed ❌
  1  →  Placed     ✅
```

---

## 🔄 Project Workflow

```
1. Data Loading using Pandas
        ↓
2. Data Exploration and Understanding
        ↓
3. Data Visualization using Matplotlib
        ↓
4. Feature Selection
        ↓
5. Train-Test Split
        ↓
6. Model Training using Logistic Regression
        ↓
7. Model Evaluation
        ↓
8. Saving the trained model using Pickle
```

---

## 📁 Project Structure

```
placement-project-logistic-regression/
│
├── placement.csv           # Dataset used for training
├── end_to_end_ml.ipynb     # Jupyter notebook — full ML workflow
├── model.pkl               # Saved trained model
└── README.md               # Project documentation
```

---

## 🚀 How to Run the Project

### 1. Clone the repository

```bash
git clone https://github.com/vikashkumarsingh21/placement-project-logistic-regression.git
```

### 2. Navigate to the project directory

```bash
cd placement-project-logistic-regression
```

### 3. Install required libraries

```bash
pip install pandas numpy matplotlib scikit-learn
```

### 4. Open Jupyter Notebook

```bash
jupyter notebook
```

### 5. Run the notebook

Open and run:
```
end_to_end_ml.ipynb
```

---

## 💾 Model Saving

After training the model, it is saved using the **Pickle** library.

```python
import pickle

# Save the model
pickle.dump(model, open('model.pkl', 'wb'))

# Load the model later
model = pickle.load(open('model.pkl', 'rb'))
```

> ✅ Saved model file: `model.pkl`
>
> This allows the model to be reused later **without retraining**.

---

## 🔮 Future Improvements

- [ ] Add more student features such as internships, projects, and communication skills
- [ ] Build a web interface using **Flask or Streamlit**
- [ ] Deploy the model online
- [ ] Add more datasets for better prediction accuracy

---

## 👤 Author

**Vikash Kumar**
📚 Computer Science and Engineering (AI & ML)
GitHub: https://github.com/vikashkumarsingh21

---

<div align="center">

⭐ If you found this project helpful, please give it a star!

</div>
