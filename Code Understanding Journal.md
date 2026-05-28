JavaScript Task Manager - My Findings
-----------------------------------------------------------------------------
This seems like a task organiser application that organises an individual's tasks according to their due date and level of priorty that the user has put in their task manager(Makes use of CRUD operations).
 
The dates are organised in priority from low, medium, high and urgent, by prompting the user to state the level of priority for the task
on a scale of 1-4.
 
This application organises the date according to a seamless date structure, therefore, it is important that the date is correctly placed
otherwise an error will be passed, asking the individual to correctly input the date according to the required format (yyyy-mm-dd)
 
## --------------------------------------------------------------------------------
# Exercise 2: Finding Feature Implementation

## Findings for Task Export to CSV Feature

After exploring the codebase, I found that the best place for implementing CSV export functionality is inside `storage.js` because it already handles reading and writing files using the Node.js `fs` module.

The export feature would likely follow this flow:

1. The user runs a CLI command such as:

   ```bash
   node cli.js export
   ```

2. `cli.js` calls a new method in `app.js`

3. `app.js` calls a new storage method like:

   ```js
   exportTasksToCSV()
   ```

4. `storage.js` retrieves tasks using:

   ```js
   getAllTasks()
   ```

5. The tasks are converted into CSV format

6. The CSV data is written to a file such as:

   ```txt
   tasks.csv
   ```

Components affected:

* `cli.js` → new command
* `app.js` → new export method
* `storage.js` → CSV file generation
* Possibly `package.json` if a CSV library is added

Planned implementation approach:

* Create CSV conversion logic
* Add export method to storage
* Add app-level wrapper method
* Add CLI command
* Test generated CSV file

## ---------------------------------------------------------------

## Exercise 3: Understanding Domian Model
## Glossary

# Senior Developer Review of the Domain Model

## 1. Validation of Your Current Understanding

Your interpretation is correct overall.

This application models a task management domain where users create, organize, track, and complete tasks over time.

You correctly identified that:

* `Task` is the core business entity
* `TaskStatus` represents workflow progression
* `isOverdue()` contains business logic tied to deadlines

Your observation about workflow states is especially important because it shows you're thinking beyond simple data storage and recognizing lifecycle behavior.

---

# 2. Core Business Concepts in the Domain

The system revolves around several important domain concepts:

## Task

A `Task` represents a unit of work a user wants to track and complete.

In business terms, the task is the central object that all user actions revolve around:

* creating work
* prioritizing work
* tracking progress
* completing work
* organizing work

The `Task` entity contains both:

* data/state
* behavior/business rules

This is a strong object-oriented design pattern because the task knows how to manage itself.

---

## Workflow / Task Lifecycle

The statuses:

* `todo`
* `in_progress`
* `review`
* `done`

represent a workflow lifecycle.

This means tasks move through stages as work progresses.

Business meaning:

* `todo` → work has not started
* `in_progress` → someone is actively working
* `review` → work is awaiting verification/checking
* `done` → work is completed

This is more than just labels — it models a real business process.

---

## Priority

Priority represents urgency or importance.

The reason priorities are stored as numbers instead of strings is likely because numeric values are easier to:

* sort
* compare
* filter
* rank

Example:

```js id="mqe5lm"
HIGH > MEDIUM
```

is easier with numbers:

```js id="uxgjjv"
3 > 2
```

rather than comparing text values.

This is a very common design choice in business applications.

---

## Deadlines and Time Management

The domain strongly emphasizes time tracking:

* `dueDate`
* `createdAt`
* `updatedAt`
* `completedAt`

These fields support business features like:

* overdue detection
* productivity tracking
* reporting/statistics
* audit history

The method:

```js id="6qqqca"
isOverdue()
```

encodes an actual business rule:

> A task is overdue if its due date has passed AND it is not completed.

This is important because business rules should live close to the entity they belong to.

---

## Tags

Tags represent categorization and organization.

Right now they are simple strings, but in larger systems tags often become full entities with:

* IDs
* colors
* ownership
* permissions
* relationships

So your intuition that tags might eventually become their own entity is very good architectural thinking.

---

# 3. Relationships Between Entities (Business Perspective)

## Task ↔ TaskStatus

Business meaning:
A task always exists in a current stage of work.

Status answers:

> "Where is this work item in its lifecycle?"

This relationship enables features like:

* filtering tasks
* workflow dashboards
* progress tracking

---

## Task ↔ TaskPriority

Business meaning:
Every task has a level of urgency.

Priority answers:

> "How important is this work compared to other work?"

This supports:

* task ordering
* decision-making
* workload management

---

## Task ↔ Time Fields

Business meaning:
Tasks evolve over time.

The timestamps provide:

* accountability
* history
* analytics
* scheduling intelligence

---

# 4. Important Domain Terminology

## Entity

A business object with identity.

Example:

```txt id="kgj7ow"
Task
```

Tasks are entities because each task has a unique ID and lifecycle.

---

## Workflow

The sequence of stages work moves through.

In this application:

```txt id="1k0chz"
todo → in_progress → review → done
```

---

## Business Rule

Logic enforcing real-world behavior.

Example:

```txt id="q3u7sl"
Completed tasks cannot be overdue
```

---

## State Transition

Movement from one status to another.

Example:

```txt id="vt2g4h"
todo → in_progress
```

---

## Domain Logic

Logic that represents business behavior instead of technical behavior.

Example:

```js id="kp0l3d"
markAsDone()
```

This is domain logic because it reflects real-world completion behavior.

---

# 5. Connection to User-Facing Features

| Domain Concept | User Feature             |
| -------------- | ------------------------ |
| Task           | Create/manage tasks      |
| Status         | Kanban/workflow tracking |
| Priority       | Sort urgent work         |
| Due Date       | Deadlines/reminders      |
| Tags           | Organize/filter tasks    |
| Overdue Logic  | Alerts and reporting     |
| completedAt    | Productivity statistics  |
| updatedAt      | Activity tracking        |

The domain model directly powers everything the user experiences.

Good software architecture connects business concepts cleanly to user features, and this project does that fairly well.

---

# Questions to Test Your Understanding

## Question 1

Why is the `Task` entity responsible for determining whether it is overdue instead of placing that logic inside `TaskStorage`?

---

## Question 2

If the application later added team collaboration, which parts of the current domain model would likely need to change?

---

## Question 3

Why might it be dangerous for external code to directly modify task fields without using methods like `update()` or `markAsDone()`?

---

## Question 4

Suppose a business rule says:

> "Urgent tasks cannot remain in `todo` for more than 24 hours."

Where would that logic logically belong in this architecture?

---

## Question 5

What business insights become possible because the application tracks both `createdAt` and `completedAt`?

---

# Suggested Diagram to Sketch

Draw a central `Task` box in the middle.

Then connect:

* `TaskStatus`
* `TaskPriority`
* `Tags`
* `Time Tracking`

to the task.

Your sketch could look like:

```txt
               +----------------+
               | TaskStatus     |
               +----------------+
                        ^
                        |
+------------+   +-------------+   +--------------+
| Tags       |-->|    Task     |<--| TaskPriority |
+------------+   +-------------+   +--------------+
                        |
                        v
               +----------------+
               | Time Tracking  |
               | createdAt      |
               | updatedAt      |
               | dueDate        |
               | completedAt    |
               +----------------+
```

This visualization helps reinforce that:

* `Task` is the core domain entity
* everything else describes or affects the task
* business behavior is centered around task lifecycle management

## -----------------------------------------------------------------------------------

