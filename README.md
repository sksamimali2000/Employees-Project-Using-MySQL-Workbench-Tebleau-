# 📊 Employee Data Analytics Project (MySQL + Tableau)

![Tableau Dashboard](Tableau%20Dashboard.jpg) 
## 📌 Project Overview
This project analyzes the **Employees dataset** using **MySQL Workbench** and visualizes insights with **Tableau**.  
The goal is to answer business questions related to **employee growth, salaries, department managers, and gender distribution** over time.  

✅ Database created with **37975+ employee records**  
✅ SQL queries written for **business insights**  
✅ Interactive dashboards designed in **Tableau**  

---

## 🗄 Database Schema

The database `employees_mod` consists of 5 core tables:  

- **t_employees** → Employee details (emp_no, name, gender, hire_date)  
- **t_departments** → Department information  
- **t_dept_manager** → Department managers over time  
- **t_dept_emp** → Department assignments of employees  
- **t_salaries** → Employee salary history  

<p align="center">
  <img src="Employees Database Schema.jpg" width="600"/>
</p>

---

## 🚀 Business Tasks & SQL Solutions  

### **Task 1: Employee Growth by Gender (1990 onwards)**
📌 *Objective*: Analyze the breakdown of male vs female employees over the years.  
🔑 *Insight*: The proportion of male employees remained higher (~60%), though female representation grew steadily over time.  

***```
SELECT YEAR(d.from_date) AS calendar_year,
       e.gender,    
       COUNT(e.emp_no) AS num_of_employees
FROM t_employees e
JOIN t_dept_emp d ON d.emp_no = e.emp_no
GROUP BY calendar_year , e.gender 
HAVING calendar_year >= 1990;```***

---

## 📈 Task 1: Breakdown of Male and Female Employees Over Years  

![Gender Breakdown](Task%201%20Breakdown%20between%20male%20and%20female%20employees%20over%20years.jpg)  
Task 2: Active Managers per Department
📌 Objective: Track department managers across years.
🔑 Insight: Some departments had overlapping managers; peak active managers observed around 1995–1997.

***```
SELECT d.dept_name, ee.gender, dm.emp_no, dm.from_date, dm.to_date, e.calendar_year,
       CASE WHEN YEAR(dm.to_date) >= e.calendar_year 
                 AND YEAR(dm.from_date) <= e.calendar_year THEN 1 ELSE 0 END AS active
FROM (SELECT YEAR(hire_date) AS calendar_year
      FROM t_employees GROUP BY calendar_year) e
CROSS JOIN t_dept_manager dm
JOIN t_departments d ON dm.dept_no = d.dept_no
JOIN t_employees ee ON dm.emp_no = ee.emp_no
ORDER BY dm.emp_no, calendar_year;```***

Task 3: Average Salary by Gender & Department
📌 Objective: Compare salaries across genders and departments.
🔑 Insight: Salaries were fairly consistent between genders, with slight variations across departments.

***```
Copy code
SELECT e.gender, d.dept_name,
       ROUND(AVG(s.salary), 2) AS salary,
       YEAR(s.from_date) AS calendar_year
FROM t_salaries s
JOIN t_employees e ON s.emp_no = e.emp_no
JOIN t_dept_emp de ON de.emp_no = e.emp_no
JOIN t_departments d ON d.dept_no = de.dept_no
GROUP BY d.dept_no , e.gender , calendar_year
HAVING calendar_year <= 2002
ORDER BY d.dept_no;```***


Task 4: Salary Filter Stored Procedure
📌 Objective: Create a stored procedure to filter employees based on salary range.
🔑 Insight: Dynamic salary filtering helps HR & analysts evaluate pay distribution across departments.

***```
CREATE PROCEDURE filter_salary (IN p_min_salary FLOAT, IN p_max_salary FLOAT)
BEGIN
    SELECT e.gender, d.dept_name, AVG(s.salary) as avg_salary
    FROM t_salaries s
    JOIN t_employees e ON s.emp_no = e.emp_no
    JOIN t_dept_emp de ON de.emp_no = e.emp_no
    JOIN t_departments d ON d.dept_no = de.dept_no
    WHERE s.salary BETWEEN p_min_salary AND p_max_salary
    GROUP BY d.dept_no, e.gender;
END;
CALL filter_salary(50000, 90000);```***



📊 Tableau Dashboard
The SQL outputs were visualized in Tableau to create an interactive dashboard:

📌 Employee growth by gender

📌 Average annual salary trend

📌 Number of managers per department

📌 Salary comparison across departments & gender

<p align="center"> <img src="Tableau Dashboard.jpg" width="700"/> </p>
🔑 Key Business Insights
1️⃣ Employee Growth:

Workforce expanded steadily from 1990–2002.

Male employees remained dominant (~60%), though female representation grew consistently.

2️⃣ Salaries:

Average salaries increased gradually, crossing $60K+ by 2002.

Salary gaps between genders were minimal overall.

3️⃣ Managers:

The number of department managers peaked during mid-90s.

Departments like Development & Sales had the largest manager activity.

4️⃣ Department Insights:

Salaries varied slightly by department, but Finance & Development offered higher averages.

🛠 Tools & Technologies
Database: MySQL Workbench

Visualization: Tableau

Languages: SQL

Dataset: Employees (custom-modified ~37K records)

🌟 Project Value
This project demonstrates:
✔ SQL skills for data extraction & business logic
✔ Ability to design data pipelines & stored procedures
✔ Data visualization & storytelling with Tableau dashboards
✔ Real-world HR & Workforce analytics use case

📬 Contact
👨‍💻 SK Samim Ali

📧 roy871858@gmail.com
