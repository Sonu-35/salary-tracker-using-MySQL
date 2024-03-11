#salary-tracker-using-MySQL

-- TABLE CREATION SECTION

-- Creating the 'workers' table
CREATE TABLE workers ( 
    id INT NOT NULL AUTO_INCREMENT,
    first_name VARCHAR(255) NOT NULL,
    last_name VARCHAR(255) NOT NULL,
    hourly_pay INT NOT NULL,
    salary INT,
    JOB VARCHAR(255) NOT NULL,
    PRIMARY KEY(id)
) ENGINE = INNODB;

-- Creating the 'expenses' table
CREATE TABLE expenses (
    id INT NOT NULL AUTO_INCREMENT,
    expense_name VARCHAR(255) NOT NULL,
    expense_total DECIMAL(10, 2) NOT NULL,
    PRIMARY KEY(id)
) ENGINE = INNODB;

-- INSERTION SECTION

-- Inserting dummy data into the 'workers' table
INSERT INTO workers (first_name, last_name, hourly_pay, salary, JOB)
VALUES ("san", "FE", 50000, NULL, "FE"),
       ("san", "BE", 100000, NULL, "BE"),
       ("san", "ryuu", 200000, NULL, "FS");

-- Inserting initial data into the 'expenses' table
INSERT INTO expenses (expense_name, expense_total)
VALUES ("salary", 0),
       ("supplies", 0),
       ("taxes", 0);

-- SELECT SECTION

-- Selecting all data from the 'workers' table
SELECT * FROM workers;

-- Selecting all data from the 'expenses' table
SELECT * FROM expenses;

-- UPDATE SECTION

-- Updating data in the 'workers' table
UPDATE workers
SET hourly_pay = 51000
WHERE id = 1;

-- Updating all data in the 'workers' table
UPDATE workers
SET hourly_pay = hourly_pay + 1;

-- Updating data in the 'expenses' table
UPDATE expenses
SET expense_total = (SELECT SUM(salary) FROM workers)
WHERE expense_name = "salary";

-- DELETE SECTION

-- Deleting data from the 'workers' table
DELETE FROM workers
WHERE id = 3;

-- TRIGGER SECTION

-- Creating triggers
CREATE TRIGGER before_hourly_pay_update
BEFORE UPDATE ON workers
FOR EACH ROW
SET NEW.salary = (NEW.hourly_pay * 160);

CREATE TRIGGER before_hourly_pay_insert
BEFORE INSERT ON workers
FOR EACH ROW
SET NEW.salary = (NEW.hourly_pay * 160);

CREATE TRIGGER after_salary_delete
AFTER DELETE ON workers
FOR EACH ROW
UPDATE expenses
SET expense_total = expense_total - OLD.salary
WHERE expense_name = "salary";

CREATE TRIGGER after_salary_insert
AFTER INSERT ON workers
FOR EACH ROW
UPDATE expenses
SET expense_total = expense_total + NEW.salary
WHERE expense_name = "salary";

CREATE TRIGGER after_salary_update
AFTER UPDATE ON workers
FOR EACH ROW
UPDATE expenses
SET expense_total = expense_total + (NEW.salary - OLD.salary)
WHERE expense_name = "salary";

-- SHOW TRIGGERS SECTION

-- Showing all triggers
SHOW TRIGGERS;
