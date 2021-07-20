# Capstone-Diabetes-Readmission-
Predictive Model for Diabetes Readmission

---

# <B>Table of Contents<B>
  
---
  
1. [Research Question]
2. [Data Collection]
3. [Data Dictionary]
4. [Data Extraction and Preparation]
5. [Distribution of Variables]
6. [Feature Selection]
7. [Class Imbalance]
8. [Summary and Implications]
9. [References]
  
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
 
One of the assumptions of logistic regression is independence of observations (Kassambara & U, 2018), meaning none of the observations should be related to another. Looking at the data, there are multiple records for some patients as they have more than one visit. To keep in line with the independence of observarions assumption the data will be sorted by “patient_nbr”, so that the first record for patients with multiple visits will be kept for further analysis.
 
The figure above shows the data ordered by “patient_nbr”. The figure below shows a new dataset name “first_patient” being created that only takes the first record for each patient if there are more than one record. This will be the new dataset that variable modifications will be done on.
 
Now that the dataset has the only patient records that will be used, the str() command shows what data type each variable is :
 
To be able to clearly see what variables need to be dropped or altered, a temporary dataset name “factors” will be created and analyzed to see what variables should be dropped or altered.  Once the temporary dataset is created, the variables that are of the character data type will be converted to factors. A str() command confirms the data has been changed to factors. This is done in the screenshot seen below:
 
Using the summary command, the statistics for each variable can been observed to see what further action to take. 
 
The figure above shows “race” as having 1948 missing values, and “gender” having 3, so those entries will not be included. The code below shows the process of removing these on the “first_patient” dataset:
 
 The variable “weight” has over 95 percent missing data and will be dropped. The variables “payer_code” and “medical_specialty” have over 50 percent missing data and will be dropped from further analysis. Looking at the summary of the “factors” dataset, the medications “examide”, “citoglipton” and “glimepiride.ploglitazone” have “No” for all entries and therefore will be dropped from the dataset. The response variable “readmitted” must be binary, and therefore have only two values, to meet an assumption of logistic regression (Zach, 2020). The “readmitted” variable has three values, <30, >30 and “No”. Since this study is only concerned with readmission of less than 30 days, it will be converted so that <30 will be 1, and >30 and “No” will be 0, as seen below:
 
The columns “diag_1”, “diag_2” and “diag_3” are the primary, secondary, and additional diagnosis of the patient using ICD9 codes. These codes are based on the World Health Organization and stand for the International Classification of Diseases (ICD-ICD–9-CM–International Classification of Diseases, Ninth Revision, 2015). Using the code below, it shows how many unique values for each column there are:
 
To make the variables usable for model making, they will be grouped into categories based on their medical code value. The categories will be “Infectious/Neoplasm”, “Immunity/Blood”, “Mental”, “Circulatory”, “Respiratory”, “Digestive”, “Pregnancy” and “Other”. The column is in character format, so first it must be changed to numeric. The code below shows the columns being changed to numerical format first:
 
 
 
And now the ICD9 codes will be put into the groups mentioned above:
 
Some of the ICD9 codes begin with letters. When changing the format to numeric, these values change to “NA”. These fall into the “Other” category and will be coded with 
following:
 
The output below shows the three diagnosis columns have been changed to factors with eight categories depending on the ICD9 code. 
 
	The “discharge_dispostion_id” refers references where or how the patient was discharged. The code numbers 11, 13, 14, 19, 20 and 21 all refer to patients that will not be able to be readmitted due to death or hospice care. They will need to be dropped from the dataset. The code below drops the rows with those code numbers and the table command shows they are not included in the dataset.

---
  
# <B>Distribution of Variables<B>
  
---
  
# <B>Feature Selection<B>
  
---
  
# <B>Class Imbalance<B>
  
---
  
# <B>Summary and Implications<B>
  
---
  
# <B>References<B>
  
---
