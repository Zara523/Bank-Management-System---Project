# Bank Management System in C++

## Overview
This project implements a comprehensive banking system with three user roles:
1. **Customers** - Manage accounts, transactions, and loans
2. **Employees** - Handle customer accounts and view transactions
3. **Admin** - Manage employees and view system-wide reports

## Class Structure

### Account Class
cpp
class Account {
private:
    int accountNumber;
    string name;
    int pin;
    double balance;
    double loanAmount;
    double interestRate;
public:
    // Constructor and accessors
    void deposit(double amount);
    bool withdraw(double amount);
    void display() const;
    string toString() const;
    static Account fromString(const string& line);
};


### Employee Class
cpp
class Employee {
private:
    int empID;
    string name;
    string position;
    double salary;
public:
    // Constructor and accessors
    void display() const;
    string toString() const;
    static Employee fromString(const string& line);
};


### Admin Class
cpp
class Admin {
private:
    const string username = "admin";
    const string password = "admin123";
public:
    bool login(const string& user, const string& pass) const;
};


## Key Features

### Customer Functions
1. **Account Management**:
   - View account details
   - Deposit/withdraw funds
   - Change PIN (4-digit validation)
2. **Loan Services**:
   - Apply for loans (10% interest rate)
   - Pay back loans (full payment required)
3. **Transaction History**:
   - View last 5 transactions

### Employee Functions
1. **Customer Management**:
   - Create new accounts
   - Delete accounts (with audit trail)
   - View customer details
2. **Transaction Monitoring**:
   - View customer transaction history

### Admin Functions
1. **Employee Management**:
   - Add/view/delete employees
   - Set salaries and positions
2. **System Monitoring**:
   - View ATM transaction logs
   - Audit deleted accounts
   - Access customer databases

## Data Persistence
mermaid
graph LR
    A[Customers] --> B[customers.txt]
    C[Employees] --> D[employees.txt]
    E[Transactions] --> F[transactions.txt]
    G[ATM Transactions] --> H[atm_transactions.txt]
    I[Deleted Accounts] --> J[deleted_accounts.txt]


## Utility Functions
- **Automatic ID Generation**:
  - `generateAccountNumber()`
  - `generateEmployeeID()`
- **File Handling**:
  - `loadAllAccounts()`
  - `saveAllAccounts()`
  - `logTransaction()`
  - `logATMTransaction()`
- **Search Functions**:
  - `findAccountIndex()`
  - `findEmployeeIndex()`

## Security Measures
- PIN validation (exactly 4 digits)
- Separate credentials for each user type:
  - Customers: Account number + PIN
  - Employees: Password "employee"
  - Admin: "admin"/"admin123"
- Audit trails for critical operations:
  cpp
  void logDeletedAccount(int accNo, const string& name) {
      // Records account number, name, and deletion date
  }
  

## Transaction Handling
cpp
void logATMTransaction(int accNo, const string& type, double amt) {
    // Logs with timestamp in YYYY-MM-DD format
    // Stores in atm_transactions.txt
}


## Menu System
mermaid
flowchart TD
    MainMenu --> CustomerLogin
    MainMenu --> EmployeeLogin
    MainMenu --> AdminLogin
    CustomerLogin --> CustomerMenu
    EmployeeLogin --> EmployeeMenu
    AdminLogin --> AdminMenu


## How to Use
1. **Customer Access**:
   - Login with account number/PIN
   - Perform banking operations
2. **Employee Access**:
   - Password: "employee"
   - Manage customer accounts
3. **Admin Access**:
   - Credentials: "admin"/"admin123"
   - Full system oversight

## Error Handling
- Input validation for all operations
- File existence checks before operations
- Graceful error messages:
  cpp
  cout << "\n ? Invalid or insufficient funds for withdrawal.\n";
  

## Notes
- All financial values display with 2 decimal places (PKR)
- Data files include headers for readability
- System uses standard C++ libraries for cross-platform compatibility
