# Experiment 10: PL/SQL – Triggers

## AIM
To write and execute PL/SQL trigger programs for automating actions in response to specific table events like INSERT, UPDATE, or DELETE.

---

## THEORY

A **trigger** is a stored PL/SQL block that is automatically executed or fired when a specified event occurs on a table or view. Triggers can be used for enforcing business rules, auditing changes, or automatic updates.

### Types of Triggers:
- **Before Trigger**: Executes before the operation (INSERT, UPDATE, DELETE).
- **After Trigger**: Executes after the operation.
- **Row-level Trigger**: Executes for each affected row.
- **Statement-level Trigger**: Executes once for the triggering statement.

**Basic Syntax:**
```sql
CREATE OR REPLACE TRIGGER trigger_name
BEFORE|AFTER INSERT|UPDATE|DELETE ON table_name
[FOR EACH ROW]
BEGIN
   -- trigger logic
END;
```

## 1. Write a trigger to log every insertion into a table.
**Steps:**
- Create two tables: `employees` (for storing data) and `employee_log` (for logging the inserts).
- Write an **AFTER INSERT** trigger on the `employees` table to log the new data into the `employee_log` table.

**Expected Output:**
- A new entry is added to the `employee_log` table each time a new record is inserted into the `employees` table.

### Code:
```
CREATE OR REPLACE TRIGGER trg_employee_insert
AFTER INSERT ON employee
FOR EACH ROW
BEGIN
    INSERT INTO employee_log (
        employee_id,
        employee_name,
        action_date
    )
    VALUES (
        :NEW.employee_id,
        :NEW.employee_name,
        SYSDATE
    );
END;
/
```
### Output:

<img width="853" height="154" alt="image" src="https://github.com/user-attachments/assets/640b0539-47f3-4a39-9188-fe3310dcc267" />

---

## 2. Write a trigger to prevent deletion of records from a sensitive table.
**Steps:**
- Write a **BEFORE DELETE** trigger on the `sensitive_data` table.
- Use `RAISE_APPLICATION_ERROR` to prevent deletion and issue a custom error message.

**Expected Output:**
- If an attempt is made to delete a record from `sensitive_data`, an error message is raised, e.g., `ERROR: Deletion not allowed on this table.`

### Code:
```
CREATE OR REPLACE TRIGGER trg_prevent_delete
BEFORE DELETE ON sensitive_data
FOR EACH ROW
BEGIN
    RAISE_APPLICATION_ERROR(
        -20001,
        'Deletion is not allowed from sensitive_data table'
    );
END;
/
```
### Output:

<img width="755" height="128" alt="image" src="https://github.com/user-attachments/assets/a574f0d6-7e75-45d4-8c52-cdd90934c7b0" />

---

## 3. Write a trigger to automatically update a `last_modified` timestamp.
**Steps:**
- Add a `last_modified` column to the `products` table.
- Write a **BEFORE UPDATE** trigger on the `products` table to set the `last_modified` column to the current timestamp whenever an update occurs.

**Expected Output:**
- The `last_modified` column in the `products` table is updated automatically to the current date and time when any record is updated.

### Code:
```
CREATE OR REPLACE TRIGGER trg_update_timestamp
BEFORE UPDATE ON employee_details
FOR EACH ROW
BEGIN
    :NEW.last_modified := SYSDATE;
END;
/
```
### Output:

<img width="889" height="198" alt="image" src="https://github.com/user-attachments/assets/1b383949-f4e9-4593-aa48-1c49ce0f0e41" />

---

## 4. Write a trigger to keep track of the number of updates made to a table.
**Steps:**
- Create an `audit_log` table with a counter column.
- Write an **AFTER UPDATE** trigger on the `customer_orders` table to increment the counter in the `audit_log` table every time a record is updated.

**Expected Output:**
- The `audit_log` table will maintain a count of how many updates have been made to the `customer_orders` table.

### Code:
```
CREATE OR REPLACE TRIGGER trg_count_updates
BEFORE UPDATE ON employee_records
FOR EACH ROW
BEGIN
    :NEW.update_count := NVL(:OLD.update_count, 0) + 1;
END;
/
```
### Output:

<img width="829" height="204" alt="image" src="https://github.com/user-attachments/assets/8b4a8c4d-4de1-4c3f-ba0c-a7c97c61bf7b" />

---

## 5. Write a trigger that checks a condition before allowing insertion into a table.
**Steps:**
- Write a **BEFORE INSERT** trigger on the `employees` table to check if the inserted salary meets a specific condition (e.g., salary must be greater than 3000).
- If the condition is not met, raise an error to prevent the insert.

**Expected Output:**
- If the inserted salary in the `employees` table is below the condition (e.g., salary < 3000), the insert operation is blocked, and an error message is raised, such as: `ERROR: Salary below minimum threshold.`

### Code:
```
CREATE OR REPLACE TRIGGER trg_check_salary
BEFORE INSERT ON employee_salary
FOR EACH ROW
BEGIN
    IF :NEW.salary < 10000 THEN
        RAISE_APPLICATION_ERROR(
            -20002,
            'Salary must be at least 10000'
        );
    END IF;
END;
/
```
### Output:

<img width="706" height="136" alt="image" src="https://github.com/user-attachments/assets/ded2e4ec-20aa-4e7d-b5a1-2b8d1b68b5bd" />



## RESULT
Thus, the PL/SQL trigger programs were written and executed successfully.
