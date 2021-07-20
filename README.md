# Capstone Diabetes Readmission Prediction
Predictive Model for Diabetes Readmission

---

# <B>Table of Contents<B>
  
---
  
1. [Research Question](https://github.com/Kchurch29/Capstone-Diabetes-Readmission-#research-question)
2. [Data Collection](https://github.com/Kchurch29/Capstone-Diabetes-Readmission-#data-collection)
3. [Data Dictionary](https://github.com/Kchurch29/Capstone-Diabetes-Readmission-#data-dictionary)
4. [Data Extraction and Preparation](https://github.com/Kchurch29/Capstone-Diabetes-Readmission-#data-extraction-and-preparation)
5. [Distribution of Variables](https://github.com/Kchurch29/Capstone-Diabetes-Readmission-#distribution-of-variables)
6. [Feature Selection](https://github.com/Kchurch29/Capstone-Diabetes-Readmission-#feature-selection)
7. [Class Imbalance](https://github.com/Kchurch29/Capstone-Diabetes-Readmission-#class-imbalance)
8. [Summary and Implications](https://github.com/Kchurch29/Capstone-Diabetes-Readmission-#summary-and-implications)
9. [References](https://github.com/Kchurch29/Capstone-Diabetes-Readmission-#references)
  
---
  
# <B>Research Question<B>
  
&ensp;Diabetes is a condition that affects how your body works when food is digested. When food is broken down into sugar or glucose, it is released into the bloodstream. When the sugar levels in your blood rises, the pancreas releases insulin. Insulin allows the body to absorb glucose from the blood. People with diabetes do not have the capability to produce enough insulin or cannot make any to meet the body’s needs. With more than 34 million people in America having diabetes, and over 88 million people having prediabetes, and the estimated costs of diagnosed diabetes having risen to 327 billion dollars, the need for effective recognition of signs of readmission and steps for prevention is paramount. From 2012-2017 the costs have risen over 26 percent. (The Cost of Diabetes, n.d.). 
	
&ensp; Studies have shown that 15-25 percent of diabetic patients will have unplanned readmissions within 30 days, with many of these being preventable (Staff, 2018).  By examining a patient’s readmission history as it relates to diabetes treatment, an effective plan to reduce readmissions can reduce costs and lead to a better quality of life for the patient. Using patient’s demographic, medicinal intake, diagnosis status, and admission information, a model can predict a patient being readmitted in the first 30 days after release.

---
  
# <B>Data Collection<B>
 
&ensp; The data used in this study is publicly available and was downloaded from Kaggle.com. The data comes from 130 hospitals in the United States and was collected from 1999-2008. The advantage of using this data is that it easily available and has many observations, over 100,000. As with most data, there are many missing values or values that must be changed, but the content makes the effort worthwhile. A downside of this dataset is its age. Even though it was collected over a 10-year span, it has also been over a decade since data collection ended. Many advances in medicine and prevention could make the results not as meaningful. Another disadvantage of this data is not knowing what hospitals this data came from. Knowing the region from which the data was obtained from could be helpful for future studies.  
  
---
  
# <B>Data Dictionary<B>
  
* Encounter ID: Unique identifier of an encounter
* Patient Number: Unique identifier of a patient
* Race: Race of patient
* Gender: Gender of patient
* Age: Grouped in 10-year intervals
* Weight: Weight in pounds of patient
* Admission Type: Integer identifier with 9 distinct values (Examples: emergency, urgent, elective, newborn...etc.)
* Discharge disposition: Integer indentifier with 29 distinct values (Examples: discharged to home, expired, hospice...etc)
* Admission Source: Integer indetifier with 21 distinct values (Examples: physician referral, emergency room, transfered from hospital...etc)
* Time in hospital: Number of days in hospital in Integer form
* Payer Code: Integer indentifier with 23 distinct values (Examples: Blue Cross\Blue Shield, Medicare, self-pay...etc)
* Medical Specialty: Nominal (Examples: cardiology, internal medicine, surgeon ... etc)
* Number of lab procedures: Integer number of lab procedures performed during encounter
* Number of procedures: Integer number of procedures other than lab performed during encounter
* Number of medications: Integere number of medications administered during encounter
* Number of outpatient visits: Integer number of outpatient visits in the year preceding the encounter
* Number of emergency visits: Integer number of emergency visits in the year preceding the encounter
* Number of inpatient visits: Integer number of inpatient visits in the year preceding the encounter
* Diagnosis 1: Primary diagnosis coded as ICD9 medical codes. 848 distinct values
* Diagnosis 2: Secondary diagnosis coded as ICD9 medical codes. 923 distinct values
* Diagnosis 3: Additional diagnosis coded as ICD9 medical codes. 954 distinct values
* Number of diagnosis: Number of diagnosis entered into the system
* Glucose serum test result: Indicates the range of the test result: Values ">200", ">300", "normal", "none"
* A1C result: Nominal values of ">7", "normal", and "none"
* Change of medications: Nominal value if there was a change in diabetic medications: Values "yes" or "no"
* Diabetes medications: Nominal value if there was any diabetic medications prescribed. Values "yes" or "no"
* 24 different features for medications: Nominal names of medications prescribed
* Readmitted: Nominal value if patient was readmitted. Values ">30", "<30", "No"
  
---
  
# <B>Data Extraction and Preparation<B>
  
&ensp; The dataset is imported into R Studio as seen below and a new data set named “data” is created to keep the original dataset in the original form:
	
![image](https://user-images.githubusercontent.com/87247651/126395421-ee74b5c4-6b14-47a5-9b85-2a30a09b0cae.png)
 
&ensp; One of the assumptions of logistic regression is independence of observations (Kassambara & U, 2018), meaning none of the observations should be related to another. Looking at the data, there are multiple records for some patients as they have more than one visit. To keep in line with the independence of observarions assumption the data will be sorted by “patient_nbr”, so that the first record for patients with multiple visits will be kept for further analysis.

![image](https://user-images.githubusercontent.com/87247651/126395493-f7a1d90f-3ab9-4bb7-b326-64e58ae7c724.png)

The figure above shows the data ordered by “patient_nbr”. The figure below shows a new dataset name “first_patient” being created that only takes the first record for each patient if there are more than one record. This will be the new dataset that variable modifications will be done on.
	
![image](https://user-images.githubusercontent.com/87247651/126395520-d0f2da0e-4723-4a0f-9832-039415da0e54.png)
 
&ensp; Now that the dataset has the only patient records that will be used, the str() command shows what data type each variable is:
	
![image](https://user-images.githubusercontent.com/87247651/126395586-b8cb48b3-fc6f-45ac-8e1f-5e6ec743d78b.png)
 
To be able to clearly see what variables need to be dropped or altered, a temporary dataset name “factors” will be created and analyzed to see what variables should be dropped or altered.  Once the temporary dataset is created, the variables that are of the character data type will be converted to factors. A str() command confirms the data has been changed to factors. This is done in the screenshot seen below:
	
![image](https://user-images.githubusercontent.com/87247651/126395628-0f9a51f5-c5c8-45b3-a811-a5ec26f5e4c0.png)
 
Using the summary command, the statistics for each variable can been observed to see what further action to take. 
	
![image](https://user-images.githubusercontent.com/87247651/126395664-e0b3a03a-61f0-4ac8-8da7-d7a18275295a.png)

&ensp; The figure above shows “race” as having 1948 missing values, and “gender” having 3, so those entries will not be included. The code below shows the process of removing these on the “first_patient” dataset:
 
![image](https://user-images.githubusercontent.com/87247651/126395742-3831236c-a9fd-42d5-875e-91aa5acaa54a.png)

The variable “weight” has over 95 percent missing data and will be dropped. The variables “payer_code” and “medical_specialty” have over 50 percent missing data and will be dropped from further analysis. Looking at the summary of the “factors” dataset, the medications “examide”, “citoglipton” and “glimepiride.ploglitazone” have “No” for all entries and therefore will be dropped from the dataset. The response variable “readmitted” must be binary, and therefore have only two values, to meet an assumption of logistic regression (Zach, 2020). The “readmitted” variable has three values, <30, >30 and “No”. Since this study is only concerned with readmission of less than 30 days, it will be converted so that <30 will be 1, and >30 and “No” will be 0, as seen below:
	
![image](https://user-images.githubusercontent.com/87247651/126395789-36abd8cb-ec4f-449b-a217-acd5ddcd6f80.png)
 
&ensp; The columns “diag_1”, “diag_2” and “diag_3” are the primary, secondary, and additional diagnosis of the patient using ICD9 codes. These codes are based on the World Health Organization and stand for the International Classification of Diseases (ICD-ICD–9-CM–International Classification of Diseases, Ninth Revision, 2015). Using the code below, it shows how many unique values for each column there are:
	
![image](https://user-images.githubusercontent.com/87247651/126395867-ebcc04ef-f660-46f3-8710-d79a51a661bb.png)

To make the variables usable for model making, they will be grouped into categories based on their medical code value. The categories will be “Infectious/Neoplasm”, “Immunity/Blood”, “Mental”, “Circulatory”, “Respiratory”, “Digestive”, “Pregnancy” and “Other”. The column is in character format, so first it must be changed to numeric. The code below shows the columns being changed to numerical format first:
	
![image](https://user-images.githubusercontent.com/87247651/126395903-3e897676-8b0b-46ea-841d-7b733a5669d2.png)
![image](https://user-images.githubusercontent.com/87247651/126395918-79d2f670-7fa7-4196-86dc-e0a24d8391dc.png)
![image](https://user-images.githubusercontent.com/87247651/126395930-e57402c9-435f-43eb-8c5c-9ce0a693bce1.png)
 
And now the ICD9 codes will be put into the groups mentioned above:
	
![image](https://user-images.githubusercontent.com/87247651/126395972-c6ca9413-c72f-4443-a321-f80e7bf80cfa.png)
 
Some of the ICD9 codes begin with letters. When changing the format to numeric, these values change to “NA”. These fall into the “Other” category and will be coded with 
following:
	
![image](https://user-images.githubusercontent.com/87247651/126396011-60a515a9-108b-45dc-8029-117ba4bf8c73.png)
 
The output below shows the three diagnosis columns have been changed to factors with eight categories depending on the ICD9 code. 
	
![image](https://user-images.githubusercontent.com/87247651/126396042-d99b4a14-74fc-419b-82d3-1d870d9eb309.png)
 
&ensp; The “discharge_dispostion_id” refers references where or how the patient was discharged. The code numbers 11, 13, 14, 19, 20 and 21 all refer to patients that will not be able to be readmitted due to death or hospice care. They will need to be dropped from the dataset. The code below drops the rows with those code numbers and the table command shows they are not included in the dataset.

![image](https://user-images.githubusercontent.com/87247651/126396183-dda05f74-3d85-4805-b468-be5dc9075cd1.png)
	
The variables “discharge_disposition_id”, “admission_source_id” and “admission_type_id” will be changed to categorical variables and grouped by what their number values represent. A table shows how many entries there are for each value of the “admission_type_id” variable:
	
![image](https://user-images.githubusercontent.com/87247651/126396610-90191917-9fe6-4eb0-b2c9-37144aace31b.png)
 
The code below shows the name for each category and resulting table confirms:
	
![image](https://user-images.githubusercontent.com/87247651/126396635-3b45d3e5-8b8d-4b2c-942a-bc5960fea77b.png)
 
A similar process will be done for the “discharge_disposition_id” next. First the table:
	
![image](https://user-images.githubusercontent.com/87247651/126396683-e1ce4300-78ca-424f-91ba-00954008e2b6.png)
 
Next, the code to change the values to categories and resulting table:
	
![image](https://user-images.githubusercontent.com/87247651/126396696-9f946ba5-2292-4be9-baff-17fae0daa52e.png)
 
The “admission_source_id” table is next:
	
![image](https://user-images.githubusercontent.com/87247651/126396715-e7dbb897-8ad2-4559-a91b-faad27c79cd9.png)
 
The code for categorical features and resulting table:
	
![image](https://user-images.githubusercontent.com/87247651/126396736-feedd659-7859-4fce-bae7-ec8dfca335c5.png)
 
&ensp; In the process of cleaning, the medication “metformin.rosiglitazone” was reduced to having only “No” for all answers. This variable will not be included in the final dataset. Now that the data has been cleaned, a new dataset called “cleaned_data” will be created and all character variables made into factors for further analysis as seen below:
	
![image](https://user-images.githubusercontent.com/87247651/126396785-02adf45e-4e73-428c-864c-71fec16bc9dd.png)
 
&ensp; The variables “num_procedures” and “number_emergency” are found to have a value of zero for most of their entries. It is difficult to transform these variables to meet the linearity to logit of the outcome assumption (Kassambara & U, 2018), so they will be converted to factors for further analysis. First, the “num_procedures” variable will be converted, and the results shown below:
	
![image](https://user-images.githubusercontent.com/87247651/126396828-cb23447d-0acf-45db-abaf-aed1fd7babce.png)
 
Next, the same treatment for “number_emergency”:
	
![image](https://user-images.githubusercontent.com/87247651/126396846-dadfec40-a13b-474b-81c3-2054fe72214d.png)
 
&ensp; Unlike linear regression, logistic regression does not require that there be a linear relationship between the dependent variable and the independent variables. It does, however, assume a linear relationship between numerical explanatory variables and the Logit of the dependent variable. This can be tested using the Box-Tidwell test.  The code and output below show the results for the “time_in_hospital” variable:
	
![image](https://user-images.githubusercontent.com/87247651/126396893-7ae644bc-6337-45ee-8f43-49f1ee9a3221.png)
 
The highlighted output shows p-value for the “time_in_hospital” variable and the logarithmic transformation of the variable. Since the p-value is less than or equal 0.05, it shows this is a statistically significant variable and has a nonlinear association. This variable will require transformation to meet the assumption. When performing the log transformation, it cannot handle values of zero or negative numbers. To deal with, a constant of 0.01 is added to the variable during transformation. The code below shows the transformation and the Box_Tidwell test results of the transformed variable now named “tih”:
	
![image](https://user-images.githubusercontent.com/87247651/126401742-9b31c128-c94d-45a5-9d74-da1e680fbf17.png)

The highlighted p-value shows the new variable is not statistically significant and does not show a nonlinear association. The next variable to be tested is “num_lab_procedures”, as seen below:
	
![image](https://user-images.githubusercontent.com/87247651/126401772-e38fab65-d1ca-46f7-8ba5-947da0b622ad.png)

The p-value for this variable is not statistically significant and does not show a nonlinear association, no transformation will be needed. The Box-Tidwell will now the run on the “num_medications” variable:
	
![image](https://user-images.githubusercontent.com/87247651/126401797-a9a659ca-4162-41f2-bc89-531eb2e1ac8b.png)

The p-value shows that it will need transformation. This will be done the same as the “time_in_hospital” variable, with the new variable being name “nm”:
	
![image](https://user-images.githubusercontent.com/87247651/126401833-8e220364-2361-4835-b89c-4ae12018db3a.png)

The p-value indicates it is not statistically significant. The variable “number_diagnoses” is tested next:
	
![image](https://user-images.githubusercontent.com/87247651/126401854-eb17f3d8-0993-4658-84ca-fcc4b72c2c3c.png)

Once again, the p-value show statistical significance and will require transformation. The log transformation will be applied and a new variable name “nd” will takes its place:
	
![image](https://user-images.githubusercontent.com/87247651/126401886-7c273d27-6e9b-451a-a459-c32675d3604d.png)

The transformed variable has a p-value now showing no statistical significance. The last variables to be tested, “number_outpatient” and “number_inpatient” have values such that when Box-Tidwell test is applied, it returns an error indicating there are negative or zero values:
	
![image](https://user-images.githubusercontent.com/87247651/126401906-e93939a6-b3c9-4b69-85f4-8842252d2674.png)

To work around this, a constant of 10 will be added to the variable and then the test applied:
	
![image](https://user-images.githubusercontent.com/87247651/126401929-6d54b4bd-904c-43ad-bf9d-76a5e83c4764.png)
![image](https://user-images.githubusercontent.com/87247651/126401941-eafb3eb8-1b50-41e9-881c-dda973bf5cf8.png)

The p-value shows transformation is needed, which will be applied in the same manner as the previous variables:
	
![image](https://user-images.githubusercontent.com/87247651/126401961-44d29342-16f8-4c19-96fd-b22ca5227bc4.png)

The p-value shows no statistical significance and will be used for further analysis. The last variable is the “number_outpatient” variable. The test results are shown below:
	
![image](https://user-images.githubusercontent.com/87247651/126401992-b7b9e85f-e494-4c98-b51f-a9caa3594fc4.png)

![image](https://user-images.githubusercontent.com/87247651/126402011-219d08af-c9ff-4364-9794-7236affc4906.png)

The p-value shows transformation will be required and the log transformation and results are shown:
	
![image](https://user-images.githubusercontent.com/87247651/126402047-6260f069-2ed7-49e3-b715-2f5e0dcf0b3a.png)

After transformation, the p-value shows no statistical significance, and the numerical values all hold the assumption of linearity to the Logit of the response variable.
&ensp; A new dataset with the variables that will be used for model making is created in dataset name “final_data”.
	
![image](https://user-images.githubusercontent.com/87247651/126402100-e3418f9f-834a-445a-9082-a73e4ef0ffa1.png)

---
  
# <B>Distribution of Variables<B>
  
&ensp; A Q-Q Plot shows the the distribution of the numeric variables. It does so by plotting the quantiles of a normal distribion with the quanitiles of the numeric variables. 

The “num_lab_procedure” variable was not transformed, and the QQ-plot shows the distribution:
	
![image](https://user-images.githubusercontent.com/87247651/126400331-d236fe01-f33f-4a2b-94ec-545db1902eb7.png)

QQ-Plots of the transformed variables show the distribution:
	
![image](https://user-images.githubusercontent.com/87247651/126400364-b8b3946b-f7f1-41f6-8fe2-07f2e0b86e26.png)
![image](https://user-images.githubusercontent.com/87247651/126400378-33dbf538-23a0-49a7-b6da-6fa72ed2ffdd.png)

Graphs showing the categorical features has been included to show the amount of each group associated with the variables.
	
![image](https://user-images.githubusercontent.com/87247651/126400419-53d4ab44-4814-4464-8105-87f01dfc5f9d.png)
![image](https://user-images.githubusercontent.com/87247651/126400428-623e3d3a-5e25-4e5f-ab71-d23d81315474.png)
![image](https://user-images.githubusercontent.com/87247651/126400440-e868df08-3b60-4903-b893-424b1a014870.png)
![image](https://user-images.githubusercontent.com/87247651/126400451-6bd2a1db-3fa9-4045-add5-7bb8e10f243f.png)
![image](https://user-images.githubusercontent.com/87247651/126400468-d37209f3-49de-45ec-8a2a-55aad7834e98.png)
![image](https://user-images.githubusercontent.com/87247651/126400484-fed23125-2053-442c-885b-7c710cadcdbc.png)
![image](https://user-images.githubusercontent.com/87247651/126400495-d3950d87-9b92-49fa-be01-967276c79e53.png)
![image](https://user-images.githubusercontent.com/87247651/126400520-a0665873-f680-4bb6-a6fd-580c530513cf.png)
![image](https://user-images.githubusercontent.com/87247651/126400529-e7287cec-459d-4002-b35a-6a3b551d8b21.png)

The final graph shows the Readmission rate:
	
![image](https://user-images.githubusercontent.com/87247651/126400571-1caf0977-5620-44ee-967c-ddf81b0b87d9.png)
	
---
  
# <B>Feature Selection<B>
	
&ensp; Feature selection is an important part of model building and testing. Not every variable is needed for model training and using features that are non-redundant can improve your model (Boruta Feature Selection in R, n.d.). The Boruta algorithm is a wrapper method, used with the random forest algorithm to capture all the most important and interesting features from the dataset. Wrapper methods use a subset of features to train a model, with Forward Selection and Backward Elimination being two examples of wrapper methods (Boruta Feature Selection R, n.d). The drawback to using Boruta is it can be time consuming. The mores variables a dataset has, the longer the Boruta algorithm takes, as it goes through its iterations trying to find the most important features. The longer wait will be worth it when a model can be made with fewer variables without losing accuracy. The code below shows the Boruta algorithm being used on the training dataset.
	
![image](https://user-images.githubusercontent.com/87247651/126400784-3c1bcbbd-00e6-41c1-aa67-39137cc3d6c9.png)

After the algorithm is finished, a plot of the variables shows which variables are most important, in green:
	
![image](https://user-images.githubusercontent.com/87247651/126400820-fa12dc0f-ae56-4465-8e99-7a6b67ba9bae.png)

The variable “gender” is shown in yellow, meaning Boruta has classified it as tentative. The tentative variable can be verified as rejected or confirmed and a new plot created with the code below:
	
![image](https://user-images.githubusercontent.com/87247651/126400850-ddfeab7b-6461-4bb4-bee2-f8cae5b115af.png)

The new plot shows all variables that are confirmed or rejected:
	
![image](https://user-images.githubusercontent.com/87247651/126400874-ba5ad958-9c1c-4033-b1b8-0ced8100da5b.png)

![image](https://user-images.githubusercontent.com/87247651/126400881-43d1518a-2199-4645-8b41-88a75ca2ba40.png)

Using the attStats command gives a table with the statistics and decisions for each variable:
	
![image](https://user-images.githubusercontent.com/87247651/126400920-cb198bb9-bf60-42bf-850c-cf3da6fa2edd.png)

The “meanImp” scores shows which variables Boruta deems the most important. The decision column makes it easy to understand which variables to keep for further analysis.
Using the code below, the model formula is shown with the confirmed important attributes:

![image](https://user-images.githubusercontent.com/87247651/126400949-8d343da0-856d-4ba2-a0d0-2928e50f103b.png)

In logistic regression, the absence of multicollinearity is an important issue, and the problem variable or variables should be removed (Kassambara & U, 2018). A logistic regression model is created to check the Variance Inflation Factor, which measures the amount of multicollinearity.
	
![image](https://user-images.githubusercontent.com/87247651/126400994-66089c93-1f28-48fe-9bd0-871bfdf95bb4.png)

The VIF can be checked with vif() command in R:
	
![image](https://user-images.githubusercontent.com/87247651/126401021-0799f160-0f3e-4129-8772-14f07c6647b0.png)

This error encountered shows there are variables with perfect multicollinearity. This means two or more independent variables are perfectly predicted with no randomness (Perfect Multicollinearity and Your Economic Model, n.d.). To find out what variables are highly correlated, a correlation plot of all numerical variables is created, as seen below:
	
![image](https://user-images.githubusercontent.com/87247651/126401052-385f60bd-80b2-44a3-aebb-e32a276809a5.png)

The correlation plot shows perfect collinearity between the log transformed variables of “number_inpatient” and “number_outpatient”. One of these variables will have to be removed. For this study, the “number_outpatient” transformed variable “no” is removed. The model is recreated and VIF function run again, as seen below:
	
![image](https://user-images.githubusercontent.com/87247651/126401084-a3a2eb27-992d-4299-9967-57c6187e0049.png)

The Variance Inflation scores now show there is no high correlations present in the model.

---
  
# <B>Class Imbalance<B>
	
&ensp; Class imbalance in logistic regression can cause results to be biased toward the majority class when modeling (Procrastinator, 2021). The table below shows the imbalance present in the dataset regarding the response variable:
	
![image](https://user-images.githubusercontent.com/87247651/126401334-f6c8209b-450c-4fef-bc94-b6aa840e20c8.png)

There are many different options available to help with class imbalance, but this study will utilize the ROSE package in R. ROSE stands for Random Over Sampling Examples. This method generates artificial data based on resampling and bootstrapping (Analytics Vidhya This is the official account of the Analytics Vidhya team., 2020). The code below shows the model being created using the training dataset and ROSE function:
	
![image](https://user-images.githubusercontent.com/87247651/126401370-51b9d5e9-eb57-4684-912b-51c9feef312d.png)

A table shows the response variable using the ROSE method is more evenly distributed:
	
![image](https://user-images.githubusercontent.com/87247651/126401400-1b5f0f5f-8892-48dd-a312-4d65f944ab68.png)

A predictive model is now created using the Random Forest and the “rose” dataset:
	
![image](https://user-images.githubusercontent.com/87247651/126401428-6f2e5a10-f295-41d5-90e6-f5a04260fee8.png)

A confusion matrix shows the predictive capabilities of the model:
	
![image](https://user-images.githubusercontent.com/87247651/126401469-78e356da-1b80-4bb7-86f5-05761964662c.png)

---
  
# <B>Summary and Implications<B>

&ensp; The confusion matrix shows the model to be 87.5% percent accurate. The Specificity of the model is 95%. Specificity for this model is how well it predicts a patient not being readmitted with 30 days, and the actual number of patients to not be readmitted. The matrix shows that out of 20,415 cases, the model predicts 17,695 patients will not be readmitted. The model incorrectly predicted 880 patients would not be readmitted. The Sensitivity of the model is 10%. The model correctly predicted 183 patients would be readmitted within the 30-day period, but incorrectly predicted 1657 patients would be readmitted in the 30-day time frame. 
&ensp; This model does an excellent job at predicting the non-readmission of the patients in this diabetic dataset. It would be difficult to use this model to predict a patient that would be readmitted with the given dataset. If more information from patients who were readmitted could be obtained, a model with higher accuracy could be created to determine a positive readmission of a patient. 
&ensp; In looking at the Boruta plot, the most important features are the number of medications and the amount of time in the hospital for each patient. If these features could be more closely studied and any possible correlations found, a new action plan could be created to help reduce readmissions and recognize signs that lead to readmission. 
&ensp; There are many ICD9 diagnoses involved with this study. They were grouped in a way that made them easier to model. The conditions involved with these patients could be studied more closely and grouped in a way that would possibly show a relationship between types of diagnoses and future readmission.

---
  
# <B>References<B>
	
The Cost of Diabetes. The Cost of Diabetes | ADA. (n.d.). https://www.diabetes.org/resources/statistics/cost-diabetes. 

Staff, D. T. N. (2018, April 23). Cost Of Diabetes Readmission. diabetestalk.net. https://diabetestalk.net/diabetes/cost-of-diabetes-readmission.

Kassambara, & U, M. (2018, March 11). Logistic Regression Assumptions and Diagnostics in R. STHDA. http://www.sthda.com/english/articles/36-classification-methods-essentials/148-logistic-regression-assumptions-and-diagnostics-in-r/#:~:text=Logistic%20regression%20assumptions%20The%20logistic%20regression%20method%20assumes,logit%20of%20the%20outcome%20and%20each%20predictor%20variables. 

Nicola Lunardon, G. M. (2019, May 2). ROSE-package: ROSE: Random Over-Sampling Examples in ROSE: ROSE: Random Over-Sampling Examples. https://rdrr.io/cran/ROSE/man/ROSE-package.html. 

Zach. (2020, October 13). The 6 Assumptions of Logistic Regression (With Examples). Statology. https://www.statology.org/assumptions-of-logistic-regression/. 

Centers for Disease Control and Prevention. (2015, November 6). ICD - ICD-9-CM - International Classification of Diseases, Ninth Revision, Clinical Modification. Centers for Disease Control and Prevention. https://www.cdc.gov/nchs/icd/icd9cm.htm. 

Procrastinator. (2021, January 6). How To Dealing With Imbalanced Classes in Machine Learning. Analytics Vidhya. https://www.analyticsvidhya.com/blog/2020/10/improve-class-imbalance-class-weights/#:~:text=What%20is%20Class%20Imbalance%3F%20Class%20imbalance%20is%20a,very%20high%20compared%20to%20the%20other%20classes%20present

Boruta Feature Selection in R. DataCamp Community. (n.d.). https://www.datacamp.com/community/tutorials/feature-selection-R-boruta. 

Analytics VidhyaThis is the official account of the Analytics Vidhya team. (2020, July 5). Imbalanced Classification Problems in R. Analytics Vidhya. https://www.analyticsvidhya.com/blog/2016/03/practical-guide-deal-imbalanced-classification-problems/#:~:text=In%20R%2C%20packages%20such%20as%20ROSE%20and%20DMwR,based%20on%20sampling%20methods%20and%20smoothed%20bootstrap%20approach. 

Kassambara, & U, M. (2018, March 11). Logistic Regression Assumptions and Diagnostics in R. STHDA. http://www.sthda.com/english/articles/36-classification-methods-essentials/148-logistic-regression-assumptions-and-diagnostics-in-r/#multicollinearity-logistic-regression. 

Perfect Multicollinearity and Your Econometric Model. dummies. (n.d.). https://www.dummies.com/education/economics/econometrics/perfect-multicollinearity-and-your-econometric-model/. 

Boruta Feature Selection in R. DataCamp Community. (n.d.). https://www.datacamp.com/community/tutorials/feature-selection-R-boruta. 

  
---
