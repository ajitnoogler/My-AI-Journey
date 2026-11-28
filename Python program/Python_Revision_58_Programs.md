<h1 align="center">Python from Basics to Regex</h1>

<p align="center">Complete Python revision with hands-on practice. Beginner friendly, ready-to-run examples covering variables, conditions, loops, data structures, functions, file handling, exception handling, regular expressions and mini projects.</p>

<p align="center"><img src="https://img.shields.io/badge/programs-58-blue" alt="programs"> <img src="https://img.shields.io/badge/modules-9-green" alt="modules"> <img src="https://img.shields.io/badge/level-Beginner%20to%20Intermediate-purple" alt="level"> <img src="https://img.shields.io/badge/language-Python-yellow" alt="python"></p>

---

## Course Contents

| # | Module | Covers | Programs |
|---|--------|--------|----------|
| 01 | [Python Basics](#01-python-basics) | Variables, input, operators and strings | 7 |
| 02 | [Conditional Statements](#02-conditional-statements) | if, elif, else and nested conditions | 6 |
| 03 | [Loops](#03-loops) | for, while, break, continue and patterns | 9 |
| 04 | [Data Structures](#04-data-structures) | Lists, tuples, sets and dictionaries | 9 |
| 05 | [Functions and Lambda](#05-functions-and-lambda) | Reusable code, arguments and return | 6 |
| 06 | [File Handling](#06-file-handling) | Read, write, append and line-by-line processing | 6 |
| 07 | [Exception Handling](#07-exception-handling) | try, except, else, finally and raise | 5 |
| 08 | [Regular Expressions](#08-regular-expressions) | Search, findall, match and validation | 7 |
| 09 | [Revision Mini Projects](#09-revision-mini-projects) | Combine topics in real-world tasks | 3 |

> **Teaching tip:** Run each program, ask students to predict the output, then change the inputs. File-handling examples create files in the current folder. The log-reading example needs a local file named `sample log`. Regular-expression examples for IP and DOB demonstrate matching patterns, not full validation.

> Use your browser find (Ctrl+F or Cmd+F) to search for a topic, for example "prime", "list", "regex" or "file".

---

## 01. Python Basics

*Variables, input, operators and strings · 7 programs*

### #01 Hello Python

```python
print("Hello, Python!")
```

### #02 Variables and data types

```python
name = "Ravi"
age = 22
salary = 25000.5
is_active = True
print(name, age, salary, is_active)
print(type(age), type(salary))
```

### #03 User input

```python
name = input("Enter your name: ")
print("Welcome", name)
```

### #04 Type casting

```python
a = "10"
b = "20"
print(int(a) + int(b))
```

### #05 Arithmetic operators

```python
a, b = 17, 5
print("Add:", a + b)
print("Divide:", a / b)
print("Floor:", a // b)
print("Remainder:", a % b)
print("Power:", a ** b)
```

### #06 Swap two variables

```python
a, b = 10, 20
a, b = b, a
print(a, b)
```

### #07 String slicing

```python
text = "Python Programming"
print(text[:6])
print(text[-11:])
print(text[::-1])
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 02. Conditional Statements

*if, elif, else and nested conditions · 6 programs*

### #08 Positive, negative or zero

```python
n = int(input("Enter number: "))
if n > 0:
    print("Positive")
elif n < 0:
    print("Negative")
else:
    print("Zero")
```

### #09 Even or odd

```python
n = int(input("Enter number: "))
print("Even" if n % 2 == 0 else "Odd")
```

### #10 Largest of three

```python
a, b, c = 10, 25, 15
if a >= b and a >= c:
    print(a)
elif b >= a and b >= c:
    print(b)
else:
    print(c)
```

### #11 Grade calculator

```python
marks = 82
if marks >= 90:
    grade = "A"
elif marks >= 75:
    grade = "B"
elif marks >= 50:
    grade = "C"
else:
    grade = "Fail"
print(grade)
```

### #12 Leap year

```python
year = 2028
if year % 400 == 0 or (year % 4 == 0 and year % 100 != 0):
    print("Leap year")
else:
    print("Not a leap year")
```

### #13 Nested if login

```python
username = "admin"
password = "1234"
if username == "admin":
    if password == "1234":
        print("Login successful")
    else:
        print("Wrong password")
else:
    print("Unknown user")
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 03. Loops

*for, while, break, continue and patterns · 9 programs*

### #14 Numbers 1 to 10

```python
for i in range(1, 11):
    print(i)
```

### #15 Even numbers 1 to 20

```python
for i in range(2, 21, 2):
    print(i)
```

### #16 Sum of first N numbers

```python
n = 10
total = 0
for i in range(1, n + 1):
    total += i
print(total)
```

### #17 Multiplication table

```python
n = 7
for i in range(1, 11):
    print(f"{n} x {i} = {n * i}")
```

### #18 Factorial using for

```python
n = 5
fact = 1
for i in range(1, n + 1):
    fact *= i
print(fact)
```

### #19 While countdown

```python
n = 5
while n > 0:
    print(n)
    n -= 1
print("Go!")
```

### #20 Break and continue

```python
for i in range(1, 8):
    if i == 3:
        continue
    if i == 6:
        break
    print(i)
```

### #21 Star triangle

```python
for i in range(1, 6):
    print("*" * i)
```

### #22 Prime number check

```python
n = 29
if n < 2:
    print("Not prime")
else:
    for i in range(2, int(n ** 0.5) + 1):
        if n % i == 0:
            print("Not prime")
            break
    else:
        print("Prime")
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 04. Data Structures

*Lists, tuples, sets and dictionaries · 9 programs*

### #23 List add, update, remove

```python
names = ["Ravi", "Kiran"]
names.append("Anil")
names[0] = "Ravi Kumar"
names.remove("Kiran")
print(names)
```

### #24 List sum and average

```python
marks = [80, 90, 75, 85]
print("Sum:", sum(marks))
print("Average:", sum(marks) / len(marks))
```

### #25 Remove duplicate values

```python
numbers = [1, 2, 2, 3, 1, 4]
unique = list(dict.fromkeys(numbers))
print(unique)
```

### #26 List comprehension

```python
squares = [n * n for n in range(1, 6)]
print(squares)
```

### #27 Tuple indexing and unpacking

```python
student = (101, "Ravi", 22)
id, name, age = student
print(id, name, age)
```

### #28 Set union and intersection

```python
a = {1, 2, 3}
b = {3, 4, 5}
print("Union:", a | b)
print("Common:", a & b)
```

### #29 Dictionary CRUD

```python
student = {"name": "Ravi", "age": 22}
student["city"] = "Hyderabad"
student["age"] = 23
print(student.get("name"))
student.pop("city")
print(student)
```

### #30 Count word frequency

```python
words = ["python", "sql", "python", "azure", "sql"]
counts = {}
for word in words:
    counts[word] = counts.get(word, 0) + 1
print(counts)
```

### #31 Sort students by marks

```python
students = [("Ravi", 75), ("Kiran", 90), ("Anil", 80)]
print(sorted(students, key=lambda s: s[1], reverse=True))
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 05. Functions and Lambda

*Reusable code, arguments and return · 6 programs*

### #32 Function with parameters

```python
def greet(name):
    print("Hello", name)
greet("Ravi")
```

### #33 Return a value

```python
def add(a, b):
    return a + b
print(add(10, 20))
```

### #34 Default arguments

```python
def welcome(name, course="Python"):
    print(name, "is learning", course)
welcome("Kiran")
welcome("Ravi", "PySpark")
```

### #35 Arbitrary arguments

```python
def total(*numbers):
    return sum(numbers)
print(total(10, 20, 30))
```

### #36 Lambda function

```python
square = lambda n: n * n
print(square(6))
```

### #37 Map and filter

```python
numbers = [1, 2, 3, 4, 5, 6]
print(list(map(lambda n: n * 2, numbers)))
print(list(filter(lambda n: n % 2 == 0, numbers)))
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 06. File Handling

*Read, write, append and line-by-line processing · 6 programs*

### #38 Write a text file

```python
with open("students.txt", "w") as file:
    file.write("Ravi\nKiran\nAnil\n")
print("File created")
```

### #39 Read complete file

```python
with open("students.txt", "r") as file:
    print(file.read())
# Run the write example first.
```

### #40 Read line by line

```python
with open("students.txt", "r") as file:
    for line in file:
        print(line.strip())
# Run the write example first.
```

### #41 Append to a file

```python
with open("students.txt", "a") as file:
    file.write("Priya\n")
print("New name added")
```

### #42 Count lines in a file

```python
with open("students.txt", "r") as file:
    count = sum(1 for line in file)
print("Total lines:", count)
```

### #43 Create and read CSV

```python
import csv
with open("employees.csv", "w", newline="") as file:
    writer = csv.writer(file)
    writer.writerow(["id", "name", "salary"])
    writer.writerow([1, "Ravi", 25000])
    writer.writerow([2, "Kiran", 30000])
with open("employees.csv", "r", newline="") as file:
    for row in csv.reader(file):
        print(row)
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 07. Exception Handling

*try, except, else, finally and raise · 5 programs*

### #44 Handle division by zero

```python
try:
    a = 10
    b = 0
    print(a / b)
except ZeroDivisionError:
    print("Cannot divide by zero")
```

### #45 Handle invalid input

```python
try:
    age = int(input("Enter age: "))
    print("Age:", age)
except ValueError:
    print("Please enter a valid number")
```

### #46 Handle missing file

```python
try:
    with open("missing.txt", "r") as file:
        print(file.read())
except FileNotFoundError:
    print("File not found")
```

### #47 Else and finally

```python
try:
    result = 20 / 4
except ZeroDivisionError:
    print("Error")
else:
    print("Result:", result)
finally:
    print("Finished")
```

### #48 Raise a custom error

```python
def check_age(age):
    if age < 18:
        raise ValueError("Must be at least 18")
    return "Eligible"
try:
    print(check_age(16))
except ValueError as error:
    print(error)
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 08. Regular Expressions

*Search, findall, match and validation · 7 programs*

### #49 Find digits in text

```python
import re
text = "Order 123 costs 450 rupees"
print(re.findall(r"\d+", text))
```

### #50 Extract IPv4-shaped address

```python
import re
line = "LOGIN_SUCCESS ip=192.168.1.15 user=Ravi"
ip = re.search(r"ip=(\d+\.\d+\.\d+\.\d+)", line)
if ip:
    print(ip.group(1))
# Pattern matches shape; does not validate 0–255.
```

### #51 Find email addresses

```python
import re
text = "Contact ravi@example.com or kiran@test.org"
print(re.findall(r"[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,}", text))
```

### #52 Validate Indian mobile number

```python
import re
mobile = "9876543210"
if re.fullmatch(r"[6-9]\d{9}", mobile):
    print("Valid Indian mobile format")
else:
    print("Invalid mobile format")
```

### #53 Validate DOB format

```python
import re
dob = "25-11-2002"
if re.fullmatch(r"\d{2}-\d{2}-\d{4}", dob):
    print("Correct DD-MM-YYYY format")
else:
    print("Wrong format")
# Format only: does not check actual calendar dates.
```

### #54 Replace extra spaces

```python
import re
text = "Python    is   easy"
print(re.sub(r"\s+", " ", text))
```

### #55 Extract IP and login status from log

```python
import re
data = []
with open("sample log", "r") as txt:
    for line in txt:
        ip = re.search(r"ip=(\d+\.\d+\.\d+\.\d+)", line)
        if ip:
            status = "SUCCESS" if "LOGIN_SUCCESS" in line else "ERROR"
            data.append([ip.group(1), status])
for row in data:
    print(row)
# Needs a file named sample log in the working folder.
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

## 09. Revision Mini Projects

*Combine topics in real-world tasks · 3 programs*

### #56 Student marks report

```python
students = {"Ravi": 82, "Kiran": 46, "Anil": 91}
for name, marks in students.items():
    status = "PASS" if marks >= 50 else "FAIL"
    print(name, marks, status)
```

### #57 Word counter from text file

```python
with open("revision.txt", "w") as file:
    file.write("Python is easy. Python is powerful.")
with open("revision.txt", "r") as file:
    words = file.read().lower().split()
print("Words:", len(words))
```

### #58 Log report to CSV

```python
import re
import csv
logs = ["LOGIN_SUCCESS ip=192.168.1.15", "LOGIN_FAILURE ip=10.0.0.21"]
with open("log_report.csv", "w", newline="") as file:
    writer = csv.writer(file)
    writer.writerow(["IP Address", "Status"])
    for line in logs:
        ip = re.search(r"ip=(\d+\.\d+\.\d+\.\d+)", line)
        if ip:
            status = "SUCCESS" if "LOGIN_SUCCESS" in line else "ERROR"
            writer.writerow([ip.group(1), status])
print("Saved log_report.csv")
```

<p align="right"><a href="#course-contents">Back to top</a></p>

---

