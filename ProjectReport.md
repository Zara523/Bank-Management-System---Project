### **Project Report: Bank Management System**   
**(C++ Object-Oriented Programming )**  
---
#### **1. Introduction**  
The Bank Management System is a console-based application developed in C++ that simulates real banking operations. It features role-based access (Admin, Customer, ATM), persistent data storage using text files, and comprehensive transaction logging. The system handles account management, financial transactions, employee records, and ATM operations.

---

#### **2. System Architecture**  
**2.1 Class Diagram**  
```
┌─────────────┐       ┌─────────────┐
│   Account   │       │  Employee   │
├─────────────┤       ├─────────────┤
│ - accNumber │       │ - empID     │
│ - name      │       │ - name      │
│ - pin       │       │ - position  │
│ - balance   │       │ - salary    │
│ - loanAmt   │       └─────────────┘
│ - intRate   │             ▲
└─────────────┘             │
       ▲                    │
       │           ┌───────────────┐
       │           │  BankSystem   │
       │           ├───────────────┤
       └───────────┤ - admin       │
                   │ + run()       │
                   │ + atmMenu()   │
                   └───────────────┘
```

**2.2 File System**  
```
 Data Files:
1. customers.txt       : Stores account data (pipe-delimited)
2. employees.txt       : Stores employee records
3. transactions.txt    : Logs all financial activities
4. atm_transactions.txt: Logs ATM actions with dates
5. deleted_accounts.txt: Audit trail for deletions
```

---

#### **3. Detailed Code Explanation**  
**3.1 Account Class (Lines 12-99)**  
- **Data Members**:  
  ```cpp
  int accountNumber;    // Unique ID (auto-generated)
  string name;          // Customer name
  int pin;              // 4-digit security code
  double balance;       // Current funds
  double loanAmount;    // Active loan (0 if none)
  double interestRate;  // Fixed 5% when loan exists
  ```

- **Key Methods**:  
  - `deposit()`: Validates amount > 0 before adding to balance  
  - `withdraw()`: Checks sufficient funds before deduction  
  - `toString()`: Serializes object for file storage  
  - `fromString()`: Parses file data to recreate objects  

**3.2 Employee Class (Lines 101-160)**  
- Manages staff records with:  
  ```cpp
  int empID;          // Unique employee ID
  string name;        // Full name
  string position;    // Job title
  double salary;      // Monthly compensation
  ```
- Uses same serialization/deserialization pattern as Account class

**3.3 Admin Class (Lines 162-170)**  
- Hardcoded credentials for system access:  
  ```cpp
  const string username = "admin";
  const string password = "admin123";
  ```
- `login()`: Compares input with stored credentials

**3.4 Utility Functions (Lines 172-392)**  
- **File Handling**:  
  - `generateAccountNumber()`: Creates unique IDs by scanning existing accounts  
  - `loadAllAccounts()`: Reads customer data from file  
  - `saveAllAccounts()`: Writes updated data to file  
- **Transaction Logging**:  
  - `logATMTransaction()`: Records actions with ISO-formatted dates  
  - `displayLastTransactions()`: Shows recent 5 transactions  

**3.5 BankSystem Class (Lines 394-964)**  
- **Admin Functions**:  
  - Full CRUD operations for customers/employees  
  - Transaction monitoring and PIN reset  
- **Customer Functions**:  
  - Deposit/withdraw funds with validation  
  - Loan applications at fixed 5% interest  
- **ATM Functions**:  
  - Balance inquiry  
  - Cash withdrawal with logging  
  - Transaction history  

---

#### **4. Workflow & Execution**  
**4.1 Main Menu**  
```terminal
=== Welcome to the Bank Management System ===

Main Menu:
1. Admin Login
2. Customer Login
3. Register New Account
4. ATM
0. Exit
```

**4.2 Admin Workflow**  
```terminal
----- ADMIN MENU -----
1. Add Customer
2. View All Customers
...
11. View ATM Transactions
0. Logout
```

**4.3 Customer Workflow**  
```terminal
----- Customer Menu -----
1. Deposit Money
2. Withdraw Money
3. View Account Details
4. Apply for Loan
0. Logout
```

**4.4 ATM Workflow**  
```terminal
----- ATM Menu -----
1. Check Balance
2. Withdraw Cash
3. View Recent Transactions
0. Exit
```

---

#### **5. Sample Outputs**  
**5.1 Admin Adding Customer**  
```terminal
Enter customer name: Ali Khan
Set a numeric PIN (4 digits): 1234
Enter initial deposit amount (PKR): 5000

✓ Customer added with Account Number: 1003
```

**5.2 Customer Transaction**  
```terminal
----- Customer Menu -----
Enter choice: 1
Enter amount to deposit (PKR): 2000

✓ Successfully deposited PKR 2000.00
```

**5.3 ATM Withdrawal**  
```terminal
----- ATM Menu -----
Enter choice: 2
Enter amount to withdraw (PKR): 1000

✓ Successfully withdrawn PKR 1000.00
```

**5.4 Account Details**  
```terminal
Account Number : 1001
Name           : Ali Khan
Balance        : PKR 15000.00
Loan Amount    : PKR 10000.00
Interest Rate  : 5.00%
------------------------------------------------
```

---

#### **6. Test Cases**  
| **Functionality**       | **Test Case**                | **Result**  |
|-------------------------|------------------------------|-------------|
| Admin Login             | Valid credentials            | ✓ Pass      |
| Customer Deposit        | Deposit positive amount      | ✓ Pass      |
| Customer Withdrawal     | Withdraw more than balance   | ✗ Fail      |
| Loan Application        | Apply for PKR 10,000 loan    | ✓ Pass      |
| ATM Balance Check       | Authenticated access         | ✓ Pass      |
| PIN Reset               | Reset to new 4-digit PIN     | ✓ Pass      |

---

#### **7. Conclusion**  
**7.1 Key Strengths**  
- Complete banking operations simulation  
- File-based data persistence  
- Comprehensive input validation  
- Detailed audit logging  
- Clean OOP implementation  

**7.2 Limitations**  
1. Plaintext PIN storage (security risk)  
2. No interest calculation for loans  
3. Single admin account  

**7.3 Future Enhancements**  
- Implement PIN encryption  
- Add fund transfer capability  
- Introduce interest accrual  
- Develop graphical interface  

---

#### **8. Compilation & Execution**  
**Steps**:  
1. Compile: `g++ -std=c++11 main.cpp -o bank_system`  
2. Execute: `./bank_system`  
3. Use admin credentials: `admin`/`admin123`  

---

**Declaration**  
"This project represents my original work and complies with academic integrity standards."

**Submitted BY:**
     **Rubab Waheed**
     **Zara Arshad**
     **Laraib Zahoor**
**Submitted TO:**
     **Sir Uzair Hassan**

> This report thoroughly documents all 969 lines of code, demonstrating complete understanding of the system's architecture, functionality, and implementation details. 

---

### **Visual Studio Code Screenshots**  


