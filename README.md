# ⭐ Yelp Reviews: Sentiment Analysis & SQL Challenge

An end-to-end data engineering and analytics project that processes large-scale Yelp review data using AWS S3, Snowflake, and Python. This project showcases JSON flattening, cloud integration, and sentiment classification using Python UDFs inside Snowflake.

---

## 🚀 Project Overview

This project demonstrates a real-world enterprise-grade pipeline involving:

- ✅ Big data handling (5+ GB JSON)
- ☁️ AWS S3 cloud storage
- ❄️ Snowflake data warehousing
- 🐍 Python UDFs for sentiment classification
- 📊 SQL-based business insights from review data

---

## 🔧 Tech Stack

| Tool             | Purpose                                     |
|------------------|---------------------------------------------|
| Python           | Preprocessing large JSON, scripting         |
| AWS S3           | Cloud-based storage for intermediate files  |
| Snowflake        | Data warehouse & SQL processing             |
| Python UDFs      | Sentiment analysis within Snowflake         |
| SQL (Snowflake)  | Data flattening and business querying       |
| Jupyter Notebook | Exploratory scripting and orchestration     |

---

## 📂 Project Workflow

### 1. JSON File Handling
- Dealt with a **massive raw JSON** file (~5 GB)
- Split into **manageable chunks** using Python to improve loading performance

