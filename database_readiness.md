# EduTrack Database Readiness Checklist

**Status:** Partially confirmed. The seed file describes the expected database, and the student reports importing it and running the assignment queries. Live database state is not independently visible from this workspace.

Status labels distinguish repository facts from student-reported Supabase evidence and items still needing confirmation.

## Repository-Verified Facts

- [x] Seed file: `edutrack.sql/eduTrack-data-edit-sql`.
- [x] The script drops and recreates `enrollments`, `students`, and `courses`. It is destructive and should not be rerun against a database containing data that must be preserved.
- [x] The script defines and seeds all three tables. Seed-file counts are 10 students, 7 courses, and 17 enrollments; these are expected seed counts, not proof of current live counts.
- [x] Assignment work is on `enrollments`; `students` and `courses` are context tables.

### Schema in the Seed File

| Table | Columns and declared types |
|---|---|
| `students` | `id SERIAL PRIMARY KEY`; `name VARCHAR(100) NOT NULL`; `email VARCHAR(150) NOT NULL UNIQUE`; `signup_date DATE NOT NULL` |
| `courses` | `id SERIAL PRIMARY KEY`; `title VARCHAR(150) NOT NULL`; `category VARCHAR(50) NOT NULL`; `instructor_name VARCHAR(100)`; `monthly_fee DECIMAL(6,2) NOT NULL` |
| `enrollments` | `id SERIAL PRIMARY KEY`; `student_id INTEGER NOT NULL`; `student_name VARCHAR(100) NOT NULL`; `student_email VARCHAR(150) NOT NULL`; `course_id INTEGER NOT NULL`; `course_title VARCHAR(150) NOT NULL`; `category VARCHAR(50) NOT NULL`; `enrollment_date DATE NOT NULL`; `completion_percentage INTEGER NOT NULL DEFAULT 0`; `passed BOOLEAN NOT NULL DEFAULT FALSE`; `monthly_fee_paid DECIMAL(6,2) NOT NULL`; `instructor VARCHAR(100)` (nullable) |

- [x] The IDs imply logical links: `enrollments.student_id` to `students.id`, and `enrollments.course_id` to `courses.id`.
- [x] The seed file declares no foreign-key constraints for those links; they are not enforced relationships in this schema.
- [x] `enrollments.completion_percentage` supports progress filtering and averages; `passed` supports pass-status filtering; `monthly_fee_paid` supports revenue totals.

### Assignment Data Mapped to the Schema

- [x] Q1-Q2 use `course_title`, student name/email, and `completion_percentage`.
- [x] Q3 and Q7 concern nullable `instructor`; the seed identifies enrollment IDs 10 and 11 as NULL initially.
- [x] Q4 uses `passed` and `completion_percentage`.
- [x] Q5 uses `enrollment_date` with the assignment cutoff and descending order.
- [x] Q6's missing record is enrollment ID 18: student 3, Lucia Fernandes, course 5, Advanced Python, Programming, 2025-04-01, completion 0, passed false, monthly fee 69.99, instructor Carlos Vega.
- [x] Q8 targets enrollment emails ending in `@test.com`; the seed marks IDs 13 (James Miller) and 14 (Alex Chen).
- [x] Q9-Q11 aggregate enrollment counts/completion by `category` or `course_title`; Q12 sums `monthly_fee_paid` by `category`.

## Student-Reported Supabase Evidence

- [x] Student reports the import ran and the baseline `enrollments` count was 17.
- [x] Student reports Q1-Q5 returned 5, 4, 2, 5, and 9 rows respectively. Q5 was run before the Q6-Q8 corrections.
- [x] Student reports ID 18 already exists and supplied the expected Q6 record values. A duplicate-ID error alone confirms the ID exists, not the other column values.
- [x] Student reports Q7 completed; its `UPDATE` returned "No rows returned." The report records IDs 10 and 11 as updated.
- [x] Student reports the Q8 preview showed IDs 13 and 14, the deletion succeeded, and a later preview returned no rows.
- [x] Student reports Q9-Q12 matched the expected results.

These are user-provided reports, not database evidence independently retrieved by Copilot.

## Still Unverified in the Live Supabase Project

- [ ] `students` and `courses` currently exist and contain the seeded data; live counts/results were not supplied.
- [ ] The live column definitions and constraints match the seed file; only the repository SQL was inspected.
- [ ] The current values for ID 18 and instructor IDs 10 and 11 were not independently confirmed with a follow-up read.
- [ ] The final `enrollments` row count after Q6-Q8 was not supplied.
- [ ] The intended Supabase project identity was not independently verified.

## Evidence Needed to Complete Readiness

- [ ] Student confirms the intended Supabase project and provides evidence that `students`, `courses`, and `enrollments` exist and contain data (for example, Table Editor screenshots or query result screenshots with sensitive project details hidden).
- [ ] Provide live schema/column-type evidence, and clarify that the student/course ID links are logical only unless foreign keys were added separately.
- [ ] Confirm the post-correction enrollment state: ID 18 has the expected values, IDs 10 and 11 have `Pending assignment`, IDs 13 and 14 are absent, and the final enrollment count is 16.

**Copilot actions:** No database connection, import, insert, update, or deletion was performed.
