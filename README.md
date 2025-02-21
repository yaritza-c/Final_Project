# Final_Project

## Notes

### Communication

1. Using Slack for most communications
2. Meeting Mondays at 7 to prepare ourselves for the weekly meetings and segment deliverables

### Important Links

1. Dataset: [Consumer Finance Protection Bureau](<https://www.consumerfinance.gov/data-research/hmda/historic-data/?geo=nationwide&records=all-records&field_descriptions=codes>) (2017 data)
2. Google Slides presentation: [Mortgage Loan Approval Project](<https://docs.google.com/presentation/d/10wGSHtKTgT1KMNkHMMkzDM7hiJk7xlth1AUYh64E-4w/edit?usp=sharing>)
3. Tableau Dashboard: [Mortgage Approvals Dashboard](<https://public.tableau.com/app/profile/victor.pesantez/viz/DashboardFinalProject_16601819787170/DashboardFinalProject?publish=yes>)






### 7/14/2022 Session

1. Cleaning the dataset
a) Dropping columns applicant_race_2 through applicant_race_5 and coapplicant_race_2 through coapplicant_race_5
b) respondent_id is index
c) add zeros to denied_reason_1 to indicate no denial (working)
d) drop denied_reason_2 and denied_reason_3 (working)

2. For segment 2 deliverables: Anu updating database (SQL); Alex, Victor, Olivia running machine learning models to determine most accurate model with small loss; Yaritza setting up presentation in Google Slides

NOTE: plz wait until we have a cleaned version of dataset before SQL and tableau use.

NOTE: hdma has an api

### 7/19/2022 Session

1. Additional Cleaning: All steps above were taken and...
a) removing outliers from the data under assumption that our dataset it normally distributed

2. Binning the data
a) Using the Action Taken column, add new column that condenses codes 1 and 6 to "Accepted", 4 and 5 to "Other", and 2, 3, 7, and 8 to "Denied." Drop "Other," and then This will be used as our output variable 

3. Once cleaned, (no null values), we will split the dataset into a smaller proportional sample (bc 14mil rows is not working on our computers)
a) decide on a number of sample csv's with proportions of output column similar to population (large csv)
b) current potential options: race/ethnicity (combine columns??), income bracket (bin), population size (bin), or agency type

4. In reference to the rubric, Anu is cleaning the model via SQL and will export new table for us to use. The data will be split into proportional groups based on choices (Alex?), run machine learning models (everyone?), reconvene and determine which models are working the best for the groups that we choose. What do we want to include in the presentation (Yartiza), create visualizations of our data in Tableau or something similar (Victor)

### 7/21/2022 Session

1. Running into issues with github storage, so to combat, we will only create important csv files, which will include...
a) machine learning model - proportional sample (based on action taken summary column)
b) visualization portion - income and loan amount binned (for analysis of columns)
c) hypothesis tests? - proportional sample (other variables yet to be determined like race, agency type, etc)
NOTE: original file is too large to get it on here

2. Create a cleaning file so that all that has to be done is run input file and it will automatically spit out a useful output file.

### 7/26/2022 Session

1. Preprocessed file is complete. We took a 1% sample of the file and are using that to run models on (the sample has >100,000 rows, so its big). 
a) had to run the preprocessed file through two jupyter notebooks because I kept running into error issues: prop_sample.ipynb abd prop_sample_v2.ipynb

2. Running models today, will submit deliverable 2 today.

3.

### 7/28/2022 Session

1. Instructor confirms that database deliverables are met, and helps make integration more efficient by recommending the use of Pickle to compress results and take less space/time.
2. Anu imports pickle in database_integration.ipynb, re-runs the codes, and creates a .pkl output. Anu sends .pkl output and updated code to team in Slack channel. This completes the database integration portion of the project in segment 2.
3. Yaritza plans to share presentation work with Anu to ramp up presentation portion of the project. She shared google_slides_presentation_in_progress with team, and showed samples of a deck template for use in the final presentation.
4. Victor creates charts to display metrics like denial_reason & applicant_income, then begins work in Tableau to plot data and display results. 
5. Team will submit complete deliverable 2 today

# Rubric Double Check

## Segment One

### Presentation

1. Selected Topic: Mortgage Loan Approval Calculator
2. Reason why topic is selected: Housing market is nuts, thus making it a topic of interest
3. Description of data source: hdma 2017 loan data
4. Questions we want data to answer: what variables most affect the approval of a mortgage

### Github

1. Include README: Done
2. README has description of the communication protocols: see above
3. Individual branches for each member: Done
4. Four commits from each member: probably impossible for segment 1

### Machine Learning Model

1. Take in data from provisional database: done
2. outputs labels for input data: loan approval is output and "several factors" for input

### Database:

