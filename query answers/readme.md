CREATE TABLE student_migration (
    student_id VARCHAR(10) PRIMARY KEY,
    origin_country VARCHAR(100),
    destination_country VARCHAR(100),
    destination_city VARCHAR(100),
    university_name VARCHAR(255),
    course_name VARCHAR(100),
    field_of_study VARCHAR(100),
    year_of_enrollment INT,
    scholarship_received VARCHAR(10),
    enrollment_reason VARCHAR(100),
    graduation_year INT,
    placement_status VARCHAR(20),
    placement_country VARCHAR(100),
    placement_company VARCHAR(100),
    starting_salary_usd INT,
    gpa_or_score FLOAT,
    visa_status VARCHAR(100),
    post_graduation_visa VARCHAR(100),
    language_proficiency_test VARCHAR(50),
    test_score FLOAT
);
select * from student_migration;
select count(*) from student_migration
---------1. Total number of students from each origin country------
select origin_country,count(*) as total_students 
from student_migration
group by origin_country 
order by total_students desc ;
---------2. Top 5 destination countries for students-------
select destination_country,count(*) as students 
from student_migration 
group by destination_country
order by students desc 
limit 5;
---------3. How many students received scholarships?-------------
select scholarship_received,count(*) as received
from student_migration
group by scholarship_received 
order by received desc;  
----------4. Average starting salary of placed students by field of study-----------
SELECT field_of_study, round(AVG(starting_salary_usd)) AS avg_salary
FROM student_migration
WHERE placement_status = 'Placed'
GROUP BY field_of_study
ORDER BY avg_salary DESC;
-------------5. Count of students placed vs not placed-----------
select placement_status,count(*) as student
from student_migration 
group by placement_status
-------6. Average GPA and test score by visa type---------
select round(avg(gpa_or_score)) as avg_gpa,round(avg(test_score)) as avg_test,visa_status
from student_migration group by visa_status
---7. Top universities with the most enrollments------------
select university_name,count(*) as stu_enrolled 
from student_migration
group by university_name
limit 10;
----8. Students enrolled between 2020 and 2023----------
SELECT COUNT(*) AS count
FROM student_migration
WHERE year_of_enrollment BETWEEN 2020 AND 2023;
------9. Placement rate by destination country----------
SELECT destination_country,
       COUNT(*) AS total_students,
       SUM(CASE WHEN placement_status = 'Placed' THEN 1 ELSE 0 END) AS placed_students,
       ROUND(SUM(CASE WHEN placement_status = 'Placed' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS placement_rate
FROM student_migration
GROUP BY destination_country
ORDER BY placement_rate DESC;

-----10. Most popular reasons for enrollment----------
select enrollment_reason,count(*) as count_reason 
from student_migration 
group by enrollment_reason order by count_reason desc
-------- 11. Which language proficiency tests are most commonly taken?----------
select language_proficiency_test,count(*) as total_count
from student_migration where language_proficiency_test is not null and language_proficiency_test != 'None'
group by language_proficiency_test
order by total_count desc
--- 12. Which courses have the highest placement rate?--------
SELECT course_name,
       COUNT(*) AS total_students,
       SUM(CASE WHEN placement_status = 'Placed' THEN 1 ELSE 0 END) AS placed_students,
       ROUND(SUM(CASE WHEN placement_status = 'Placed' THEN 1 ELSE 0 END) * 100.0 / COUNT(*), 2) AS placement_rate
FROM student_migration
GROUP BY course_name
ORDER BY placement_rate DESC
LIMIT 10;
---------13. What is the average GPA for students who received a scholarship?--------
select scholarship_received,avg(gpa_or_score) as avg_gpa
from student_migration
group by scholarship_received
------14. List universities that placed students in the USA--------
select university_name,count(*) as counted
from student_migration
where placement_country='USA'
group by university_name
------15. How many students migrated to each city for education?----------
select destination_city,
count(*) as students
from student_migration 
group by destination_city
order by students desc
limit 10
