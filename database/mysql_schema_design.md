# AI-Powered Academic University Management System

## MySQL Schema Design Document

### Overview

This document defines the initial database schema design for the AI-Powered Academic University Management System. The system manages students, teachers, departments, courses, enrollments, attendance, assignments, examinations, results, payments, notices, and AI assistant interactions.

---

# 1. Department

```sql
Department(
    department_id INT PRIMARY KEY,
    department_name VARCHAR(100) NOT NULL,
    department_code VARCHAR(20) UNIQUE NOT NULL
)
```

### Description

Stores academic departments within the university.

---

# 2. Student

```sql
Student(
    student_id INT PRIMARY KEY,
    student_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    phone VARCHAR(20),
    gender ENUM('Male','Female','Other'),
    date_of_birth DATE,
    admission_date DATE,
    semester INT,
    department_id INT,
    FOREIGN KEY (department_id)
    REFERENCES Department(department_id)
)
```

### Description

Stores student information.

---

# 3. Teacher

```sql
Teacher(
    teacher_id INT PRIMARY KEY,
    teacher_name VARCHAR(100) NOT NULL,
    email VARCHAR(100) UNIQUE NOT NULL,
    phone VARCHAR(20),
    designation VARCHAR(100),
    department_id INT,
    FOREIGN KEY (department_id)
    REFERENCES Department(department_id)
)
```

### Description

Stores teacher information.

---

# 4. Course

```sql
Course(
    course_id INT PRIMARY KEY,
    course_code VARCHAR(20) UNIQUE NOT NULL,
    course_name VARCHAR(100) NOT NULL,
    credit DECIMAL(3,1),
    department_id INT,
    teacher_id INT,
    FOREIGN KEY (department_id)
    REFERENCES Department(department_id),
    FOREIGN KEY (teacher_id)
    REFERENCES Teacher(teacher_id)
)
```

### Description

Stores course information.

---

# 5. Enrollment

```sql
Enrollment(
    enrollment_id INT PRIMARY KEY,
    student_id INT,
    course_id INT,
    enrollment_date DATE,
    FOREIGN KEY (student_id)
    REFERENCES Student(student_id),
    FOREIGN KEY (course_id)
    REFERENCES Course(course_id)
)
```

### Description

Represents student-course enrollment.

---

# 6. Attendance

```sql
Attendance(
    attendance_id INT PRIMARY KEY,
    student_id INT,
    course_id INT,
    attendance_date DATE,
    status ENUM('Present','Absent','Late'),
    FOREIGN KEY (student_id)
    REFERENCES Student(student_id),
    FOREIGN KEY (course_id)
    REFERENCES Course(course_id)
)
```

### Description

Tracks attendance records.

---

# 7. Assignment

```sql
Assignment(
    assignment_id INT PRIMARY KEY,
    course_id INT,
    title VARCHAR(200),
    description TEXT,
    due_date DATE,
    FOREIGN KEY (course_id)
    REFERENCES Course(course_id)
)
```

### Description

Stores course assignments.

---

# 8. Submission

```sql
Submission(
    submission_id INT PRIMARY KEY,
    assignment_id INT,
    student_id INT,
    submission_date DATETIME,
    file_url VARCHAR(255),
    marks DECIMAL(5,2),
    FOREIGN KEY (assignment_id)
    REFERENCES Assignment(assignment_id),
    FOREIGN KEY (student_id)
    REFERENCES Student(student_id)
)
```

### Description

Stores assignment submissions.

---

# 9. Exam

```sql
Exam(
    exam_id INT PRIMARY KEY,
    course_id INT,
    exam_type VARCHAR(50),
    exam_date DATE,
    total_marks DECIMAL(5,2),
    FOREIGN KEY (course_id)
    REFERENCES Course(course_id)
)
```

### Description

Stores examination information.

---

# 10. Result

```sql
Result(
    result_id INT PRIMARY KEY,
    student_id INT,
    exam_id INT,
    marks_obtained DECIMAL(5,2),
    grade VARCHAR(5),
    FOREIGN KEY (student_id)
    REFERENCES Student(student_id),
    FOREIGN KEY (exam_id)
    REFERENCES Exam(exam_id)
)
```

### Description

Stores examination results.

---

# 11. Notice

```sql
Notice(
    notice_id INT PRIMARY KEY,
    title VARCHAR(200),
    description TEXT,
    publish_date DATE
)
```

### Description

Stores university notices and announcements.

---

# 12. Payment

```sql
Payment(
    payment_id INT PRIMARY KEY,
    student_id INT,
    amount DECIMAL(10,2),
    payment_date DATE,
    payment_method VARCHAR(50),
    payment_status ENUM('Pending','Paid','Failed'),
    FOREIGN KEY (student_id)
    REFERENCES Student(student_id)
)
```

### Description

Stores tuition and fee payment information.

---

# 13. User

```sql
User(
    user_id INT PRIMARY KEY,
    email VARCHAR(100) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    role ENUM('Admin','Teacher','Student')
)
```

### Description

Stores authentication and authorization data.

---

# 14. AI_Assistant_Log

```sql
AI_Assistant_Log(
    log_id INT PRIMARY KEY,
    student_id INT,
    query_text TEXT,
    ai_response TEXT,
    created_at DATETIME,
    FOREIGN KEY (student_id)
    REFERENCES Student(student_id)
)
```

### Description

Stores AI chatbot conversations and recommendations.

---

# 15. Library_Book

```sql
Library_Book(
    book_id INT PRIMARY KEY,
    title VARCHAR(200),
    author VARCHAR(150),
    isbn VARCHAR(30),
    available_copies INT
)
```

### Description

Stores library books.

---

# 16. Book_Issue

```sql
Book_Issue(
    issue_id INT PRIMARY KEY,
    book_id INT,
    student_id INT,
    issue_date DATE,
    return_date DATE,
    FOREIGN KEY (book_id)
    REFERENCES Library_Book(book_id),
    FOREIGN KEY (student_id)
    REFERENCES Student(student_id)
)
```

### Description

Tracks issued library books.

---

# Relationships Summary

* Department → Student (1:M)
* Department → Teacher (1:M)
* Department → Course (1:M)
* Teacher → Course (1:M)
* Student ↔ Course (M:N via Enrollment)
* Course → Assignment (1:M)
* Assignment → Submission (1:M)
* Student → Submission (1:M)
* Course → Exam (1:M)
* Student → Result (1:M)
* Student → Attendance (1:M)
* Student → Payment (1:M)
* Student → AI_Assistant_Log (1:M)
* Student → Book_Issue (1:M)
* Library_Book → Book_Issue (1:M)

---

# Future Features

* AI Course Recommendation
* AI Performance Prediction
* Smart Attendance Analytics
* Student Risk Detection
* Scholarship Recommendation System
* Placement Prediction
* Faculty Performance Dashboard

---

Prepared for:
Database Theory, Database Lab, and Software Engineering Course Project

Project Name:
AI-Powered Academic University Management System
