```cpp
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
int generateEmployeeID() {
    ifstream inFile("employees.txt");
    int maxID = 100;
    if (!inFile.is_open()) {
        return maxID + 1;
    }

    string line;
    getline(inFile, line); // Skip header
    while (getline(inFile, line)) {
        if (line.empty()) continue;
        Employee emp = Employee::fromString(line);
        if (emp.getEmpID() > maxID) {
            maxID = emp.getEmpID();
        }
    }
    inFile.close();
    return maxID + 1;
}

vector<Employee> loadAllEmployees() {
    vector<Employee> emps;
    ifstream inFile("employees.txt");
    if (!inFile.is_open()) return emps;

    string line;
    getline(inFile, line); // Skip header
    while (getline(inFile, line)) {
        if (line.empty()) continue;
        Employee emp = Employee::fromString(line);
        // Only add valid employees
        if (emp.getEmpID() != 0) {
            emps.push_back(emp);
        }
    }
    inFile.close();
    return emps;
}

void saveAllEmployees(const vector<Employee>& emps) {
    ofstream outFile("employees.txt", ios::trunc);
    outFile << "EmployeeID|Name|Position|Salary\n";
    size_t i = 0;
    while (i < emps.size()) {
        outFile << emps[i].toString() << "\n";
        i++;
    }
    outFile.close();
}

// =========================
// BANK SYSTEM CLASS
// =========================
class BankSystem {
private:
    Admin admin;

    // ---------- Admin Functions ----------
    void addCustomer() {
        int accNo = generateAccountNumber();
        string name;
        int pin;
        double initBalance;

        cout << "\nEnter customer name: ";
        cin.ignore();
        getline(cin, name);
        cout << "Set a numeric PIN (4 digits): ";
        cin >> pin;
        cout << "Enter initial deposit amount (PKR): ";
        cin >> initBalance;

        Account newAcc(accNo, name, pin, initBalance);
        vector<Account> accounts = loadAllAccounts();
        accounts.push_back(newAcc);
        saveAllAccounts(accounts);

        cout << "\n ? Customer added with Account Number: " << accNo << "\n";
    }

    void viewAllCustomers() {
        vector<Account> accounts = loadAllAccounts();
        if (accounts.empty()) {
            cout << "\nNo customer accounts found.\n";
            return;
        }

        cout << "\n----- All Customer Accounts -----\n";
        size_t i = 0;
        while (i < accounts.size()) {
            accounts[i].display();
            i++;
        }
    }

    void modifyCustomer() {
        int accNo;
        cout << "\nEnter Account Number to modify: ";
        cin >> accNo;

        vector<Account> accounts = loadAllAccounts();
        bool found = false;
        size_t idx = 0;
        while (idx < accounts.size()) {
            if (accounts[idx].getAccountNumber() == accNo) {
                found = true;
                string newName;
                int newPin;
                double newBalance;

                cout << "\nCurrent Name: " << accounts[idx].getName() << "\n";
                cout << "Enter new name: ";
                cin.ignore();
                getline(cin, newName);
                cout << "Enter new PIN (4 digits): ";
                cin >> newPin;
                cout << "Enter new balance (PKR): ";
                cin >> newBalance;

                accounts[idx].setName(newName);
                accounts[idx].setPin(newPin);
                accounts[idx].setBalance(newBalance);
                saveAllAccounts(accounts);
                cout << "\n ? Account updated successfully.\n";
                break;
            }
            idx++;
        }
        if (!found) {
            cout << "\n ? Account not found.\n";
        }
    }

    void resetCustomerPIN() {
        int accNo;
        cout << "\nEnter Account Number to reset PIN: ";
        cin >> accNo;

        vector<Account> accounts = loadAllAccounts();
        bool found = false;
        size_t idx = 0;
        while (idx < accounts.size()) {
            if (accounts[idx].getAccountNumber() == accNo) {
                found = true;
                int newPin;
                cout << "Enter new PIN (4 digits): ";
                cin >> newPin;
                accounts[idx].setPin(newPin);
                saveAllAccounts(accounts);
                cout << "\n ? PIN reset successfully.\n";
                break;
            }
            idx++;
        }
        if (!found) {
            cout << "\n ? Account not found.\n";
        }
    }

    void deleteCustomer() {
        int accNo;
        cout << "\nEnter Account Number to delete: ";
        cin >> accNo;

        vector<Account> accounts = loadAllAccounts();
        bool found = false;
        size_t i = 0;
        while (i < accounts.size()) {
            if (accounts[i].getAccountNumber() == accNo) {
                found = true;
                logDeletedAccount(accNo, accounts[i].getName());
                accounts.erase(accounts.begin() + i);
                saveAllAccounts(accounts);
                cout << "\n ? Account deleted successfully.\n";
                break;
            }
            i++;
        }
        if (!found) {
            cout << "\n ? Account not found.\n";
        }
    }

    void addEmployee() {
        int empId = generateEmployeeID();
        string name, position;
        double salary;

        cout << "\nEnter employee name: ";
        cin.ignore();
        getline(cin, name);
        cout << "Enter position: ";
        getline(cin, position);
        cout << "Enter salary (PKR): ";
        cin >> salary;

        Employee newEmp(empId, name, position, salary);
        vector<Employee> emps = loadAllEmployees();
        emps.push_back(newEmp);
        saveAllEmployees(emps);

        cout << "\n ? Employee added with ID: " << empId << "\n";
    }

    void viewAllEmployees() {
        vector<Employee> emps = loadAllEmployees();
        if (emps.empty()) {
            cout << "\nNo employee records found.\n";
            return;
        }

        cout << "\n----- All Employees -----\n";
        size_t i = 0;
        while (i < emps.size()) {
            emps[i].display();
            i++;
        }
    }

    void modifyEmployee() {
        int empId;
        cout << "\nEnter Employee ID to modify: ";
        cin >> empId;

        vector<Employee> emps = loadAllEmployees();
        bool found = false;
        size_t idx = 0;
        while (idx < emps.size()) {
            if (emps[idx].getEmpID() == empId) {
                found = true;
                string newName, newPos;
                double newSalary;

                cout << "\nCurrent Name: " << emps[idx].getName() << "\n";
                cout << "Enter new name: ";
                cin.ignore();
                getline(cin, newName);
                cout << "Enter new position: ";
                getline(cin, newPos);
                cout << "Enter new salary (PKR): ";
                cin >> newSalary;

                emps[idx].setName(newName);
                emps[idx].setPosition(newPos);
                emps[idx].setSalary(newSalary);
                saveAllEmployees(emps);
                cout << "\n ? Employee updated successfully.\n";
                break;
            }
            idx++;
        }
        if (!found) {
            cout << "\n ? Employee not found.\n";
        }
    }

    void deleteEmployee() {
        int empId;
        cout << "\nEnter Employee ID to delete: ";
        cin >> empId;

        vector<Employee> emps = loadAllEmployees();
        bool found = false;
        size_t i = 0;
        while (i < emps.size()) {
            if (emps[i].getEmpID() == empId) {
                found = true;
                emps.erase(emps.begin() + i);
                saveAllEmployees(emps);
                cout << "\n ? Employee deleted successfully.\n";
                break;
            }
            i++;
        }
        if (!found) {
            cout << "\n ? Employee not found.\n";
        }
    }

    void viewAllTransactions() {
        ifstream inFile("transactions.txt");
        if (!inFile.is_open()) {
            cout << "\nNo transactions found.\n";
            return;
        }

        cout << "\n----- All Transactions -----\n";
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
            
            int accNo;
            double amt;
            stringstream(tokens[0]) >> accNo;
            string type = tokens[1];
            stringstream(tokens[2]) >> amt;
            
            cout << "Account: " << accNo
                 << " | Type: " << type
                 << " | Amount: PKR " << fixed << setprecision(2) << amt << "\n";
        }
        inFile.close();
        cout << "------------------------------------------------------------\n";
    }

 
void viewATMTransactions() {
        ifstream inFile("atm_transactions.txt");
        if (!inFile.is_open()) {
            cout << "\nNo ATM transactions found.\n";
            return;
        }

        cout << "\n----- ATM Transactions -----\n";
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
            if (tokens.size() < 4) continue;
            
            int accNo;
            double amt;
            stringstream(tokens[0]) >> accNo;
            string type = tokens[1];
            stringstream(tokens[2]) >> amt;
            string date = tokens[3];
            
            cout << "Account: " << accNo
                 << " | Type: " << type
                 << " | Amount: PKR " << fixed << setprecision(2) << amt
                 << " | Date: " << date << "\n";
        }
        inFile.close();
        cout << "------------------------------------------------------------\n";
    }

    bool adminLogin() {
        string user, pass;
        cout << "\nEnter admin username: ";
        cin >> user;
        cout << "Enter admin password: ";
        cin >> pass;

        if (admin.login(user, pass)) {
            cout << "\n ? Admin login successful.\n";
            return true;
        } else {
            cout << "\n ? Invalid admin credentials.\n";
            return false;
        }
    }

    // ---------- Customer Functions ----------
    void customerMenu(int accNo) {
        vector<Account> accounts = loadAllAccounts();
        bool found = false;
        Account cust;
        size_t idx = 0;
        while (idx < accounts.size()) {
            if (accounts[idx].getAccountNumber() == accNo) {
                found = true;
                cust = accounts[idx];
                break;
            }
            idx++;
        }
        if (!found) {
            cout << "\n ? Account not found.\n";
            return;
        }

        int choice;
        do {
            cout << "\n----- Customer Menu -----\n";
            cout << "1. Deposit Money\n";
            cout << "2. Withdraw Money\n";
            cout << "3. View Account Details\n";
            cout << "4. Apply for Loan\n";
            cout << "0. Logout\n";
            cout << "Enter choice: ";
            cin >> choice;

            if (choice == 1) {
                double amt;
                cout << "\nEnter amount to deposit (PKR): ";
                cin >> amt;
                cust.deposit(amt);
                size_t i = 0;
                while (i < accounts.size()) {
                    if (accounts[i].getAccountNumber() == accNo) {
                        accounts[i].setBalance(cust.getBalance());
                        break;
                    }
                    i++;
                }
                saveAllAccounts(accounts);
                logTransaction(accNo, "Deposit", amt);
            } else if (choice == 2) {
                double amt;
                cout << "\nEnter amount to withdraw (PKR): ";
                cin >> amt;
                if (cust.withdraw(amt)) {
                    size_t i = 0;
                    while (i < accounts.size()) {
                        if (accounts[i].getAccountNumber() == accNo) {
                            accounts[i].setBalance(cust.getBalance());
                            break;
                        }
                        i++;
                    }
                    saveAllAccounts(accounts);
                    logTransaction(accNo, "Withdraw", amt);
                }
            } else if (choice == 3) {
                cout << "\n----- Account Details -----\n";
                cust.display();
            } else if (choice == 4) {
                double loanAmt;
                cout << "\nEnter loan amount to apply for (PKR): ";
                cin >> loanAmt;
                if (loanAmt > 0) {
                    double rate = 5.0; // Fixed interest rate
                    cust.setLoanAmount(loanAmt);
                    cust.setInterestRate(rate);
                    size_t i = 0;
                    while (i < accounts.size()) {
                        if (accounts[i].getAccountNumber() == accNo) {
                            accounts[i].setLoanAmount(loanAmt);
                            accounts[i].setInterestRate(rate);
                            break;
                        }
                        i++;
                    }
                    saveAllAccounts(accounts);
                    cout << "\n ? Loan approved: PKR " << fixed << setprecision(2) << loanAmt
                         << " at " << fixed << setprecision(2) << rate << "% interest.\n";
                    logTransaction(accNo, "Loan", loanAmt);
                } else {
                    cout << "\n ? Invalid loan amount.\n";
                }
            } else if (choice == 0) {
                cout << "\n ? Logging out of customer menu...\n";
            } else {
                cout << "\n ? Invalid option. Try again.\n";
            }
        } while (choice != 0);
    }

    // ---------- ATM Functions ----------
    void atmMenu() {
        int accNo, pin;
        cout << "\nATM Login - Enter Account Number: ";
        cin >> accNo;
        cout << "Enter PIN: ";
        cin >> pin;

        vector<Account> accounts = loadAllAccounts();
        bool authenticated = false;
        Account user;
        size_t idx = 0;
        while (idx < accounts.size()) {
            if (accounts[idx].getAccountNumber() == accNo && accounts[idx].getPin() == pin) {
                authenticated = true;
                user = accounts[idx];
                break;
            }
            idx++;
        }
        if (!authenticated) {
            cout << "\n ? Invalid account number or PIN.\n";
            return;
        }

        int choice;
        do {
            cout << "\n----- ATM Menu -----\n";
            cout << "1. Check Balance\n";
            cout << "2. Withdraw Cash\n";
            cout << "3. View Recent Transactions\n";
            cout << "0. Exit ATM\n";
            cout << "Enter choice: ";
            cin >> choice;

            if (choice == 1) {
                cout << "\nCurrent Balance: PKR " << fixed << setprecision(2) << user.getBalance() << "\n";
            } else if (choice == 2) {
                double amt;
                cout << "\nEnter amount to withdraw (PKR): ";
                cin >> amt;
                if (user.withdraw(amt)) {
                    size_t i = 0;
                    while (i < accounts.size()) {
                        if (accounts[i].getAccountNumber() == accNo) {
                            accounts[i].setBalance(user.getBalance());
                            break;
                        }
                        i++;
                    }
                    saveAllAccounts(accounts);
                    logATMTransaction(accNo, "Withdrawal", amt);
                    user.setBalance(user.getBalance() - amt);
                }
            } else if (choice == 3) {
                displayLastTransactions(accNo);
            } else if (choice == 0) {
                cout << "\n ? Exiting ATM...\n";
            } else {
                cout << "\n ? Invalid option. Try again.\n";
            }
        } while (choice != 0);
    }

    // Check if account exists
    bool accountExists(int accNo) {
        vector<Account> accounts = loadAllAccounts();
        size_t i = 0;
        while (i < accounts.size()) {
            if (accounts[i].getAccountNumber() == accNo) {
                return true;
            }
            i++;
        }
        return false;
    }

public:
    void run() {
        cout << "\n=== Welcome to the Bank Management System ===\n";
        while (true) {
            cout << "\nMain Menu:\n";
            cout << "1. Admin Login\n";
            cout << "2. Customer Login\n";
            cout << "3. Register New Account\n";
            cout << "4. ATM\n";
            cout << "0. Exit\n";
            cout << "Enter choice: ";

            int choice;
            cin >> choice;

            if (choice == 1) {
                if (adminLogin()) {
                    int adminChoice;
                    do {
                        cout << "\n----- ADMIN MENU -----\n";
                        cout << "1. Add Customer\n";
                        cout << "2. View All Customers\n";
                        cout << "3. Modify Customer\n";
                        cout << "4. Reset Customer PIN\n";
                        cout << "5. Delete Customer\n";
                        cout << "6. Add Employee\n";
                        cout << "7. View All Employees\n";
                        cout << "8. Modify Employee\n";
                        cout << "9. Delete Employee\n";
                        cout << "10. View All Transactions\n";
                        cout << "11. View ATM Transactions\n";
                        cout << "0. Logout\n";
                        cout << "Enter choice: ";
                        cin >> adminChoice;

                        switch (adminChoice) {
                            case 1: addCustomer(); break;
                            case 2: viewAllCustomers(); break;
                            case 3: modifyCustomer(); break;
                            case 4: resetCustomerPIN(); break;
                            case 5: deleteCustomer(); break;
                            case 6: addEmployee(); break;
                            case 7: viewAllEmployees(); break;
                            case 8: modifyEmployee(); break;
                            case 9: deleteEmployee(); break;
                            case 10: viewAllTransactions(); break;
                            case 11: viewATMTransactions(); break;
                            case 0: cout << "\n ? Admin logging out...\n"; break;
                            default: cout << "\n ? Invalid option. Try again.\n";
                        }
                    } while (adminChoice != 0);
                }
            } else if (choice == 2) {
                int accNo, pin;
                cout << "\nCustomer Login - Enter Account Number: ";
                cin >> accNo;
                
                if (!accountExists(accNo)) {
                    cout << "\n ? Account not found. Please register first.\n";
                    continue;
                }
                
                cout << "Enter PIN: ";
                cin >> pin;

                vector<Account> accounts = loadAllAccounts();
                bool found = false;
                size_t idx = 0;
                while (idx < accounts.size()) {
                    if (accounts[idx].getAccountNumber() == accNo && accounts[idx].getPin() == pin) {
                        found = true;
                        customerMenu(accNo);
                        break;
                    }
                    idx++;
                }
                if (!found) {
                    cout << "\n ? Invalid PIN. Please try again.\n";
                }
            } else if (choice == 3) {
                addCustomer();
            } else if (choice == 4) {
                atmMenu();
            } else if (choice == 0) {
                cout << "\nThank you for using the Bank Management System. Goodbye!\n";
                break;
            } else {
                cout << "\n ? Invalid choice. Try again.\n";
            }
        }
    }
};

// =========================
// MAIN FUNCTION
// =========================
int main() {
    BankSystem bank;
    bank.run();
    return 0;
}                                                                                                                                                                    
```
