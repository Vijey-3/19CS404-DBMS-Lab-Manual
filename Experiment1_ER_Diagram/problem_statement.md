# ER Diagram Workshop – Submission Template

## Objective
To understand and apply ER modeling concepts by creating ER diagrams for real-world applications.

## Purpose
Gain hands-on experience in designing ER diagrams that represent database structure including entities, relationships, attributes, and constraints.

---

# Scenario A: City Fitness Club Management

**Business Context:**  
FlexiFit Gym wants a database to manage its members, trainers, and fitness programs.

**Requirements:**  
- Members register with name, membership type, and start date.  
- Each member can join multiple programs (Yoga, Zumba, Weight Training).  
- Trainers assigned to programs; a program may have multiple trainers.  
- Members may book personal training sessions with trainers.  
- Attendance recorded for each session.  
- Payments tracked for memberships and sessions.

### ER Diagram:
 
<img width="1702" height="512" alt="Er diagram" src="https://github.com/user-attachments/assets/87b078dd-99e3-4eef-adb0-9595ee4cec84" />

---

### Entities and Attributes

| Entity  | Attributes (PK, FK)                                                     | Notes                                  |
| ------- | ----------------------------------------------------------------------- | -------------------------------------- |
| Member  | **Member_ID (PK)**, Name, Membership_Type, Start_Date                   | Stores gym member details              |
| Trainer | **Trainer_ID (PK)**, Name, Specialization, Contact_No                   | Stores trainer information             |
| Program | **Program_ID (PK)**, Program_Name, Description                          | Contains fitness program details       |
| Session | **Session_ID (PK)**, Date, Time, Start_Date                             | Represents training sessions           |
| Payment | **Payment_ID (PK)**, Amount, Payment_Date, Payment_Type, Member_ID (FK) | Stores payment records made by members |

---

### Relationships and Constraints

| Relationship                 | Cardinality | Participation         | Notes                                      |
| ---------------------------- | ----------- | --------------------- | ------------------------------------------ |
| Member — Register — Program  | M:N         | Total on Member side  | Members can register for multiple programs |
| Trainer — Assigned — Program | M:N         | Partial               | Trainers may handle multiple programs      |
| Member — Book — Session      | M:N         | Partial               | Members can book many sessions             |
| Trainer — Host — Session     | M:N         | Partial               | Trainers can host multiple sessions        |
| Member — Pay — Payment       | 1:N         | Total on Payment side | Each payment belongs to one member         |

---

### Assumptions
- Each entity has a unique primary key.
- Members can enroll in more than one fitness program.
- Trainers may conduct both group and personal sessions.
- Payments include membership fees and session charges.
- Sessions can be attended by multiple members.
- Attendance details are maintained separately or within session records.

---


