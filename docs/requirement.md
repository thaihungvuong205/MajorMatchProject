# MajorMatch Requirements

## 1. Project Overview

**MajorMatch** is a web-based academic major recommendation system that helps students identify university majors that may match their skills and interests.

The system stores information about students, their skills and interests, university majors, and prerequisite subjects. This information can be used to generate major recommendations for students.

---

## 2. Student Requirements

The system shall allow a student to create and maintain a profile.

A student profile shall contain:

* `student_id`
* `student_name`
* `student_email`
* `student_education_level`

A student may have multiple skills and multiple interests.

The system shall maintain a list of available skills and interests.

---

## 3. Student Skill Requirements

A student can have multiple skills.

A skill can belong to multiple students.

Therefore, the relationship between students and skills is **many-to-many (M:N)**.

The system shall store this relationship using `STUDENT_SKILL`.

Each student-skill relationship may also store the student's **skill level**.

### Relationship

```text
STUDENT N:N SKILL
```

---

## 4. Student Interest Requirements

A student can have multiple interests.

An interest can belong to multiple students.

Therefore, the relationship between students and interests is **many-to-many (M:N)**.

The system shall connect this relationship using `STUDENT_INTEREST`.

### Relationship

```text
STUDENT N:N INTEREST
```

---

## 5. University Student Requirements

`UNIVERSITY_STUDENT` is a subtype of `STUDENT`.

A university student must first exist as a student in the `STUDENT` relation.

The university student shall be identified using the same `student_id` as the corresponding student.

The system shall store information specific to university students, such as their major.

Student attributes such as:

* `name`
* `email`
* `education_level`

are inherited from `STUDENT` and therefore shall **not be duplicated** in `UNIVERSITY_STUDENT`.

### Relationship

```text
STUDENT 1:1 UNIVERSITY_STUDENT
```

The relational representation is:

```text
UNIVERSITY_STUDENT(
    student_id PK/FK,
    major FK
)
```

---

## 6. Major Requirements

The system shall maintain a list of university majors.

Each major shall have a unique `major`.

The system shall store majors separately from students so that multiple students can belong to the same major.

### Relation

```text
DEGREE(
    major PK
)
```

---

## 7. Major Prerequisite Requirements

A major may require multiple prerequisite subjects.

A subject may be a prerequisite for multiple majors.

Therefore, the relationship between majors and prerequisite subjects is **many-to-many (M:N)**.

The system shall use `HAS_PREREQUISITE` as an associative relation to represent this relationship.

### Relationship

```text
MAJOR N:N PREREQUISITE
```

The relational representation is:

```text
HAS_PREREQUISITE(
    major PK/FK,
    subject PK/FK
)
```

---

## 8. Prerequisite Requirements

The system shall maintain a list of prerequisite subjects.

Each subject shall have a unique `subject` identifier.

### Relation

```text
PREREQUISITE(
    subject PK
)
```

---

## 9. Database Integrity Requirements

The database shall:

* Use primary keys to uniquely identify records.
* Use foreign keys to maintain referential integrity.
* Use associative relations to represent many-to-many relationships.
* Avoid unnecessary duplication of data.
* Maintain consistent relationships between entities.
* Follow appropriate normalization rules.
* Satisfy **Boyce-Codd Normal Form (BCNF)** where applicable.
* Be implementable using MySQL.

---
This design represents the inheritance between `STUDENT` and `UNIVERSITY_STUDENT` without duplicating student information.
