# Class Diagram — Explanation

**System:** University Automated Grading System

---

## What is a Class Diagram?

A class diagram is like a **blueprint of a system**. A class diagram shows the structure of a system by displaying its classes, their attributes, methods, and the relationships between them. It helps the team understand how the system is organized and how components interact.

---

## What Does This Diagram Show?

This diagram shows how the university's automated grading system is structured. There are **19 classes** in total. Each class is a box with three sections — the class name at the top, the data it stores in the middle, and the actions it can do at the bottom.

---

## The Classes — In Simple Words

### `User`
This is the base class for everyone who uses the system. It is marked as **abstract**, which means no one logs in as just a "User"  they are always a Student, Professor, or Admin. It holds common information like name and email that everyone shares.

### `Student`
Represents a student. Stores their student ID and the courses they are enrolled in. A student can submit assignments, check their grade, and read feedback.

### `Professor`
Represents a lecturer. A professor can create assignments, set how they will be graded, set the due date, and if needed, manually change a grade.

### `Admin`
Represents a system administrator. They manage user accounts, generate reports, and export records for the yearly government audit.

### `Course`
Represents a subject like "Introduction to Programming." A course belongs to a professor and contains assignments. It can sync its information to the university's LMS (Learning Management System).

### `Assignment`
Represents a piece of work given to students. It has a title, a due date, and a setting for whether students can submit more than once. Once the deadline passes, the system automatically stops accepting submissions.

### `Submission`
Created every time a student submits their code. It records which attempt number it is, when it was submitted, and a link to their GitHub repository. After processing, it gets connected to a grade and a plagiarism report.

### `GradingCriteria`
The marking rules the professor sets for an assignment. It holds how much each metric (like code style or efficiency) is worth, and what score a student needs to pass. It also includes test cases that the code must pass.

### `SourceFile`
The actual code file the student uploaded. It stores the file name, the programming language, and the size of the file.

### `ExecutionResult`
The result of running the student's code. It records what the program printed, whether there were any errors, and how long it took to run.

### `Grade`
The final mark a student receives. It holds the score, the maximum possible score, and whether a professor has manually changed it. Every grade is saved permanently for audit purposes.

### `AuditLog`
A permanent record of important events — especially when a grade is published or changed. This exists to satisfy the government audit requirement. It cannot be edited after it is created.

### `Notification`
A message sent to the student when their assignment has been graded. It tracks whether the student has read it yet.

### `TestSuite`
A set of automated tests written by the professor. The system runs these tests against the student's code to check if it works correctly.

### `CodeExecutionEngine`
The part of the system that actually runs the student's code. It does this inside a safe, isolated environment (like a container) so that badly written code cannot harm the server. It has a time limit and a memory limit.

### `GradingEngine`
The brain of the system. It coordinates everything — fetching the code, running it, checking for plagiarism, calculating the grade, saving the result, and notifying the student. This is what replaces the professor doing everything manually.

### `GitHubConnector`
Fetches the student's code from their GitHub repository automatically. This replaces the old process where professors had to clone repositories themselves.

### `PlagiarismService` *(interface)*
A set of rules that any plagiarism checker must follow. Because it is an interface, the system can easily switch between different plagiarism tools without breaking anything else.

### `TurnitinAdapter`
Connects the system to Turnitin — a well-known plagiarism detection website. It sends the student's submission to Turnitin and gets back a similarity score.

### `InternalPlagChecker`
A built-in plagiarism checker that compares a student's submission against other students' submissions within the same assignment.

### `LMSConnector` *(interface)*
A set of rules for connecting to the university's Learning Management System. Because the LMS is an old mainframe system that is hard to change, the interface keeps all the messy LMS communication in one place.

### `MainframeLMSAdapter`
Does the actual job of sending grades to the university's old mainframe LMS. It translates the system's data into a format the mainframe understands, without needing any changes to the mainframe itself.

---

## How the Classes Connect

### Inheritance
Student, Professor, and Admin all **inherit from User**. This means they automatically get the name and email fields from User, and each adds its own extra details on top.

### Composition (strong ownership)
Some classes are built from other classes and cannot exist without them:
- An `Assignment` owns its `Submission` objects — if the assignment is deleted, so are the submissions.
- A `Submission` owns its `SourceFile` and `ExecutionResult`.
- `GradingCriteria` owns its `TestSuite` objects.

### Aggregation (loose ownership)
A `Course` contains `Assignment` objects, but an assignment could exist on its own — it is a softer connection.

### Dependency (uses)
Some classes temporarily use another class to get a job done:
- `GradingEngine` uses `CodeExecutionEngine` to run code.
- A `Grade` being created causes an `AuditLog` entry to be written.

### Realization (implements an interface)
- `TurnitinAdapter` and `InternalPlagChecker` both implement `PlagiarismService`.
- `MainframeLMSAdapter` implements `LMSConnector`.

---

## Why This Design Makes Sense

| Problem | Solution in the Diagram |
|---|---|
| Professors grading manually | `GradingEngine` automates the whole pipeline |
| Students submitting via GitHub | `GitHubConnector` fetches code automatically |
| Need for plagiarism checking | `PlagiarismService` interface with Turnitin and internal checker |
| Difficult mainframe LMS | `MainframeLMSAdapter` wraps it without changing it |
| Government audit requirement | `AuditLog` permanently records every grade event |
| Students wanting multiple tries | `allowMultipleAttempts` flag on `Assignment` |
| Deadline enforcement | `dueDateTime` and `isAcceptingSubmissions()` on `Assignment` |

---

