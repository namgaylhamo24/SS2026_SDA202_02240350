# Reflection
---

## Diagram 1 — Class Diagram
### What I Did
I built a UML Class Diagram for a University Automated Grading System which required automatic grading of student code submissions and plagiarism detection and integration with the university LMS and maintenance of permanent grade records for auditing purposes. My system design included 19 classes which I established their attributes and methods and created their relationship diagram using draw.io. I created the same diagram in Mermaid code which I exported from draw.io.

### What I Learned
Before this activity I thought a class diagram was just drawing boxes and connecting them with lines. The process demonstrated to me that I needed to complete the task through detailed thinking. I needed to determine which information each class should store and which functions it should perform and which classes it should connect with.

I discovered the essential distinction between **composition** and **aggregation**. I used to think they were the same thing but now I understand that composition means the child cannot exist without the parent for example a `Submission` cannot exist without an `Assignment`. Aggregation provides a less strict connection which permits the child to function independently.

I discovered **interfaces** and the **Adapter pattern**. I could not modify the university LMS because it operated on an outdated mainframe system. I developed a `LMSConnector` interface which I implemented through a `MainframeLMSAdapter` class. The experience revealed to me that software development requires handling current systems instead of creating entirely new solutions.

### What Was Difficult
The most challenging aspect required me to determine where classes should be separated. I struggled to decide whether `GradingCriteria` needed to exist as a separate class or it needed to exist as an embedded component.

---

## Diagram 2 — Object Model
### What I Did
I developed a UML Object Diagram for the University Automated Grading System through this project. The object diagram displays actual system operation at a specific moment when a student submits an assignment whereas the class diagram provides system design documentation. I selected the scenario where a student presents her second Python Binary Search Tree implementation which enters the complete automated grading system.

### What I Learned
The most important thing I learned from this activity is the **difference between a class and an object**. I knew that a class serves as a template while an object functions as an instance but I could not perceive the distinction until I needed to use actual data. The system gained actual existence through my decision to write `score = 82.5` instead of `score: float`.

An object diagram functions as an effective method to determine whether your class diagram functions correctly. The process of creating actual instances from my classes enabled me to verify the accuracy of the relationships I had established. The system confirms that a single `Submission` connects to one `ExecutionResult`, one `Grade`, and one `PlagiarismReport` which aligns with my design in the class diagram.

I acquired new knowledge about **system state**. The notification attribute `isRead = false` and the assignment attribute `isOpen = false` demonstrated to me that object values undergo continuous modification because the system experiences various events. The object diagram depicts a single instant in time which functions like a photographic image.

The selection process reached its most challenging stage when I needed to find which scenario should be displayed. I wanted a scenario that would demonstrate as many parts of the system as possible — not just the happy path where everything works, but something that exercises the grading pipeline, plagiarism check, audit log, and LMS sync all at once. The second attempt which I selected passed all tests because it demonstrated the `allowMultipleAttempts` feature.

I faced difficulties when determining which objects to include and which objects should not be included. The system contains multiple classes which require every possible object to be shown in the diagram to achieve complete system representation. I needed to decide which elements of the scenario required inclusion.

### What I Would Do Differently
I would add a second object diagram that shows a **failed scenario** — for example, a submission that gets flagged for plagiarism or a submission where the code crashes during execution. The object diagram represents a single situation through its design. The system shows different behaviors for contrasting scenarios.

---

## Overall Reflection

The two activities worked together to provide me with an extensive knowledge foundation about UML and system design. The class diagram taught me to think about structure — what classes exist and how they relate. The object diagram taught me to think about behaviour — what actually happens when the system runs.

The experience of working with three different formats I found useful because I needed to create the visual diagram with draw.io and the code version with Mermaid and the explanation with Markdown. The different formats required me to process the same information in different ways which resulted in better comprehension of the material.



---

## References
 
GeeksforGeeks. (2026). *UML Class Diagram*. Retrieved from https://www.geeksforgeeks.org/system-design/unified-modeling-language-uml-class-diagrams/
 
GeeksforGeeks. (2025). *Object Diagrams | Unified Modeling Language (UML)*. Retrieved from https://www.geeksforgeeks.org/system-design/unified-modeling-language-uml-object-diagrams/