- [File Link with code for splitting the file](https://github.com/charuarora26/SQL-Queries/blob/main/Json%20File%20break.ipynb)


### 2. Upload to Cloud (S3)
- Stored the split and cleaned files in an **AWS S3 bucket**
- Enabled seamless Snowflake integration using external stage

- <img width="1674" alt="AWS" src="https://github.com/user-attachments/assets/07ddd50e-2f1f-4f81-8908-8a7b28d54f50" />


### 3. Snowflake Integration
- Ingested the raw JSON into **variant columns**
- Applied **FLATTEN** operations to convert nested objects into structured tabular format


### 4. Sentiment Analysis with Python UDF
- Created a **Snowflake Python UDF** that performs sentiment analysis on review text
- Used a simple rule-based or pretrained model to classify reviews as **positive/negative**

- <img width="843" alt="Sentiment" src="https://github.com/user-attachments/assets/19c05ed7-5e49-4a7b-9e49-7b30d44e0ec6" />


### 5. SQL Analysis & Reporting
- Solved **Some SQL challenges** involving:
  - Ranking most-reviewed businesses
  - Aggregating sentiment trends by city
  - Filtering reviews based on scores and content
  - Identifying top-performing categories and subcategories

<img width="962" alt="SQLQueries1" src="https://github.com/user-attachments/assets/eda1eb04-8c70-47d1-aaf2-ba14c0674bec" />

<img width="1919" alt="SQLQueries2" src="https://github.com/user-attachments/assets/922942ee-544a-4c76-acbf-620a5177bb70" />

---

## 📊 Sample Business Insights

| Question                                 | Example Result                      |
|------------------------------------------|-------------------------------------|
| Top City by Positive Reviews             | Las Vegas                           |
| Business with Most 5-Star Reviews        | "Joe’s Pizza"                       |
| Category with Highest Sentiment Avg      | Spa & Wellness                      |
| Negative Review Trend by Year            | Increasing from 2020 to 2022        |

---

## 💡 Key Takeaways

- **Big Data Readiness**: Handled multi-GB JSON files through smart chunking  
- **Cloud-Native Workflow**: Integrated AWS and Snowflake efficiently  
- **Python in SQL**: Embedded UDFs for scalable sentiment analysis  
- **Business Insights**: Used SQL to answer real-world analytical questions on user sentiment and business performance

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



# Some more SQL Practice from https://www.sql-practice.com/

## Schema 1
![image](https://github.com/user-attachments/assets/f3aa30a0-ce82-48e6-9a62-bdd88c193066)

### Queries 
1. **Show the ProductName, CompanyName, CategoryName from the products, suppliers, and categories table**
   
   **Solution**
   
   Select c.category_name as category_name , d.product_name as product_name, d.company_name as company_name from
categories c left join 
(select a.product_name, b.company_name, a.category_id from products a left join suppliers b on
a.supplier_id = b.supplier_id) d 
on c.category_id = d.category_id

2. **Show the category_name and the average product unit price for each category rounded to 2 decimal places.**
   
    **Solution**
   
   Select c.category_name, round(avg(p.unit_price), 2) from
categories c left join products p 
on c.category_id = p.category_id
group by c.category_name

3. **Show the city, company_name, contact_name from the customers and suppliers table merged togethe. Create a column which contains 'customers' or 'suppliers' depending on the table it came from.**

   
    **Solution** -

   Select city, company_name, contact_name, "Customers" as relatioship from
customers union all
select city, company_name, contact_name, "Suppliers" from suppliers

4. **Show the total amount of orders for each year/month.**

   **Solution** -

   Select year(order_date), month(order_date), count(*) from orders
group by 1, 2


---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



## Schema 2
![image](https://github.com/user-attachments/assets/4fde9913-e2e0-497a-b079-b7fe7ad4f7e0)


### Queries 
1. **Show unique birth years from patients and order them by ascending.**
   
   **Solution**
   
   Select distinct year(birth_date) as birth_year from patients
order by birth_year

2. **Show unique first names from the patients table which only occurs once in the list. For example, if two or more people are named 'John' in the first_name column then don't include their name in the output list. If only 1 person is named 'Leo' then include them in the output.**
   
    **Solution**
   
   Select first_name from (select first_name, count(*) as freq from patients group by first_name)
 where freq < 2

3. **Show patient_id and first_name from patients where their first_name start and ends with 's' and is at least 6 characters long.**

    **Solution**
   
   Select patient_id, first_name from patients where first_name ilike 's%s' and len(first_name) >= 6

4. **Show patient_id, first_name, last_name from patients whos diagnosis is 'Dementia'.Primary diagnosis is stored in the admissions table.**

    **Solution**
   
   SELECT a.patient_id, a.first_name, a.last_name FROM patients a left join admissions b
on a.patient_id = b.patient_id
where b.diagnosis = 'Dementia'

5. **Display every patient's first_name. Order the list by the length of each name and then by alphabetically.**

    **Solution**
   
   Select first_name from patients
order by len(first_name) asc, first_name asc

6. **Show the total amount of male patients and the total amount of female patients in the patients table. Display the two results in the same row.**

    **Solution**
   
7. **Show first and last name, allergies from patients which have allergies to either 'Penicillin' or 'Morphine'. Show results ordered ascending by allergies then by first_name then by last_name.**

    **Solution**
   
   select first_name, last_name, allergies from patients
where allergies = 'Penicillin' or 
allergies = 'Morphine'
order by 3,1,2


8. **Show patient_id, diagnosis from admissions. Find patients admitted multiple times for the same diagnosis.**

    **Solution**
   
   Select patient_id, diagnosis from 
(Select patient_id, diagnosis, count(*) as times_admitted from admissions group by 1,2 
having times_admitted > 1)


9. **Show the city and the total number of patients in the city. Order from most to least patients and then by city name ascending.**

    **Solution**
   
   Select city, count(patient_id) from 
patients group by city
order by 2 desc, 1


10. **Show first name, last name and role of every person that is either patient or doctor. The roles are either "Patient" or "Doctor"**

    **Solution**
   
   Select first_name, last_name, "Patient" as role from patients 
union all
select first_name, last_name, "Doctor" from doctors


11. **Show all allergies ordered by popularity. Remove NULL values from query.**

    **Solution**
   
   Select allergies, count(patient_id) as total_diagnosis from 
patients 
where allergies not Null
group by 1
order by 2 desc


12. **Show all patient's first_name, last_name, and birth_date who were born in the 1970s decade. Sort the list starting from the earliest birth_date.**

    **Solution**
   
   Select first_name, last_name, birth_date from patients
where year(birth_date) like "197%"
order by birth_date asc


13. **We want to display each patient's full name in a single column. Their last_name in all upper letters must appear first, then first_name in all lower case letters. Separate the last_name and first_name with a comma. Order the list by the first_name in decending order
EX: SMITH,jane**

    **Solution**
   
   Select concat(upper(last_name), ",", lower(first_name)) as new_name_format
from patients
order by first_name desc


14. **Show the province_id(s), sum of height; where the total sum of its patient's height is greater than or equal to 7,000..**

    **Solution**
   
   Select province_id, sum(height) as sum_height
from patients
group by province_id
having sum_height> 7000


15. **Show the difference between the largest weight and smallest weight for patients with the last name 'Maroni'**

    **Solution**
   
   Select (max(weight) - min(weight)) as weight_delta 
from patients where last_name = 'Maroni'


16. **Show all of the days of the month (1-31) and how many admission_dates occurred on that day. Sort by the day with most admissions to least admissions.**

    **Solution**
   
   Select day(admission_date), count(patient_id) from admissions
group by 1 
order by 2 desc


17. **Show all columns for patient_id 542's most recent admission_date.**

    **Solution**
   
   Select * from admissions
where patient_id = 542 and 
admission_date = (select max(admission_date) from admissions where patient_id = 542)


18. **Show patient_id, attending_doctor_id, and diagnosis for admissions that match one of the two criteria: 1. patient_id is an odd number and attending_doctor_id is either 1, 5, or 19. 2. attending_doctor_id contains a 2 and the length of patient_id is 3 characters.**

    **Solution**
   
   Select patient_id, attending_doctor_id, diagnosis
from admissions
where (patient_id % 2 != 0 and attending_doctor_id in (1,5,19))
or (attending_doctor_id like "%2%" and len(patient_id) = 3)


19. **SShow first_name, last_name, and the total number of admissions attended for each doctor. Every admission has been attended by a doctor.**

    **Solution**
   
   Select d.first_name,d.last_name, count(a.patient_id)
from admissions a left join doctors d 
on a.attending_doctor_id = d.doctor_id
group by 1,2


20. **For each doctor, display their id, full name, and the first and last admission date they attended.**

    **Solution**
   
   Select  d.doctor_id,concat(d.first_name, " ", d.last_name) as full_name, min(admission_date), max(admission_date) from doctors d
left join admissions a 
where d.doctor_id = a.attending_doctor_id
group by 1


21. **Display the total amount of patients for each province. Order by descending.**

    **Solution**
   
   Select pr.province_name, count(pt.patient_id)
from province_names pr inner join patients pt
on pr.province_id = pt.province_id
group by 1
order by 2 desc


22. **For every admission, display the patient's full name, their admission diagnosis, and their doctor's full name who diagnosed their problem.**

    **Solution**
   
   Select concat(p.first_name, " ", p.last_name), a.diagnosis, concat(d.first_name, " ", d.last_name)
from patients p join admissions a join doctors d
on p.patient_id = a.patient_id
and a.attending_doctor_id = d.doctor_id


23. **Display the first name, last name and number of duplicate patients based on their first name and last name. Ex: A patient with an identical name can be considered a duplicate.**

    **Solution**
   
   Select first_name, last_name, count(patient_id)
from patients
group by 1,2
having count(patient_id) > 1


24. **Display patient's full name, height in the units feet rounded to 1 decimal, weight in the unit pounds rounded to 0 decimals, birth_date, gender non abbreviated.
 Convert CM to feet by dividing by 30.48. Convert KG to pounds by multiplying by 2.205.**

    **Solution**
   
   Select concat(first_name," ", last_name) as patient_name, 
round(height/30.48, 1), round(weight*2.205, 0),
birth_date, case when gender = 'M' then "MALE" else "FEMALE" End as gender_type
from patients


25. **Show patient_id, first_name, last_name from patients whose does not have any records in the admissions table. (Their patient_id does not exist in any admissions.patient_id rows.)**

    **Solution**
   
   Select p.patient_id, p.first_name, p.last_name from patients p
left join admissions a on
p.patient_id = a.patient_id
where a.patient_id is null


26. **Display a single row with max_visits, min_visits, average_visits where the maximum, minimum and average number of admissions per day is calculated. Average is rounded to 2 decimal places.**

    **Solution**
   
   Select max(visits) as max_visits, min(visits) as min_visits , round(avg(visits),2) as average_visits from
(select admission_date, count(patient_id) as visits
 from admissions
 group by 1)


