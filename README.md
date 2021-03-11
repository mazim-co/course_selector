# Course Selector. 
### An algorithm to predict a learning course.
*Manuel Zimmermann* | *Data Analytics, Campus Ironhack Berlin - 12. March 2021*

## Content
- [Project Description](#project-description)
- [Hypotheses / Questions](#hypotheses-questions)
- [Dataset](#dataset)
- [Cleaning](#cleaning)
- [Model Training and Evaluation](#model-training-and-evaluation)
- [Conclusion](#conclusion)
- [Future Work](#future-work)
- [Workflow](#workflow)
- [Organization](#organization)
- [Links](#links)

## Project Description
Over 900 learning course needs to be reviewed and migrated to a new Software. The workforce is low and the level of effort is high.
The purpose of this project is to help the Learning Team to decided wether an exiting course is worth to recolate in a new Learning Record Store or not.

## Hypotheses / Questions
Can we reduce the amount of excisting courses that needs to be relocation?
* $H_0$: Completion rate of a learning coure has no affect of the relocation activity.
* $H_1$: Completion rate has an affect.

## Dataset
* The dataset is build on multiple reports from the exicting software:
  * Course_details.csv ~1150 
  * Completion_count.csv ~163.567 
  * Course_categoy.csv ~93
  * User.csv ~6391
  * site_log.csv ~385.682
* To manage the the reports a SQL Database was developed with multiple table
  * 3NF (Third Normal Form)

## Cleaning
Before cleaning the data in JupyterNotebook with pandas, I collected and prepeared the nessceary data from the SQL Database.
```python
connection_string = 'mysql+pymysql://root:' + password + '@localhost/clz'
engine = create_engine(connection_string)
data = pd.read_sql_query('SELECT 
                         count(*) AS completion_count, c.course_category, cc.course_id, date_created, c.course_name, c.location 
                         FROM course_completion cc
                         JOIN course c ON c.course_id = cc.course_id
                         WHERE Completion_Status IN ('Complete', 'In progress')
                         AND c.course_category != 'Archive' AND c.course_category != 'Sandbox'
                         GROUP BY Course_id;', engine
                        )
```

* **Cleaning Porcess**
    1. *Header Standardization:*
        * Snake Casing for header standardization due to its simplicity. Snake Casing is a convention which replaces spaces with underscores and converts any upper-case letters to lower-case.
    2. *Dtypes*
        * Assigning the right data types to the features.
            * `date_created.dt.days`
    3. *Checking for and removing NaN Value*
        * At this point I checked for the percentage of null values per column. 
            * `location` ~18%
            * `content` ~6%
            * `course_type` ~18%
            * `tags` ~35%
        * Mostly NaNs are filled with 'Unkown' or 0.
    4. *Column Split*
        * Feature 'content' & 'tags' contains multiple strings. Both columns are encoded with pd.get_dummies
        


## Model Training and Evaluation
* **Scaling Numericals**
    * I opted to use a sklearn `StandardScaler` due to the distribution shape being non-normal. It ensures that our variables are scaled to values within -4 and 12.
* **Encoding Categoricals**
    * Categorical features `course_type` & `course_category` were encoded with pandas `.astype('category')`and `.cat.codes`

* **Cross Validation**
    * I used the Hyperparameter tuning `from sklearn.model_selection import GridSearchCV` in order to determine the optimal values for a given model.
* **Model Training**
    * Selecting Classifier:
        * `DecisionTreeClassifier`
        * `LogisticRegression`
        * `RandomForestClassifier`
    * Evaluation Classification Report:
```python:    
Accuracy of LogisticRegression on test set: 0.84

 [[17  4]
 [ 2 15]]
              precision    recall  f1-score   support

           0       0.89      0.81      0.85        21
           1       0.79      0.88      0.83        17

    accuracy                           0.84        38
   macro avg       0.84      0.85      0.84        38
weighted avg       0.85      0.84      0.84        38


Accuracy DecisionTreeClassifier on test set: 0.79

 [[16  5]
 [ 3 14]]
              precision    recall  f1-score   support

           0       0.84      0.76      0.80        21
           1       0.74      0.82      0.78        17

    accuracy                           0.79        38
   macro avg       0.79      0.79      0.79        38
weighted avg       0.80      0.79      0.79        38


Accuracy RandomForestClassifier on test set: 0.84

 [[18  3]
 [ 3 14]]
              precision    recall  f1-score   support

           0       0.86      0.86      0.86        21
           1       0.82      0.82      0.82        17

    accuracy                           0.84        38
   macro avg       0.84      0.84      0.84        38
weighted avg       0.84      0.84      0.84        38

```
## Conclusion
* With the optimal parameters for the RandomForrest Classifier I get an accuracy of 84%. In short, 8 of 10 are correct predicted.
* Best paramters:
```python
    criterion = 'entropy', 
    max_depth = 12, 
    max_leaf_nodes = 4, 
    min_samples_split = 2,
    n_estimators = 69
```

## Workflow
Gathering Data • Building SQL Database • Feature Selection • Cleaning • Scaling • Train&Test • HyperTuning • Applying Model • Visualization


## Links
[Repository](https://github.com/mazin-co/course_selctor)  
[Presentaion](https://www.canva.com/design/DAEYZswuxiM/1bNHhdMZoVRu-KPAAxdD1A/view?utm_campaign=designshare&utm_source=sharebutton)  
  
