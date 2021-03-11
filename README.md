# Course Selector. An algorithm to a learning course.
*[Manuel Zimmermann]*

*[Data Analytics, Campus Ironhack & 12. March 2021]*

## Content
- [Project Description](#project-description)
- [Hypotheses / Questions](#hypotheses-questions)
- [Dataset](#dataset)
- [Cleaning](#cleaning)
- [Analysis](#analysis)
- [Model Training and Evaluation](#model-training-and-evaluation)
- [Conclusion](#conclusion)
- [Future Work](#future-work)
- [Workflow](#workflow)
- [Organization](#organization)
- [Links](#links)

## Project Description
Over 900 learning course needs to be reviewed and migrated to a new Software. The workforce is low and the level of effort is high.
The purpose of this project is to help the Learning Team to decided wether an exiting course is worth to recolate in a new Learning Record Store.

## Hypotheses / Questions
* Can we reduce the amount of excisting courses that needs to be relocation?
* H null = Completion rate of a learning coure has no affect of the relocation activity.
* H alternative = Completion rate has an affect.

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
Before cleaning the data in JupyterNotebook with Pandas I collected the applicable and nessceary data from the SQL Database
'''python:
connection_string = 'mysql+pymysql://root:' + password + '@localhost/clz'
engine = create_engine(connection_string)
data = pd.read_sql_query('SELECT count(*) AS completion_count, c.course_category, cc.course_id, date_created, c.course_name, c.location FROM course_completion cc
JOIN course c ON c.course_id = cc.course_id
WHERE Completion_Status IN ('Complete', 'In progress')
AND c.course_category != 'Archive' AND c.course_category != 'Sandbox'
GROUP BY Course_id;', engine)
'''
## Analysis
* Overview the general steps you went through to analyze your data in order to test your hypothesis.
* Document each step of your data exploration and analysis.
* Include charts to demonstrate the effect of your work.
* If you used Machine Learning in your final project, describe your feature selection process.

## Model Training and Evaluation
*Include this section only if you chose to include ML in your project.*
* Describe how you trained your model, the results you obtained, and how you evaluated those results.

## Conclusion
* Summarize your results. What do they mean?
* What can you say about your hypotheses?
* Interpret your findings in terms of the questions you try to answer.

## Future Work
Address any questions you were unable to answer, or any next steps or future extensions to your project.

## Workflow
Outline the workflow you used in your project. What were the steps?
How did you test the accuracy of your analysis and/or machine learning algorithm?

## Organization
How did you organize your work? Did you use any tools like a trello or kanban board?

What does your repository look like? Explain your folder and file structure.

## Links
Include links to your repository, slides and trello/kanban board. Feel free to include any other links associated with your project.


[Repository](https://github.com/)  
[Slides](https://slides.com/)  
[Trello](https://trello.com/en)  
