
# Placement Prediction using Logistic Regression
This project builds a Machine Learning model that predicts whether a student will get placed or not based on their academic performance.  
The model uses **Logistic Regression** and is trained using student attributes such as **CGPA** and **IQ score**.
This project demonstrates the **end-to-end machine learning workflow**, including data loading, preprocessing, visualization, model training, evaluation, and saving the trained model.
---
## Project Overview
Campus placements play a significant role in a student’s career. Educational institutions often want to analyze the factors that influence student placements.
In this project, a machine learning model is built to analyze patterns in student data and predict whether a student is likely to get placed.
The goal of this project is to demonstrate how **machine learning can be applied to real-world educational data to make predictive decisions.**
---
## Dataset Information
The dataset contains the following features:
| Feature | Description |
|--------|-------------|
| CGPA | Student's academic performance |
| IQ | Intelligence quotient score |
| Placement | Target variable (0 = Not Placed, 1 = Placed) |
Dataset file: placement.csv
---
## Technologies Used
- Python  
- NumPy  
- Pandas  
- Matplotlib  
- Scikit-learn  
- Jupyter Notebook  
- Pickle  
---
## Machine Learning Model
The model used in this project is:
**Logistic Regression**
Logistic Regression is used for **binary classification problems**, where the output variable has two possible outcomes.
0 = Not Placed
1 = Placed
---
## Project Workflow
The following steps were followed in the project:
1. Data Loading using Pandas  
2. Data Exploration and Understanding  
3. Data Visualization using Matplotlib  
4. Feature Selection  
5. Train-Test Split  
6. Model Training using Logistic Regression  
7. Model Evaluation  
8. Saving the trained model using Pickle  
---
## Project Structure
placement-project-logistic-regression
│
├── placement.csv
├── end_to_end_ml.ipynb
├── model.pkl
└── README.md
File description:
- **placement.csv** → Dataset used for training  
- **end_to_end_ml.ipynb** → Jupyter notebook containing the full ML workflow  
- **model.pkl** → Saved trained model  
- **README.md** → Project documentation  
---
## How to Run the Project
### 1. Clone the repository
git clone https://github.com/vikashkumarsingh21/placement-project-logistic-regression.git
### 2. Navigate to the project directory
cd placement-project-logistic-regression
### 3. Install required libraries
pip install pandas numpy matplotlib scikit-learn
### 4. Open Jupyter Notebook
jupyter notebook
### 5. Run the notebook
Open and run:
end_to_end_ml.ipynb
---
## Model Saving
After training the model, it is saved using the Pickle library.
Saved model file:
model.pkl
This allows the model to be reused later without retraining.
---
## Future Improvements
Some possible improvements for this project:
- Add more student features such as internships, projects, and communication skills  
- Build a web interface using **Flask or Streamlit**  
- Deploy the model online  
- Add more datasets for better prediction accuracy  
---
## Author

Vikash Kumar  
Computer Science and Engineering (AI & ML)

GitHub: https://github.com/vikashkumarsingh21
