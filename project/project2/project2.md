---
layout: project
title: Effects of Generative AI on Student Learning
tags: [Python, Scikit-learn, Machine Learning, Seaborn]
repo: https://github.com/Joey019/data-science-portfolio/blob/main/project/project2/Research.ipynb
---

# Effects of Generative AI on Student Learning

## 1. Background

Technology has slowly become an integral part of the education system. Many students are given a laptop or ipad from their schools in the United States in order to complete assignments and participate in class. In just the past few years new technologies has lead to the widespread use of generative AI.

As soon as generative AI became mainstream, many students started to utilize it to help them in their classes. Some used it as a tool for learning, taking advantage of its vast knowledge to learn from it. There are many cases where generative AI is able to explain complicated problems in a much simpler way. Others used it as a tool for cheating, letting generative AI do their homework, write papers, and even take exams for them. With how widely technology has been integrated into education, it is very easy to use generative AI to go through class work and assignments without actually learning anything.

I began this project with the question: **Does generative AI help students learn and improve their academic performance?** To investigate this question, I explored data from a high-school mathematics experiment in Turkey and built a regression model to predict students’ independent exam scores.

The analysis of this data had one main goal: examine how practice and exam performance differ across AI treatment groups, and determine which features help predict exam performance. These findings could help educators and researchers understand what to measure when evaluating AI-based learning.

Previous research shows why the distinction matters. Bastani et al. (2025), whose experiment supplied this dataset, found that unrestricted AI assistance could improve assisted performance while harming subsequent independent performance. In another setting, Kestin et al. (2025) found that a deliberately designed AI tutor improved learning relative to an active-learning class in college physics. These studies involved different students and instructional designs, so their results should not be treated as interchangeable. Together, they motivate examining how an AI tool is used and how learning is assessed. UNESCO’s guidance also emphasizes human oversight, privacy, and educational purpose when adopting generative AI (Miao & Holmes, 2023).

## 2. Understanding the Data

I used the _ai-learning_ dataset from the Carnegie Mellon University Statistics & Data Science Data Repository (Sagasta Pereira, 2026). It comes from a mathematics study involving Turkish high-school students. The dataset contains **3,255 student-session observations, 943 unique students, and 31 original columns**. A row represents a student’s results in one session, and a student can appear in up to four sessions.

The three treatment groups are control, vanilla AI, and augmented AI. The control group had no access to generative AI in theri sessions. The vanilla group had access to a standard ChatGPT interface to help answer questions. the augmented tool had access to an enhanced ChatGPT interface that was designed to help specifically with the topics covered in the session. Treatment was assigned at the classroom level, and classrooms retained their assignments across sessions. A student could only be a part of the same group across all session.

Two scores are important for this analysis:

| Variable   | Role in this project                         |
| ---------- | -------------------------------------------- |
| `Part2Tot` | Assisted practice score; used as a predictor |
| `Part3Tot` | Unassisted exam score; the prediction target |

Other available features describe prior GPA, grade level, study habits, educational support, household characteristics, and students’ survey responses. Both scores are recorded as proportions, so a value of 0.70 corresponds to 70% of the available score. All of the continuous data has been standarized to a value between 0 and 1, while most other count data is realtively low (below 10).

There are 5 survey questions included in the data that were asked after the students took the unassisted exam after each session. The survey covered topic related to the study session as well as the students' perception of their exam performance.

## 3. Exploratory Data Analysis

The clearest descriptive pattern was the difference between practice and exam scores. The notebook produced the following averages across recorded sessions:

| Treatment group | Mean practice score | Mean independent exam score |
| --------------- | ------------------: | --------------------------: |
| Control         |              31.62% |                      34.81% |
| Vanilla AI      |              49.48% |                      33.80% |
| Augmented AI    |              67.45% |                      35.13% |

Practice performance differed substantially across groups, while average independent exam performance was much closer. This led me to focus on the independent exam as the prediction target as assisted performance alone would give an incomplete picture of the effects of generative AI on learning.

<figure markdown="1">
![Practice-score distribution by treatment group](Assisted Exam by Treatement Group.png)
*Figure 1. Distribution of the assisted practice exam score based on the treatment group*
</figure>

The independent exam scores varied widely. Their overall mean was 34.61%, their median was 30.00%, and values ranged from 0% to 100%.

<figure markdown="1">
![Independent exam-score distributio](Independent Exam by Treatement Group.png)
*Figure 2. Distribution of the unassisted exam score based on the treatment group*
</figure>

**Missing Data**

| Variables    | Missing Values |
| ------------ | -------------- |
| Survey Q1    | 261            |
| Survey Q2    | 271            |
| Survey Q3    | 324            |
| Survey Q4    | 343            |
| Survey Q5    | 561            |
| Previous GPA | 54             |
| Grader       | 1              |

Missingness was heavily concentrated in the survey columns. There were hundreds of rows of missing data for the survey questions. I did not want to give the median value or the most frequent value for this missing data, since I felt that these were very important features. So I decided in the end to remove the rows with missing data from the survey questions I chose to use in the model.

The missing GPA values persisted across all sessions for the affected students. There were only 15 students that were missing GPA, but across all of their sessions it added up to 54 rows of data. Because these students were missing GPA from all of their sessions, it was not possible to assume their GPA by looking at one of their other sessions.

## 4. Data Preparation & Feature Selection

