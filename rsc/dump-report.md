-- 1. List all students
SELECT * FROM students;

-- 2. Show students and their departments
SELECT s.full_name AS "full name", d.name AS"department name"  FROM students s 
INNER JOIN departments d ON s.department_id=d.id;

-- 3. Count students by status
SELECT status, COUNT(id) FROM students GROUP BY status;

-- 4. List courses with department name
SELECT c.title AS "Course", d.name AS "Department name" 
FROM courses c INNER JOIN departments d ON c.department_id=d.id

-- 5. Show enrollments with student and course
SELECT c.title AS "Course", full_name AS "Student name", final_status AS "Enrollment status"  
FROM enrollments e INNER JOIN students s ON e.student_id=s.id 
INNER JOIN course_offerings co ON co.course_id=e.offering_id 
INNER JOIN courses c ON c.id=co.id

-- 6. Average grade by course offering x
SELECT c.title AS "Course", ROUND(AVG(g.grade),2) AS "Average grade" FROM courses c 
INNER JOIN course_offerings co ON c.id=co.course_id
INNER JOIN enrollments e ON e.offering_id=co.course_id
INNER JOIN students s ON s.id=e.student_id
INNER JOIN grades g ON g.student_id=s.id
GROUP BY c.title

-- 7. Students with no grades yet
SELECT s.full_name AS "Full name", g.grade AS "Grade" 
FROM students s LEFT JOIN grades g ON s.id = g.student_id WHERE g.grade IS NULL; 

-- 8. Courses with more than 2 enrolled students
SELECT c.title AS "Course", count(final_status) AS "Students enrolled"  
FROM enrollments e INNER JOIN students s ON e.student_id=s.id 
INNER JOIN course_offerings co ON co.course_id=e.offering_id 
INNER JOIN courses c ON c.id=co.id
GROUP BY title, e.final_status
HAVING final_status LIKE 'enrolled' AND count(e.final_status) > 2
