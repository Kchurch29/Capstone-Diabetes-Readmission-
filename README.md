# Capstone-Diabetes-Readmission-
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
	Studies have shown that 15-25 percent of diabetic patients will have unplanned readmissions within 30 days, with many of these being preventable (Staff, 2018).  By examining a patient’s readmission history as it relates to diabetes treatment, an effective plan to reduce readmissions can reduce costs and lead to a better quality of life for the patient. Using patient’s demographic, medicinal intake, diagnosis status, and admission information, a model can predict a patient being readmitted in the first 30 days after release.

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
	


	

---
  
# <B>Distribution of Variables<B>
  
&ensp; A Q-Q Plot shows the the distribution of the numeric variables. It does so by plotting the quantiles of a normal distribion with the quanitiles of the numeric variables. The blue line on the graphs show perfect distribution with the actual values being in black. The closer the black line is to the blue the more perfect the distribution.
	
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
  
---
  
# <B>Class Imbalance<B>
  
---
  
# <B>Summary and Implications<B>
  
---
  
# <B>References<B>
  
---
