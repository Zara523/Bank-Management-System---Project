Step 1: generateEmployeeID() 

➤ Purpose:

To generate a new unique Employee ID by checking the existing employee records.

➤ Explanation:

ifstream inFile("employees.txt");
int maxID = 100;

Opens the file that contains employee records.

Starts with a default max ID of 100.

if (!inFile.is_open()) {
    return maxID + 1;
}

If the file can't be opened (maybe it doesn’t exist), return 101 as the first ID.


getline(inFile, line); // Skip header
while (getline(inFile, line)) {
    if (line.empty()) continue;
    Employee emp = Employee::fromString(line);
    if (emp.getEmpID() > maxID) {
        maxID = emp.getEmpID();
    }
}
Skips the header line (first row).
Reads each line, converts it into an Employee object.
Keeps track of the highest employee ID found.

return maxID + 1;

Returns the next available ID (i.e., highest + 1).


 Step 2: loadAllEmployees()

➤ Purpose:

To load and return all employees from the employees.txt file.

vector<Employee> emps;
ifstream inFile("employees.txt");
if (!inFile.is_open()) return emps;

Creates a vector to store employees.

Opens the file; if it fails, returns an empty list.


getline(inFile, line); // Skip header

Skips the first line (column names).


while (getline(inFile, line)) {
    if (line.empty()) continue;
    Employee emp = Employee::fromString(line);
    if (emp.getEmpID() != 0) {
        emps.push_back(emp);
    }
}

Converts each line into an employee.

Adds to vector only if the ID is valid (not 0).

 Step 3: saveAllEmployees(const vector<Employee>& emps)

➤ Purpose:

To write/save all employees back into the employees.txt file.

ofstream outFile("employees.txt", ios::trunc);

Opens the file in truncate mode (clears old content).


outFile << "EmployeeID|Name|Position|Salary\n";

Writes the header row.


while (i < emps.size()) {
    outFile << emps[i].toString() << "\n";
    i++;
}

Writes each employee's data using toString().




Step 4: BankSystem Class

This class handles all Admin operations like managing customers and employees.

✅ addCustomer()

Calls generateAccountNumber() to assign a new account number.

Takes name, pin, initial balance from user input.

Creates a new Account object.

Loads all accounts, adds the new one, and saves back.


✅ viewAllCustomers()

Loads all accounts from file.

Displays a message if no accounts found.

Otherwise, shows details of each customer.


✅ modifyCustomer()

Takes an account number from the user.

Searches the account in the list.

If found, asks for new name, PIN, and balance.

Updates the object and saves all accounts back.


✅ resetCustomerPIN()

Asks for account number.

Searches for it in the account list.

Updates only the PIN if found.

✅ deleteCustomer()

Takes an account number.

Searches and deletes the account from the list.

Also calls logDeletedAccount() (not shown) to log deleted data.


✅ addEmployee()

Generates a new employee ID.

Takes name, position, and salary from the user.

Creates an Employee object.

Adds to the list and saves back.


✅ viewAllEmployees()

Loads and displays all employee records.



✅ modifyEmployee()

Takes employee ID from user.

Searches the employee.

Updates name, position, salary if found.


✅ deleteEmployee()

Takes an employee ID.

Searches and removes the employee from the list.

Saves updated list.


✅ viewAllTransactions()

Opens the transactions.txt file.

Skips the header.

Reads and splits each line into account number, transaction type, and amount.

Displays each transaction line by line.



 

This BankSystem class gives full control to the Admin to:

Manage customers: add, view, modify, reset PIN, delete.

Manage employees: add, view, modify, delete.

View all transaction history .

✅ Final Summary:

This BankSystem class gives full control to the Admin to:

Manage customers: add, view, modify, reset PIN, delete.

Manage employees: add, view, modify, delete.

View all transaction history .
