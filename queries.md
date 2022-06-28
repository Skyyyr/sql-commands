### Select all rows from the classes table.
`select * from classes;`

### Select the name and credits from the classes table where the number of credits is greater than 3.
`select name, credits from classes where credits > 3;`

### All rows from the classes table where credits is an even number.
`select * from classes where mod(credits, 2) = 0;`

### All of Tianna's enrollments that she hasn't yet received a grade for.
`select enrollments from enrollments, students where students.id = 1 and enrollments.grade is null;`

### All of Tianna's enrollments that she hasn't yet received a grade for, selected by her first name, not her student.id
`select enrollments from enrollments, students where students.first_name = 'Tianna' and enrollments.grade is null;`

### All of Tianna's enrollments that she hasn't yet received a grade for, selected by her first name, not her student.id, with the class name included in the result set.
`select enrollments, classes.name 
as enroll 
from enrollments, students, classes 
where students.first_name = 'Tianna' and enrollments.grade is null and enrollments.class_id = classes.id;`

### All students born before 1986 who have a 't' in their first or last name.
`select students.first_name, students.birthdate from students where students.birthdate <= '1986-01-01';`

### The average age of all the students.
`SELECT AVG(EXTRACT(YEAR from age(birthdate))) FROM students;`
###### and
`SELECT AVG(age((SELECT current_date), birthdate)) FROM STUDENTS;`

### Addresses that have a space in their city name.
`select addresses.city from addresses where addresses.city like '% %';`

### Students & their addresses that live in a city with more than one word in the city name.
`select addresses.city, students.first_name from addresses, students where addresses.city like '% %' and addresses.id = students.id;`

### The average number of credits for classes offered at the school.
`select avg( ALL classes.credits) from classes;`

### The first and last name of all students who have received an 'A'.
`select students.first_name, students.last_name, enrollments.grade from enrollments, students where enrollments.grade = 'A' and enrollments.student_id = students.id;`

### Each student's first name and the total credits they've enrolled in
`select
    students.first_name,
    sum(credits) credits
from
    classes, students, enrollments
where
    students.id = enrollments.student_id and enrollments.class_id = classes.id
group by
    students.first_name;`

### The total number of credits each student has received a grade for.
`select
    students.first_name,
    sum(credits) credits
from
    classes, students, enrollments
where
    students.id = enrollments.student_id and
    enrollments.class_id = classes.id and
    enrollments.grade is not null
group by
    students.first_name;`

### All enrollments, including the class name.
`select
    enrollments as enrollmnts,
    classes.name as cls
from
    classes, enrollments`

### Students born between 1982-1985 (inclusive).
`select
    *
from
    students
where
    students.birthdate >= cast('1982-01-01'as date) and
    students.birthdate <= cast('1985-01-01'as date);`

### Insert a new enrollment recording that Andre Rohan took PHYS 218 and got an A.
`INSERT INTO enrollments (id, student_id, class_id, grade) VALUES (17, 6, 4, 'A');`
#### and
`INSERT INTO enrollments (student_id, class_id, grade)
VALUES (
    (SELECT students.id
    FROM students
    WHERE first_name="Andre" AND last_name="Rohan"),
    (SELECT classes.id
    FROM classes
    WHERE name="PHYS 218"),
"A")`