1. sample data that mimics expected final database structure or schema: hmda 2017 nationwide; lending club?
2. Draft machine learning module is connected to the provisional database: working

## Segment 2

### Presentation

1. Description of the data exploration phase of the project: ok
2. Description of the analysis phase of the project: ok
3. Begin drafting slides in google slides: ok

### Github

1. All code necessary to perfomr exploratory analysis in github: working
2. Some code necessary to complete machine learning portion: ok

### Machine Learning Model

1. Description of preliminary data preprocessing: been taking notes above, however, to summarize, changed values in the columns where there are categorical variables, eliminated useless columns, reworked the state column to regions (US), created a categorizing variable with the action taken column (approved v denied).

2. Description of preliminary feature engineering and preliminary feature selection, including their decision-making process: 

a) columns eliminated: respondent_id, applicant_race_2, applicant_race_3, applicant_race_4, applicant_race_5, co_applicant_race_2, co_applicant_race_3, co_applicant_race_4, co_applicant_race_5, denial_reason_1, denial_reason_2, denial_reason_3, rate_spread, edit_status, sequence_number, application_date_indicator, msamd, as_of_year, county_code, census_tract_number

b) reason behind column elimination: identifiers do not apply to machine learning models, applicant race 1 and coapplicant race 1 should be sufficient as there are too many null values in the other applicant and coapplicant race columns, denial reasons act as a predictor of the model so removed to provide better learning model, unnecessary columns, or columns with too many categorical variables that are not easily binned or can be explained by another column

c) transformations: States to regions - converts state numbers to regions in order to better bin the data, all other categorical variables have values changed to reflect their actual meaning vs the number it is assigned. This also converts dtype to object, which makes it easier to separate out using OneHotEncoder. Action taken - converts the 8 variables into two variables which is more usable for categorical model. 

d) removed null values from remaining rows bc it is immaterial (10mil values remaining)

e) purchaser type removed in the machine learning model runs because it also acts like the denial reason - if it isn't there, than it is approved and falsely predicts model

f) see prop_sample.ipynb and prop_sample_v2.ipynb for data preprocessing and feature selection and engineering

3. Description of how data was split into training and testing sets: Olivia running neural network and Alex running random forest classifier

- In this segment, the RandomForestClassifier was used with a class_weight="balanced" argument to prevent the model from predicting only 1s for action_taken_summary, since our dataset is very imbalanced.  This was used in place of stratification.

## Segment Three

### Machine Learning Model

1. Finally got the dataset together, running models of pickled data. 

a) Initial Cleaning Phase 1: Removed as_of_year, applicant_race_2, applicant_race_3, applicant_race_4, applicant_race_5, co_applicant_race_2. co_applicant_race_3, co_applicant-race_4, co_applicant_race_5, denial_reason_2, denial_reason_3, rate_spread, edit_status, sequence_number, and applicantion_date_indicator bc of duplicity and obsolescense. 

b) Intial Cleaning Phase 2: For columns agency_code, loan_type, property_type, loan_purpose, owner_occupancy, preapproval, action_taken, state_code, applicant_ethnicity, co_applicant_ethnicity, applicant_race_1, co_applicant_race_1, applicant_sex, co_applicant_sex, purchaser_type, hoepa_loan, and lien_status, change values to reflect categories and change dtype to object

c) Intial Cleaning Phase 3: Transform state_code to Region for a more concise explanation. Also transform action_taken to action_taken_summary to create a column for approvals and denials

d) Initial Cleaning Phase 4: Fill N/A values with 0 in columns msamd, applicant_income_000s, and denial_reason_1

e) Initial Cleaning Phase 5: Drop any remaining rows with null values from the dataset. Retention: 97.98% of data points (13,997,124 rows retained out of 14,285,496 rows from the raw data)

f) Transformation Phase 1: Export to pickle file and share with group via Google Drive

g) Machine Model Cleaned Version Phase 1: Drop denial_reason_1, lien_status, hoepa_status, state_code, purchaser_type, respondent_id, and action_taken columns because of circular references or obsolescence. This was determined from prior models using sample data

h) Machine Model Cleaned Version Phase 2: Drop rows that do not provide enough information to base the model off of. Only use "Home Purchase" from loan_purpose column, remove "Not Applicable" values from owner_occupancy, remove "Information not Provided" and "Not Applicable" values from applicant_sex, co_applicant_sex, applicant_ethnicity, and co_applicant_ethnicity columns, remove "Not Applicable" values from preapproval column, remove outliers from loan_amount_000s column, remove "2" values from action_taken_summary (anything other than approved or denied status).

i) Machine Model Cleaned Version Summary: Retained about 1,700,356 datapoints/rows

j) Transformation Phase 2: Export to new pickle file for use on machine learning models

