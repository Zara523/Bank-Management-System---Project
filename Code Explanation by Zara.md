Code:
```
#include <iostream>
#include <fstream>
#include <sstream>
#include <string>
#include <vector>
#include <iomanip>
#include <ctime>
using namespace std;

// =========================
// CLASS DEFINITIONS
// =========================

// --- Account Class ---
class Account {
private:
    int accountNumber;
    string name;
    int pin;
    double balance;
    double loanAmount;
    double interestRate;

public:
    Account() : accountNumber(0), name(""), pin(0), balance(0.0), loanAmount(0.0), interestRate(0.0) {}
    Account(int accNo, const string& nm, int p, double bal)
        : accountNumber(accNo), name(nm), pin(p), balance(bal), loanAmount(0.0), interestRate(0.0) {}

    int getAccountNumber() const { return accountNumber; }
    const string& getName() const { return name; }
    int getPin() const { return pin; }
    double getBalance() const { return balance; }
    double getLoanAmount() const { return loanAmount; }
    double getInterestRate() const { return interestRate; }

    void setName(const string& nm) { name = nm; }
    void setPin(int p) { pin = p; }
    void setBalance(double bal) { balance = bal; }
    void setLoanAmount(double amt) { loanAmount = amt; }
    void setInterestRate(double rate) { interestRate = rate; }

    void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            cout << "\n ? Successfully deposited PKR " << fixed << setprecision(2) << amount << ".\n";
        } else {
            cout << "\n ? Invalid deposit amount.\n";
        }
    }

    bool withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            cout << "\n ? Successfully withdrawn PKR " << fixed << setprecision(2) << amount << ".\n";
            return true;
        } else {
            cout << "\n ? Invalid or insufficient funds for withdrawal.\n";
            return false;
        }
    }

    void display() const {
        cout << "\nAccount Number : " << accountNumber
             << "\nName           : " << name
             << "\nBalance        : PKR " << fixed << setprecision(2) << balance;
        if (loanAmount > 0.0) {
            cout << "\nLoan Amount    : PKR " << fixed << setprecision(2) << loanAmount
                 << "\nInterest Rate  : " << fixed << setprecision(2) << interestRate << "%\n";
        } else {
            cout << "\nNo active loan.\n";
        }
        cout << "------------------------------------------------------------\n";
    }

    string toString() const {
        ostringstream oss;
        oss << accountNumber << "|"
            << name << "|"
            << pin << "|"
            << fixed << setprecision(2) << balance << "|"
            << fixed << setprecision(2) << loanAmount << "|"
            << fixed << setprecision(2) << interestRate;
        return oss.str();
    }

    static Account fromString(const string& line) {
        vector<string> tokens;
        string token;
        istringstream iss(line);
        while (getline(iss, token, '|')) {
            tokens.push_back(token);
        }
        
        if (tokens.size() != 6) {
            return Account(); // Return empty account on error
        }

        int accNo, p;
        double bal, loanAmt, intRate;
        stringstream(tokens[0]) >> accNo;
        string nm = tokens[1];
        stringstream(tokens[2]) >> p;
        stringstream(tokens[3]) >> bal;
        stringstream(tokens[4]) >> loanAmt;
        stringstream(tokens[5]) >> intRate;

        Account acc(accNo, nm, p, bal);
        acc.setLoanAmount(loanAmt);
        acc.setInterestRate(intRate);
        return acc;
    }
};

// --- Employee Class ---
class Employee {
private:
    int empID;
    string name;
    string position;
    double salary;

public:
    Employee() : empID(0), name(""), position(""), salary(0.0) {}
    Employee(int id, const string& nm, const string& pos, double sal)
        : empID(id), name(nm), position(pos), salary(sal) {}

    int getEmpID() const { return empID; }
    const string& getName() const { return name; }
    const string& getPosition() const { return position; }
    double getSalary() const { return salary; }

    void setName(const string& nm) { name = nm; }
    void setPosition(const string& pos) { position = pos; }
    void setSalary(double sal) { salary = sal; }

    void display() const {
        cout << "\nEmployee ID : " << empID
             << "\nName        : " << name
             << "\nPosition    : " << position
             << "\nSalary      : PKR " << fixed << setprecision(2) << salary
             << "\n------------------------------------------------------------\n";
    }

    string toString() const {
        ostringstream oss;
        oss << empID << "|"
            << name << "|"
            << position << "|"
            << fixed << setprecision(2) << salary;
        return oss.str();
    }

    static Employee fromString(const string& line) {
        vector<string> tokens;
        string token;
        istringstream iss(line);
        while (getline(iss, token, '|')) {
            tokens.push_back(token);
        }
        
        if (tokens.size() != 4) {
            return Employee(); // Return empty employee on error
        }

        int id;
        double sal;
        stringstream(tokens[0]) >> id;
        string nm = tokens[1];
        string pos = tokens[2];
        stringstream(tokens[3]) >> sal;
        
        return Employee(id, nm, pos, sal);
    }
};

// --- Admin Class ---
class Admin {
private:
    const string username = "admin";
    const string password = "admin123";

public:
    bool login(const string& user, const string& pass) const {
        return (user == username && pass == password);
    }
};

// =========================
// UTILITY FUNCTIONS
// =========================

int generateAccountNumber() {
    ifstream inFile("customers.txt");
    int maxAcc = 1000;
    if (!inFile.is_open()) {
        return maxAcc + 1;
    }

    string line;
    getline(inFile, line); // Skip header
    while (getline(inFile, line)) {
        if (line.empty()) continue;
        Account acc = Account::fromString(line);
        if (acc.getAccountNumber() > maxAcc) {
            maxAcc = acc.getAccountNumber();
        }
    }
    inFile.close();
    return maxAcc + 1;
}

vector<Account> loadAllAccounts() {
    vector<Account> accounts;
    ifstream inFile("customers.txt");
    if (!inFile.is_open()) return accounts;

    string line;
    getline(inFile, line); // Skip header
    while (getline(inFile, line)) {
        if (line.empty()) continue;
        Account acc = Account::fromString(line);
        // Only add valid accounts
        if (acc.getAccountNumber() != 0) {
            accounts.push_back(acc);
        }
    }
    inFile.close();
    return accounts;
}

void saveAllAccounts(const vector<Account>& accounts) {
    ofstream outFile("customers.txt", ios::trunc);
    outFile << "AccountNumber|Name|PIN|Balance|LoanAmount|InterestRate\n";
    size_t i = 0;
    while (i < accounts.size()) {
        outFile << accounts[i].toString() << "\n";
        i++;
    }
    outFile.close();
}

void logTransaction(int accNo, const string& type, double amt) {
    ofstream outFile("transactions.txt", ios::app);
    if (outFile.tellp() == 0) {
        outFile << "AccountNumber|Type|Amount\n";
    }
    outFile << accNo << "|" << type << "|" << fixed << setprecision(2) << amt << "\n";
    outFile.close();
}

void logATMTransaction(int accNo, const string& type, double amt) {
    ofstream outFile("atm_transactions.txt", ios::app);
    if (outFile.tellp() == 0) {
        outFile << "AccountNumber|Type|Amount|Date\n";
    }
    
    // Get current date
    time_t now = time(0);
    tm* ltm = localtime(&now);
    char dateStr[20];
    strftime(dateStr, sizeof(dateStr), "%Y-%m-%d", ltm);
    
    outFile << accNo << "|" << type << "|" << fixed << setprecision(2) << amt 
            << "|" << dateStr << "\n";
    outFile.close();
}

void logDeletedAccount(int accNo, const string& name) {
    ofstream outFile("deleted_accounts.txt", ios::app);
    if (outFile.tellp() == 0) {
        outFile << "AccountNumber|Name|DeletionDate\n";
    }
    
    // Get current date
    time_t now = time(0);
    tm* ltm = localtime(&now);
    char dateStr[20];
    strftime(dateStr, sizeof(dateStr), "%Y-%m-%d", ltm);
    
    outFile << accNo << "|" << name << "|" << dateStr << "\n";
    outFile.close();
}

void displayLastTransactions(int accNo) {
    ifstream inFile("transactions.txt");
    if (!inFile.is_open()) {
        cout << "\nNo transactions found.\n";
        return;
    }

    vector<string> matched;
    string line;
    getline(inFile, line); // Skip header
    while (getline(inFile, line)) {
        if (line.empty()) continue;
        vector<string> tokens;
        string token;
        istringstream iss(line);
        while (getline(iss, token, '|')) {
            tokens.push_back(token);
        }
        if (tokens.size() < 3) continue;
        
        int id;
        double amount;
        stringstream(tokens[0]) >> id;
        string type = tokens[1];
        stringstream(tokens[2]) >> amount;
        
        if (id == accNo) {
            ostringstream oss;
            oss << type << " PKR " << fixed << setprecision(2) << amount;
            matched.push_back(oss.str());
        }
    }
    inFile.close();

    cout << "\nLast Transactions for Account " << accNo << ":\n";
    size_t start = (matched.size() > 5 ? matched.size() - 5 : 0);
    size_t i = start;
    while (i < matched.size()) {
        cout << " - " << matched[i] << "\n";
        i++;
    }
    if (matched.empty()) {
        cout << " No transactions to display.\n";
    }
    cout << "------------------------------------------------------------\n";
}
```
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
