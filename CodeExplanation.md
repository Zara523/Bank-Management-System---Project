
# 💳 Bank Management System – C++ OOP Project

## ✅ Header Files

```cpp
#include <iostream>
#include <fstream>
#include <sstream>
#include <string>
#include <vector>
#include <iomanip>
#include <ctime>
These standard C++ headers provide support for:

iostream: Input/output operations

fstream: File handling (read/write)

sstream: String stream manipulation

string: String handling

vector: Dynamic array (used to store account/employee objects)

iomanip: Formatting output (e.g., setting precision)

ctime: Getting system date and time (for transaction logs)

✅ 1. Account Class
Represents a bank customer account.

🔐 Private Data Members
cpp
Copy
Edit
int accountNumber;
string name;
int pin;
double balance;
double loanAmount;
double interestRate;
These store the user's account details.

⚙️ Constructors
cpp
Copy
Edit
Account();
Account(int accNo, const string& nm, int p, double bal);
Default and parameterized constructors initialize account objects.

🧾 Getters and Setters
Used to access and modify private data members.

🧠 Core Functional Methods
deposit(double amount): Adds amount to balance

withdraw(double amount): Deducts amount from balance if sufficient funds

display(): Shows all account details

toString(): Converts account details to a |-separated string

fromString(string line): Parses a string to create an Account object

✅ 2. Employee Class
Represents a bank employee.

🔐 Private Members
cpp
Copy
Edit
int empID;
string name;
string position;
double salary;
🔧 Key Methods
display(): Outputs employee details

toString() / fromString(): Converts to and from string for file storage

✅ 3. Admin Class
Used for admin authentication.

🔐 Private Members
cpp
Copy
Edit
const string username = "admin";
const string password = "admin123";
🔓 Public Method
cpp
Copy
Edit
bool login(const string& user, const string& pass) const;
Checks input credentials against hardcoded ones.

✅ 4. Utility Functions
🔹 generateAccountNumber()
Generates a new unique account number by checking the max in customers.txt.

🔹 loadAllAccounts()
Loads all accounts from file into a vector of Account objects.

🔹 saveAllAccounts(vector<Account>)
Writes the list of Account objects to customers.txt.

🔹 logTransaction(int accNo, string type, double amt)
Logs deposit/withdrawal into transactions.txt.

🔹 logATMTransaction(int accNo, string type, double amt)
Logs ATM-based actions into atm_transactions.txt.

🔹 logDeletedAccount(int accNo, string name)
Records account deletion into deleted_accounts.txt.

🔹 displayLastTransactions(int accNo)
Shows last 5 transactions from transactions.txt.

✅ File Structure and Formats
📄 customers.txt
text
Copy
Edit
AccountNumber|Name|PIN|Balance|LoanAmount|InterestRate
📄 transactions.txt
text
Copy
Edit
AccountNumber|Type|Amount
📄 atm_transactions.txt
text
Copy
Edit
AccountNumber|Type|Amount|Date
📄 deleted_accounts.txt
text
Copy
Edit
AccountNumber|Name|DeletionDate
✅ Code Design Highlights
Encapsulation: All sensitive data is private

Data Persistence: Uses files for storing data

Date Logging: Uses ctime for transaction dates

Error Handling: Gracefully handles invalid inputs and file errors

