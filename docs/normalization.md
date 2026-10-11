# BCNF Normalization

A relation is in **Boyce-Codd Normal Form (BCNF)** if, for every non-trivial functional dependency `X → Y`, `X` is a **superkey** of the relation.

To check BCNF:

1. Find the closure `X+`.
2. Compare `X+` with all attributes in the relation.
3. If `X+` contains all attributes, then `X` is a superkey.
4. If `X` is a superkey, the functional dependency satisfies BCNF.
5. If `X` is not a superkey, the relation violates BCNF.

For our MajorMatch project, we want to create the databse system such that it satisfies the BCNF Normalization

---

## 1. STUDENT

**Relation:**

```text
STUDENT(student_id, student_name, student_email, student_education_level)
```

**Functional dependency:**

```text
student_id → student_name, student_email, student_education_level
```

### Closure

```text
student_id+ = {student_id}
```

Using the functional dependency:

```text
student_id → student_name, student_email, student_education_level
```

we get:

```text
student_id+ = {student_id, student_name, student_email, student_education_level}
```

Since the closure contains every attribute in `STUDENT`:

```text
student_id+ = STUDENT
```

Therefore, `student_id` is a **superkey**.

The functional dependency satisfies BCNF.

**Conclusion:** `STUDENT` is in BCNF.

---

## 2. SKILL

**Relation:**

```text
SKILL(skill_name)
```

There are no non-trivial functional dependencies because the relation contains only one attribute.

**Conclusion:** `SKILL` is in BCNF.

---

## 3. INTEREST

**Relation:**

```text
INTEREST(interest_name)
```

There are no non-trivial functional dependencies because the relation contains only one attribute.

**Conclusion:** `INTEREST` is in BCNF.

---

## 4. STUDENT_SKILL

**Relation:**

```text
STUDENT_SKILL(student_id, skill_name, skill_level)
```

The primary key is:

```text
(student_id, skill_name)
```

**Functional dependency:**

```text
(student_id, skill_name) → skill_level
```

### Closure

```text
(student_id, skill_name)+
= {student_id, skill_name}
```

Using:

```text
(student_id, skill_name) → skill_level
```

we get:

```text
(student_id, skill_name)+
= {student_id, skill_name, skill_level}
```

The closure contains every attribute in `STUDENT_SKILL`.

Therefore:

```text
(student_id, skill_name)+ = STUDENT_SKILL
```

So `(student_id, skill_name)` is a **superkey**.

The functional dependency satisfies BCNF.

**Conclusion:** `STUDENT_SKILL` is in BCNF.

---

## 5. STUDENT_INTEREST

**Relation:**

```text
STUDENT_INTEREST(student_id, interest_name, interest_level)
```

The primary key is:

```text
(student_id, interest_name)
```

**Functional dependency:**

```text
(student_id, interest_name) → interest_level
```

### Closure

```text
(student_id, interest_name)+
= {student_id, interest_name, interest_level}
```

The closure contains every attribute in `STUDENT_INTEREST`.

Therefore, `(student_id, interest_name)` is a **superkey**.

**Conclusion:** `STUDENT_INTEREST` is in BCNF.

---

## 6. UNIVERSITY_STUDENT

The original relation is:

```text
UNIVERSITY_STUDENT(
    student_id PK,
    major FK
)
```

Because `student_id` references `STUDENT(student_id)`, the student information is already determined by the student ID.

Therefore, we have the functional dependency:

```text
student_id → major
```

### Closure

```text
student_id+ = {student_id, major}
```
The closure contains every attribute in `UNIVERSITY_STUDENT`.

Therefore, `student_id` is a **superkey**.

**Conclusion:** `UNIVERSITY_STUDENT` is in BCNF.

---

## 7. MAJOR

**Relation:**

```text
MAJOR(major)
```

There are no non-trivial functional dependencies because the relation contains only one attribute.

**Conclusion:** `MAJOR` is in BCNF.

---

## 8. HAS_PREREQUISITE

**Relation:**

```text
HAS_PREREQUISITE(major, subject)
```

The primary key is:

```text
(major, subject)
```

There are no additional non-trivial functional dependencies.

### Closure

```text
(major, subject)+
= {major, subject}
```

The closure contains every attribute in `HAS_PREREQUISITE`.

Therefore, `(major, subject)` is a **superkey**.

**Conclusion:** `HAS_PREREQUISITE` is in BCNF.

---

## 9. PREREQUISITE

**Relation:**

```text
PREREQUISITE(subject)
```

There are no non-trivial functional dependencies because the relation contains only one attribute.

**Conclusion:** `PREREQUISITE` is in BCNF.

---

# Final BCNF Result

After removing the redundant student attributes from `UNIVERSITY_STUDENT`, the final relations are:

```text
STUDENT(
    student_id PK,
    student_name,
    student_email,
    student_education_level
)

SKILL(
    skill_name PK
)

INTEREST(
    interest_name PK
)

STUDENT_SKILL(
    student_id PK/FK,
    skill_name PK/FK,
    skill_level
)

STUDENT_INTEREST(
    student_id PK/FK,
    interest_name PK/FK
    interest_level
)

UNIVERSITY_STUDENT(
    student_id PK/FK,
    major FK
)

MAJOR(
    major PK
)

HAS_PREREQUISITE(
    major PK/FK,
    subject PK/FK
)

PREREQUISITE(
    subject PK
)
```

For every non-trivial functional dependency in these relations, the determinant's closure contains all attributes of its relation. Therefore, every determinant is a superkey.

**Thus, all final relations satisfy BCNF.**
