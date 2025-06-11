# Bank-Management-System---Project
 #  C++ Object-Oriented Programming Project Proposal  
##  Bank Management System

## Overview

This project is a *fully-featured, real-world inspired Bank Management System, developed using **C++ and Object-Oriented Programming principles. Designed to mimic online and ATM-based banking functionality, it includes complete role-based access for **Admin* and *Customers*, secure login systems, and transaction features with persistent file handling.  

This system is suitable for academic submission, portfolio showcase, and concept demonstration in banking automation projects.

##  Login System

- *Admin Login:* Hardcoded secure credentials
- *Customer Login:* CNIC + 4-digit PIN for secure access
- *Attempts Limited:* Simulates ATM PIN lockout behavior

##  Admin Features

- Add/View/Update/Delete Customers
- Add and Manage Employees
- View All Accounts and Their Balances
- Check Loan Requests and Issue Approvals
- Track Complete Transaction Histories
- Full Access to All System Data (unlike Customers)

##  Customer Features

- Create New Bank Account
- Deposit and Withdraw Money
- Apply for Loan
- Check Account Balance
- View Transaction History
- Secure login with CNIC & PIN
- Access limited to their own data only

##  ATM Simulation

- PIN-based authentication
- ATM-style features: balance inquiry, deposit, withdrawal
- Locked after 3 wrong attempts

##  File Handling

- All data is stored in local files for persistence
- Uses fstream, ifstream, and ofstream for real-time simulation
- Secure read/write/update/delete operations for customers and employees

##  Tech Stack

| Tool | Description |
|------|-------------|
| *Language* | C++ |
| *Paradigm* | Object-Oriented Programming |
| *Compiler* | Dev C++, Visual Studio Code |
| *Core Concepts* | Classes, Inheritance, Encapsulation, File Handling |
| *File Storage* | Local .txt files with customer and admin data |
---
##  Purpose

This project was developed as a *professional OOP-based system* that:
- Showcases advanced understanding of C++ and OOP
- Demonstrates practical banking features
- Mimics professional banking systems with role-based access
- Provides an academic-grade complete simulation

##  Authors

*Made by:*
- Rubab Waheed  
- Zara Arshad  
- Laraib Zahoor  

##  How to Run

###  Dev C++
1. Open .cpp files in Dev C++
2. Ensure all headers are included
3. Compile & Run

###  Visual Studio Code
1. Open project folder
2. Use g++ or any C++ compiler
3. Run using terminal:
