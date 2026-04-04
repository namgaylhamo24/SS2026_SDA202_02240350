# Object Model : Explanation

**System:** Automated Grading System
---

## What is an Object Diagram?

If the class diagram is the **blueprint**, the object diagram is a **photograph of the system at one moment in time**.

Instead of showing class templates, it shows real examples — actual people with actual names, actual assignments with actual values, and actual results. Every box in the diagram is a real instance of a class, with real data filled in.

Think of it like this: the class diagram shows the design of a form, and the object diagram shows a form that has already been filled out by a real person.

---

## The Scenario Being Shown

> A student has submitted their Python code for a Binary Search Tree assignment. This is their second attempt. The system automatically picked up the code from GitHub, checked it for plagiarism, ran it to see if it works, calculated the grade, and sent the student a notification.

**Note:** The student name used here is an example to show what a real submission looks like. Any student in the system would produce a similar set of objects.

**Time of snapshot:** The submission happened on 19 April 2026. The grade was published on 20 April 2026.

---

## What Each Object Represents

### `student1 : Student`
```
name = "Tshering Lhamo"   
email = "02240350.cst@rub.edu.bt"
enrolledCourses = "CSF101"
```
This is the student who submitted the assignment. She is enrolled in CSF101, which is the course this assignment belongs to.

---

### `prof1 : Professor`
```
name = "Saroj Saneyasi"
department = "Software Engineering"
```
The professor who created the assignment and set the grading rules. He is connected to the course, the assignment, and the grading criteria.

---

### `admin1 : Admin`
```
name = "Sonam Wangdi"
adminLevel = 2
```
An admin who can view the audit log. Their presence in the diagram shows that grade records are accessible to administrators for oversight and compliance.

---

### `course1 : Course`
```
courseId = "CSF101"
name = "Introduction to Programming"
semester = "Sem 1 2026"
```
The course the assignment belongs to. Both the student and the professor are linked to this course.

---

### `assign1 : Assignment`
```
title = "Implement a Binary Search Tree"
dueDateTime = "2026-04-20 23:59"
allowMultipleAttempts = true
isOpen = false
```
The assignment itself. Two things to notice:
- `allowMultipleAttempts = true` — this is why the student was allowed to submit a second time.
- `isOpen = false` — the deadline has passed. The system is no longer accepting submissions automatically.

---

### `criteria1 : GradingCriteria`
```
metricWeights = "codeStyle 0.2, efficiency 0.3"
passingThreshold = 50.0
```
The marking rules. Code style is worth 20%, efficiency is worth 30%, and the rest comes from test results. A student needs at least 50 marks to pass.

---

### `suite1 : TestSuite`
```
language = "Python"
testCases = "insert_test, delete_test, search_test"
```
Three automated tests that check whether the student's Binary Search Tree can insert a node, delete a node, and search for a value. All three were run against the student's code.

---

### `sub1 : Submission`
```
attemptNumber = 2
submittedAt = "2026-04-19 14:32"
status = "GRADED"
repoUrl = "github.com/tsheringlhamo/csf101-bst"
```
The submission itself. This is the student's second attempt, submitted one day before the deadline. The status is `GRADED`, meaning the full pipeline has completed. The `repoUrl` shows where the system fetched the code from.

---

### `file1 : SourceFile`
```
fileName = "bst.py"
language = "Python"
sizeBytes = 4096
```
The actual Python file the student wrote. It is 4 KB in size — about right for a Binary Search Tree implementation. This file was fetched from GitHub and then run inside the sandbox.

---

### `exec1 : ExecutionResult`
```
stdout = "All tests passed"
stderr = ""
exitCode = 0
executionTimeMs = 312
memoryUsedMB = 18
```
The result of running the student's code. All three tests passed. There were no errors. The code ran in 312 milliseconds and used only 18 MB of memory — well within the system's limits.

---

### `grade1 : Grade`
```
score = 82.5
maxScore = 100.0
isOverridden = false
gradedAt = "2026-04-20 14:35"
```
The final grade — 82.5 out of 100. This was calculated automatically. `isOverridden = false` means the professor did not step in to change it. The student passed (82.5 is above the 50.0 threshold).

---

### `plagRep1 : PlagiarismReport`
```
similarityScore = 12.4
matchedSources = "SUB-4100, turnitin-db"
isFlagged = false
```
The plagiarism check result. A 12.4% similarity score was found — this is low enough that the system did not flag it. The grade was allowed to proceed normally. If the score were much higher, the submission would have been flagged for the professor to review manually.

---

### `auditLog1 : AuditLog`
```
eventType = "GRADE_PUBLISHED"
timestamp = "2026-04-20 14:36"
details = "Grade 82.5 published for SUB-4421"
```
A permanent record that this grade was published. This exists so that the government regulatory body can check it during the yearly audit. It cannot be changed or deleted after it is created.

---

### `notif1 : Notification`
```
message = "Assignment graded: 82.5 out of 100"
sentAt = "2026-04-20 14:36"
isRead = false
```
The message sent to the student telling them their result. `isRead = false` means the student has not opened it yet at the time of this snapshot. It would change to `true` when she logs in and reads it.

---

### `ghConn1 : GitHubConnector`
```
baseUrl = "https://api.github.com"
apiToken = "ghp_masked"
```
This object fetched the student's code from GitHub. The API token is hidden for security. Without this, the professor would have had to clone the repository manually.

---

### `execEngine1 : CodeExecutionEngine`
```
sandboxType = "Docker"
timeoutMs = 10000
memoryLimitMB = 256
```
The safe environment where the student's code was run. It uses Docker so that if the code is badly written or malicious, it cannot harm the server. There is a 10-second time limit and a 256 MB memory limit.

---

### `gradingEng1 : GradingEngine`
```
autoGradeEnabled = true
weightingStrategy = "WeightedSum"
```
The part of the system that coordinates everything. It fetched the code, ran it, calculated the grade, saved the result, sent the notification, and synced with the LMS — all automatically. `autoGradeEnabled = true` means no professor action was needed at all.

---

### `turnitin1 : TurnitinAdapter`
```
endpoint = "https://api.turnitin.com/v1"
apiKey = "TII_masked"
```
The object that sent the submission to Turnitin's website to check for plagiarism. The API key is hidden for security.

---

### `lmsAdapter1 : MainframeLMSAdapter`
```
endpoint = "https://lms.uni.edu/mainframe"
authToken = "LMS_masked"
```
After the grade was calculated, this object sent it to the university's old mainframe LMS so that the grade appears there too. The mainframe itself did not need to be changed — this adapter handles all the translation.

---

## What Happened, Step by Step

| Step | What Happened |
|---|---|
| 1 | Student pushed her code to GitHub |
| 2 | System fetched the code automatically via `ghConn1` |
| 3 | Turnitin checked the code for plagiarism — 12.4%, not flagged |
| 4 | Code ran in a Docker sandbox — all 3 tests passed |
| 5 | Grade calculated — 82.5 out of 100 |
| 6 | Audit log entry created permanently |
| 7 | Notification sent to the student |
| 8 | Grade synced to the university's LMS |

---

## How This Connects to the Class Diagram

Every object here comes from the class diagram:

| Object | Its Class |
|---|---|
| `student1` | `Student` |
| `assign1` | `Assignment` |
| `sub1` | `Submission` |
| `grade1` | `Grade` |
| `turnitin1` | `TurnitinAdapter` |
| `lmsAdapter1` | `MainframeLMSAdapter` |
| … and so on | |

The class diagram said *what is possible*. The object diagram shows *what actually happened* in one real case. Together they prove the design works.

---
