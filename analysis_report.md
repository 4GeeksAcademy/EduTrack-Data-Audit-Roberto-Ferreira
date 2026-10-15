# EduTrack Data Audit Analysis Report

> Question descriptions are based on `.project_specifications/EduTrack Data Audit.png`. Results below are those reported from the Supabase SQL Editor.
> Question descriptions are based on `.project_specifications/EduTrack Data Audit.png`. Supabase results and actions are student-reported; they were not independently queried by Copilot.

## Q1: 'Intro to Python' enrollments
**Result:** 5 enrollments

| Name | Email | Completed |
|---|---|---:|
| Emily Watson | emily.watson@student.edutrack.com | 85% |
| Klaus Weber | klaus.weber@student.edutrack.com | 92% |
| Marco Rossi | marco.rossi@student.edutrack.com | 88% |
| James Miller | james.miller@test.com | 30% |
| Priya Sharma | priya.sharma@student.edutrack.com | 55% |

## Q2: Potential dropouts (completion below 10%)
**Result:** 4 enrollments

| Name | Email | Completed |
|---|---|---:|
| Lucia Fernandes | lucia.fernandes@student.edutrack.com | 5% |
| Lucia Fernandes | lucia.fernandes@student.edutrack.com | 3% |
| Yuki Nakamura | yuki.nakamura@student.edutrack.com | 0% |
| Pierre Dubois | pierre.dubois@student.edutrack.com | 0% |

Lucia Fernandes appears twice because these are enrollments in two different courses.

## Q3: Enrollments with NULL instructor
**Result:** 2 enrollments

| Enrollment ID | Student | Course ID |
|---:|---|---:|
| 10 | Yuki Nakamura | 6 |
| 11 | Pierre Dubois | 6 |

## Q4: Five highest completion percentages where `passed` is false
## Q4: Five students with the highest completion percentage who have not passed
**Result:** 5 rows

| Name | Completed |
|---|---:|
| Emily Watson | 60% |
| Priya Sharma | 55% |
| Yuki Nakamura | 45% |
| Emily Watson | 40% |
| James Miller | 30% |
**Interpretation unresolved:** The screenshot asks for “the 5 students,” but the reported five rows include Emily Watson twice and therefore show four distinct students. No instructor clarification about distinct students versus enrollment rows has been supplied.

## Q5: Enrollments on or after 2024-07-01
**Result:** 9 enrollments, ordered by enrollment date descending. This was run before Q6-Q8 changed the data.
**Exact returned rows:** Pending original Supabase output or a student-confirmed rerun against an isolated copy of the original seed database. Do not treat rows reconstructed from the seed file as executed results.

## Q6: Insert the missing enrollment
**Student-reported execution:** The report records one row inserted; a later retry returned a duplicate-key error for ID 18. That error confirms the identifier is occupied, not that the intended values are stored in the row.

**Full-row read-back:** Pending. The listed values match the assignment's missing-record comment and the student's report, but were not supplied as a separate read-back result.

| ID | Student ID | Student | Course ID | Course | Category | Enrollment date | Completion | Passed | Monthly fee paid | Instructor |
|---:|---:|---|---:|---|---|---|---:|---|---:|---|
| 18 | 3 | Lucia Fernandes | 5 | Advanced Python | Programming | 2025-04-01 | 0% | false | $69.99 | Carlos Vega |

## Q7: Set NULL instructors to 'Pending assignment'
**Student-reported result:** Supabase displayed "Success. No rows returned." The student report says two rows were updated; the success message itself does not report the affected-row count.

**Post-update read-back:** Pending. The seed file shows IDs 10 and 11 initially had NULL instructors; their current instructor values have not been supplied.

## Q8: Remove enrollments for imported `@test.com` accounts
**Result:** 2 enrollments deleted. The student reported that the pre-delete preview showed IDs 13 and 14 and that a post-delete preview returned no rows.

| Deleted ID | Name | Email |
|---:|---|---|
| 13 | James Miller | james.miller@test.com |
| 14 | Alex Chen | alex.chen@test.com |

## Q9: Enrollment count by category
**Order:** Q9-Q12 were run after the Q6-Q8 corrections.
**Result:**

| Category | Total enrollments |
|---|---:|
| Data | 3 |
| Design | 4 |
| Marketing | 2 |
| Programming | 7 |

## Q10: Average completion by course title
**Result:** Ordered from lowest to highest average completion.

| Course | Average completion |
|---|---:|
| UI/UX Fundamentals | 0.0% |
| Web Design Basics | 32.5% |
| Digital Marketing 101 | 36.5% |
| Advanced Python | 45.0% |
| Data Analysis with SQL | 47.7% |
| Intro to Python | 80.0% |

## Q11: Courses with more than three enrollments
**Result:** 1 course.

| Course | Total enrollments |
|---|---:|
| Intro to Python | 4 |

## Q12: Total revenue by category
**Result:** Ordered from highest to lowest.

| Category | Total revenue |
|---|---:|
| Programming | $409.93 |
| Data | $179.97 |
| Design | $169.96 |
| Marketing | $59.98 |

## Scope and verification
- Assignment work modifies only the `enrollments` table; `students` and `courses` are context tables.
- The student reports that the instructor explicitly authorized AI assistance. This report does not claim independent SQL authorship.
- Query results and mutation counts are reported by the student from Supabase. They were not independently verified through a database connection in this workspace.