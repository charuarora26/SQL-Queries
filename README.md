# SQL Practice from https://www.sql-practice.com/

## Schema
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
