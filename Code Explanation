	Header Files
#include <iostream>
#include <fstream>
#include <sstream>
#include <string>
#include <vector>
#include <iomanip>
#include <ctime>
These standard C++ headers provide support for:
•	iostream: Input/output operations.
•	fstream: File handling (read/write).
•	sstream: String stream manipulation.
•	string: String handling.
•	vector: Dynamic array (used to store account/employee objects).
•	iomanip: Formatting output (e.g., setting precision).
•	ctime: Getting system date and time (for transaction logs).
________________________________________
	Account Class
Represents a bank customer account.
Private Data Members:
int accountNumber;
string name;
int pin;
double balance;
double loanAmount;
double interestRate;
These store the user's account details.
Constructors:
Account();
Account(int accNo, const string& nm, int p, double bal);
Default and parameterized constructors initialize account objects.
Getters and Setters:
Provide access and modification of private members.
Core Functional Methods:
•	deposit(double amount): Adds amount to balance.
•	withdraw(double amount): Deducts amount from balance if sufficient funds are available.
•	display(): Shows all account details.
•	toString(): Converts account details to a |-separated string for file storage.
•	fromString(string line): Parses a string to create an Account object.
________________________________________
	Employee Class
Represents a bank employee.
Private Members:
int empID;
string name;
string position;
double salary;
Key Methods:
•	display(): Outputs employee details.
•	toString() / fromString(): Serialization and deserialization from file format.
________________________________________
	Admin Class
Used for admin authentication.
Private Members:
const string username = "admin";
const string password = "admin123";
Public Method:
bool login(const string& user, const string& pass) const;
Checks input credentials against hardcoded ones.
________________________________________
	Utility Functions

	generateAccountNumber():
Generates a unique new account number by checking the maximum account number in customers.txt.
	loadAllAccounts():
Loads all accounts from the customers.txt file into a vector of Account objects.
	saveAllAccounts(vector<Account>):
Writes the list of Account objects back to the file with a header and formatted output.
	logTransaction(int accNo, string type, double amt):
Logs basic deposit/withdrawal actions into transactions.txt.
	logATMTransaction(int accNo, string type, double amt):
Logs ATM-based transactions with a date into atm_transactions.txt.
	logDeletedAccount(int accNo, string name):
Records deleted accounts along with the deletion date into deleted_accounts.txt.
	displayLastTransactions(int accNo):
Reads and displays the last 5 transactions for a specific account from transactions.txt.
________________________________________
	File Structure and Formats
1.	customers.txt:
AccountNumber|Name|PIN|Balance|LoanAmount|InterestRate
2.	transactions.txt:
AccountNumber|Type|Amount
3.	atm_transactions.txt:
AccountNumber|Type|Amount|Date
4.	deleted_accounts.txt:
AccountNumber|Name|DeletionDate
________________________________________
	Code Design Highlights
•	Encapsulation: All sensitive data is kept private.
•	Data Persistence: Data is saved to and loaded from files.
•	Date Logging: Uses ctime to record transaction dates.
•	Error Handling: Invalid operations are gracefully handled (e.g., empty lines or invalid input).

