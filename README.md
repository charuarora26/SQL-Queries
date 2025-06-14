# SQL Practice from https://www.sql-practice.com/

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
   
   