There were some survey responses that were spelled slighlty differently or had different capitalization, so I standardized survey responses so equivalent responses were treated as the same category. The uninterpretable responses were converted to missing values.

Prior GPA was filled using median imputation, with an additional indicator recording whether GPA had been missing. This kept those observations without treating an imputed value as an original measurement from the expirement. Session was one-hot encoded, allowing the model to note differences in session performance. Selected survey responses were ordinal encoded in their response order. The remaining selected numeric variables were passed through without scaling.

The final workflow used 20 input features, including survey questions Q1, Q2, and Q4. I chose these questions because I felt that they related to the students' perception of their exam performance as well as their perception of how much they learned in the sessions. Rows missing any response from those three questions were excluded from the dataset, leaving **2,899 observations from 927 students**. Student ID was used for grouping rather than as an input. Class, teacher, grader, the treatment-name column, household size, number of household children, parental education, and Q3/Q5 were excluded from the features used in the model. These features were demeed to be unrelated to academic performance.

The rest of the features from the dataset were kept as they were all either related to academic performance, or I was interested in their affect on the independent exam score. One such feature was the gender feature, which should not really have an effect on academic performance, but I was curious what its relationship would be. The way the model determined the treatment group was through the indicators `GPTBase` and `GPTTutor`.

The purpose of the feature set was to combine academic background, practice performance, treatment group, and student context. The survey extension tested whether students’ own assessments provided additional information. This focuses the final model on performance, study habits, educational support, and selected student characteristics.

Because students appeared repeatedly, I split the data by student using `GroupShuffleSplit`, with approximately 20% of students assigned to testing. In the survey dataset, this produced 741 training students and 186 testing students, with no student shared between the two sets. Five-fold `GroupKFold` similarly kept each student’s sessions together during cross-validation.

## 5. Model Comparison & Hyperparameter Tuning

I established a mean-prediction baseline model using `DummyRegressor`, then compared Linear Regression, Ridge, Random Forest, and Gradient Boosting. Linear models provided a straightforward reference, while tree ensembles could capture nonlinear relationships.

I evaluated performance using three regression metrics:

- **MAE:** the average absolute prediction error, in score units.
- **RMSE:** an error measure that gives greater weight to large mistakes.
- **R²:** The explained variation of the targegt variable by the features

The supplied survey-model results were:

| Model             | Mean CV R² | Mean CV MAE | Mean CV RMSE |
| ----------------- | ---------: | ----------: | -----------: |
| Mean baseline     |    -0.0044 |      0.2413 |       0.2838 |
| Linear Regression |     0.4826 |      0.1643 |       0.2036 |
| Ridge             |     0.4824 |      0.1644 |       0.2036 |
| Random Forest     |     0.5185 |      0.1559 |       0.1964 |
| Gradient Boosting |     0.5350 |      0.1525 |       0.1930 |

All trained models outperformed the baseline. The tree ensembles also achieved lower errors than the linear models in this comparison. This is consistent with useful nonlinear structure, although the score differences alone do not determine which interactions account for the improvement.

An earlier comparison suggested a small improvement when surveys were included. However, that comparison used different observations, and the latest workflow also changes the feature set. I therefore treat the survey result as exploratory rather than an isolated estimate of how much surveys improve prediction. A controlled comparison would use the same students, rows, and folds for both feature sets.

Gradient Boosting was the strongest candidate in the updated initial comparison on all three metrics. I then tuned it using `GridSearchCV`, minimizing student-grouped cross-validation RMSE within the training set. The search evaluated 375 parameter combinations across five folds, for 1,875 validation fits, followed by refitting the selected pipeline.

| Selected parameter               | Value |
| -------------------------------- | ----: |
| Number of Trees (`n_estimators`) |   150 |
| Learning rate                    |  0.05 |
| Maximum tree depth               |     4 |
| Minimum samples per leaf         |     5 |

The selected model achieved a tuning CV RMSE of **0.1909**. On the test split of 573 observations from 186 students, its reported results were:

## 6. Model Interpretation

## 7. What These Findings Mean—and Their Limits


## References & Code

- Bastani, H., Bastani, O., Sungu, A., Ge, H., Kabakcı, Ö., & Mariman, R. (2025). Generative AI without guardrails can harm learning: Evidence from high school mathematics. _Proceedings of the National Academy of Sciences, 122_(26), e2422633122. https://doi.org/10.1073/pnas.2422633122
- Kestin, G., Miller, K., Klales, A., Milbourne, T., & Ponti, G. (2025). AI tutoring outperforms in-class active learning: An RCT introducing a novel research-based design in an authentic educational setting. _Scientific Reports, 15_, Article 17458. https://doi.org/10.1038/s41598-025-97652-6
- Miao, F., & Holmes, W. (2023). _Guidance for generative AI in education and research._ UNESCO. https://www.unesco.org/en/articles/guidance-generative-ai-education-and-research
- Sagasta Pereira, V. (2026, June 19). _Can generative AI harm student learning?_ CMU S&DS Data Repository. https://cmustatistics.github.io/data-repository/technology/ai-learning.html

**Jupyter Notebook Code:** [Link](https://github.com/Joey019/data-science-portfolio/blob/main/project/project2/Research.ipynb)

## AI Usage

I used Claude Code to help with the design of my website and all the code involved in that process. I used ChatGPT's GPT-5.6 Sol to help support my ideas with code generation. All of the brainstorming and analysis was done by me, but a lot of the implementation in python was guided by ChatGPT.