k) Preprocess Phase 1: Rows with dtype "object" transformed into categorical variables. These columns include agency_code, loan_type, property_type, owner_occupancy, preapproval, applicant_ethnicity, co_applicant_ethnicity, applicant_race_1, co_applicant_race_1, applicant_sex, co_applicant_sex, Region

l) Preprocess Phase 2: Drop any exported categorical variable columns that are redundant

2. Neural Networks

a) Using action_taken_summary as the target output, run the neural network...

b) First run through: 3 hidden layers, first two are relu, third is leaky relu, output is sigmoid. Loss metric is binary_crossentropy, optimizer is adam, with metrics as accuracy. Using 50 epochs, the final output is loss: 0.3837 ccuracy: 0.8715

c) Second run through: same as above, just removed the redundant columns (preprocess phase 2)

d) Third run through: added a hidden layer, change all input/hidden layer activation functions to relu, same loss and accuracy as first run through

e) Fourth run through: 

3. Random Forest Classifier

- Train-test split was stratified on our target y (action_taken_summary) in order to assist the model's detection of denials, as the unstratified, imbalanced data produced a classifier only predicting 1s.  This was tested against using the class_weight = "balanced argument in the classifier and against using the combination of the two.  The model with stratification only performed the best of the three.

a) We began by throwing a sample of 25,000 from our dataset into a logistic model targeting action_taken.  This resulted in an accuracy score of 0.55.  We at first replaced nulls in denial_reason_1 with zeroes so as to maintain approved rows when dropping nulls in our dataframe, but later realized that the inclusion of the column was incorrect since it implies our target.  The poor accuracy score even with a column that states whether a loan was approved or denied in the features and our desire to see feature importances led us to decide to switch from a logistic model to a random forest.

b) Initial decision tree and random forest models ran on a random sample of ~100,000 out of the dataset with 1000 estimators, with accuracy scores of 0.86 and 0.89 respectively.  We used StandardScaler(), OneHotEncoder() and train_test_split() from  sklearn to transform our data.  Feature importances showed purchaser_type to be most important, but this was because it contained a label for loans not originated, which directly implies action_taken_summary as not being approved.

c) In the rf2 notebook, the decision tree model was left behind since the random forest is stronger.  Dropping purchaser_type lowered the accuracy score to 0.80.  This led to applicant_income_000s and loan_amount_000s as the most prominent importances, which makes sense intuitively.

d) At this point we started running models from the full dataset, so to get them to run successfully we had to impose stronger restrictions than on the samples.  This included restricting applications to those for home purchases only rather than refinancing or home improvements, and eliminating any rows with incomplete information that were not captured by dropping nulls.  This successfully reduced the dataset to 1.7M rows while allowing us to be more pointed in what we were analyzing.  The model was ran as RandomForestClassifier(max_depth =3, max_features = 3, n_estimators = 100) in the interest of making sure my computer could handle the operation.  At first, the model appeared very strong, with an accuracy score above 0.86, but the confusion matrix showed that this was because the model predicted every single application to be approved.  Other people online who encountered issues like this applied the argument class_weight="balanced", which adjusts weights to be inversely proportional to class frequencies.  Applying this argument dropped accuracy significantly to below 0.64, but I left it in as it did not feel great about the model predicting every application as approved.

e) This model expanded the random forest to a max_depth of 10, a max_features of 5, and 250 estimators.  This improved accuracy to just above 0.66 with slightly better precision.  Feature importances were re-evaluated, with applicant_income_000s and loan_amount_000s still at the top and together representing about 30% of the variance.

f) Other sources suggested that random forest models run best with a max_features set near the square root of the number of total features and without a limit for max_depth.  I applied these parameters with 500 estimators and the cell has been running for about two hours at this point.  I'll let it keep going for now but if it doesn't do anything by the end of the night I'll interrupt the cell and re-run it with 100 or 250 estimators and hope for something by morning.

### Dashboard

1. Using Tableau, visuals created using final dataset after experimenting using sample file for ease of use
2. Link above in readme.

## Segment 4

### Machine Learning Model

#### Data Preprocessing, Feature Engineering, and Feature Selection

- Throughout the project, we found multiple columns in the set that would directly state if a mortgage application was approved or denied.  Columns such as reasons for denial were removed.

- After rows containing nulls were dropped, it was found that many columns contained entries such as "Not Applicable" or "Information not Provided", so these rows were removed as well.

- We restricted loan_purpose to only home purchase applications to be more targeted in our approach

- We removed outliers in requested loan amount so as to subset our data to the most "typical" homes (even though our remaining amounts still went well over $1M).  We thought it was best to perform this on requested loan amount only and not for applicant income.  This is because we believe that a relatively low number of high-income applicants tend to buy lots of homes and have a significant presence and effect.

