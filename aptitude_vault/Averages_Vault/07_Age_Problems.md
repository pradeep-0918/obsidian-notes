# 07 — Age Problems 🎂
*← [[06_Missing_Number_Problems]] | Next → [[08_Replacement_Problems]]*

---

## 🧒 Feynman Explanation

Age problems love to **hide one person's age** or ask what the average was **a few years ago/later**.

The trick? **All ages change together.**  
If 5 people's ages increase by 3 years, the average also increases by 3!

---

## 🔑 Key Age Rules

### Rule 1 — Ages shift equally over time

> If average age of a group is X **today**,  
> Then average age **t years later** = X + t  
> And **t years ago** = X − t

### Rule 2 — Adding a new person

> New person's age = New Avg + n × (New Avg − Old Avg)  
> *(This is just [[03_Shortcut_Tricks]] Trick 6!)*

### Rule 3 — Removing a person

> Removed person's age = Old Avg − n × (New Avg − Old Avg)  
> *(Or: use Sum difference method)*

---

## 📝 Example 1 — Classic Age Shift

> Average age of a family of 5 is 30 years today.  
> What was the average 5 years ago?

```
Answer = 30 − 5 = 25 years ✅
(Every person was 5 years younger, so average drops by 5)
```

---

## 📝 Example 2 — New Member Joins

> Average age of 4 friends is 22 years.  
> A new friend joins, average becomes 24.  
> How old is the new friend?

```
Using Trick 6:
New person = New Avg + n × (New Avg − Old Avg)
           = 24 + 4 × (24 − 22)
           = 24 + 8
           = 32 years ✅
```

**Verify:** Total before = 4×22 = 88. Total after = 5×24 = 120.  
New person = 120 − 88 = 32 ✅

---

## 📝 Example 3 — Child Replaces Old Member

> Average age of 10 employees = 35 years.  
> One employee (58 years) retires.  
> A new employee joins.  
> Average remains 35.  
> Find new employee's age.

```
If average stays same → Total sum stays same
New person must have same age as the one who left!
New employee = 58 years... Wait, that can't be right.

Let me redo: Average stays 35?
Sum before = 10×35 = 350
Person leaves: 350 − 58 = 292 (9 people)
New avg still 35 → Sum = 9×35 = 315
New person = 315 − 292 = 23 years ✅
```

*(Average can change — read carefully!)*

---

## 📝 Example 4 — Classic Trick Question

> Average age of a class of 30 students is 14.  
> The teacher is included, and average becomes 15.  
> Find the teacher's age.

```
Using Trick 6 from [[03_Shortcut_Tricks]]:
Teacher's age = New Avg + n × (New Avg − Old Avg)
              = 15 + 30 × (15 − 14)
              = 15 + 30
              = 45 years ✅
```

---

## 🚨 Common Traps

| Trap | Watch Out! |
|------|-----------|
| "5 years later" | Add 5 to average directly |
| "5 years ago" | Subtract 5 from average directly |
| Baby is born | Baby starts at age 0 — pulls average DOWN |
| New teacher joins students | Teacher likely older — pulls average UP |

---

## 🔗 Connected Notes
- [[03_Shortcut_Tricks]] — Trick 5 & 6 (Replacement & New Member)
- [[08_Replacement_Problems]] — When one person replaces another
- [[10_Practice_Questions_With_Solutions]] — Practice Q21–Q25

---
*Tags: #age-problems #family-age #teacher-student #averages*