- Two categorical features were transformed from the original data: action_taken and state_code.  We remapped action_taken's labels to reduce down to approval, denial, or withdrawal/incompletion and then subsetted to only approvals and denials so we would have a binary target variable.  For state_code, we created a dictionary of states and their region in the country and remapped them to the region for use in one-hot-encoding, since having fifty categories for states would not be useful.

- After one-hot-encoding, for any categories with only two labels, the second column was dropped.

- When splitting the data into training and testing sets, the split was stratified on our target, action_taken_summary, to preserve the proportions in the resulting sets.

- Because we had numerical variables with a large range, we applied sklearn's StandardScaler to our data.

##### Random Forest Classifier

- A random forest classifier was selected partially because we were interested in seeing the feature importances of the dataset.  The largest limitation of the model was in the amount of computing power it required compared to a simpler decision tree or a logistic model.

- Model performance was tested across a variety of parameters.  The number of estimators was initially set at one hundred, and tests with a larger number did not provide significant improvement and also required much long run times, so one hundred was selected.  The classifier was initially set to a max_depth of three, but accuracy was very poor.  Online research suggested that not limiting the estimator depths was optimal, so the limit was removed and accuracy improved.  The maximum number of features per tree was tested at six, seven, eight, and nine features.  Models with the max_features paramet set to eight or nine performed the best and almost identically, so eight was decided on for efficiency.

- Because our approvals heavily outweighed denials, the unstratified model predicted every entry as an approval.  To combat this, the classifier was retested by stratifying the train-test split on the target, balancing the class weight in the classifier, and combining both methods.  Using only stratification provided the best recall for denials and highest accuracy score (0.87).  Then, the classifier was tested with oversampled and undersampled resamplings of the data.  Out of these, random oversampling only showed a loss in accuracy of about .01, with significantly improved recall for denials (0.11 vs 0.05-0.07) while maintaining a recall of 0.97 (previously 0.99) for approvals.

- We extracted feature importances from this model to see which variables were influential in the results of the applications and then used those features to build visualizations in Tableau.  Across all models, applicant income and requested loan amount were always the two most important features.

- We attempted SMOTE oversampling, cluster centroids undersampling, and SMOTEENN combination sampling techniques on our data as well.

- SMOTE provided an improved recall for denials of 0.20 with 0.82 overall accuracy.  Recall for approvals dropped to 0.92 while maintaining a 0.88 precision score.

- SMOTEENN yielded 0.45 recall for denials, the highest out of all tested models by far.  This came at the cost of accuracy, which dropped to just under 0.70, as well as approval recall, which dropped to 0.74.  The top ten feature importances were much closer in value, but applicant income and requested loan amount remained at the top.

- Cluster centroid resampling was unsuccessful.  SMOTEENN took a very long time to run, so I hoped that time would be all the cluster centroids technique needed, but it yielded memory errors stating that 200+ GiB of memory would need to be allotted for the resampling alone, and my computer is not that great so I didn't have a good workaround for this issue.

- Because this project is centered around trends in mortgage approvals rather than detecting as many risky loans as possible, the SMOTE-adjusted model probably provides the best results out of the random forest models.  It maintains high precision and recall in approvals, with moderate precision and recall (out of tested models) for denials, so we are able to feel reasonably confident in our feature importances.

##### Neural Network

- During the fourth week, had to rework the neural networks to balance the dataset so that it doesn't automatically assume an approval for each of the test datapoints

- Using random_state = 42 across all applicable code. y is stratified in the train-test split

- Two hidden layers are used in the neural network with relu as the input and hidden activation functions and sigmoid as the output

- loss = binary cross-entropy, the optimizer = adam, the accuracy = binary accuracy

- y-predict takes any prediction and rounds to the nearest binary value (0 for denial and 1 for approval)

- confusion matrix created using tensorflow to find precision and accuracy ratios 

- the only thing changed in these is the balancing function. Use of random oversampling, random undersampling, SMOTE, SMOTEENN, and Centroid Clusters (?)

- SMOTE: 

 ![image](https://user-images.githubusercontent.com/101011641/183313369-36af7c4f-5c43-4bed-9a6a-c5f6990cbf22.png)

- Random Oversampling: 

 ![image](https://user-images.githubusercontent.com/101011641/183313406-4ef991e2-8426-4d07-b742-87399e67a0e3.png)

- Random Undersampling: 

 ![image](https://user-images.githubusercontent.com/101011641/183313439-0b3ed8f6-400f-4964-a06f-64a69ae215f0.png)

- SMOTEENN: 

 ![image](https://user-images.githubusercontent.com/101011641/183313736-7ed93d67-bc77-4944-8acb-f2254582e20f.png)

- Clustered Centroid: N/A ran into MemoryError 3 times. Not trying again

- No simple way to extract feature importance from neural network, thus I will be making an analysis on the confusion matrix statistics


