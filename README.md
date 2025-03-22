# Expense-tracker-
#include <iostream>
#include <vector>
#include <fstream>
#include <iomanip>

using namespace std;

// Struct to hold expense details
struct Expense {
    string date;
    string category;
    double amount;
};

// Function to add a new expense
void addExpense(vector<Expense> &expenses) {
    Expense e;
    cout << "Enter date (YYYY-MM-DD): ";
    cin >> e.date;
    cout << "Enter category (Food, Transport, etc.): ";
    cin >> e.category;
    cout << "Enter amount: ";
    cin >> e.amount;

    expenses.push_back(e);
    cout << "Expense added successfully!\n";
}

// Function to display all expenses
void displayExpenses(const vector<Expense> &expenses) {
    if (expenses.empty()) {
        cout << "No expenses recorded yet.\n";
        return;
    }

    cout << "\n===== Expense List =====\n";
    cout << left << setw(15) << "Date" 
         << setw(15) << "Category" 
         << setw(10) << "Amount" << endl;
    cout << "-----------------------------------\n";

    for (const auto &e : expenses) {
        cout << left << setw(15) << e.date 
             << setw(15) << e.category 
             << setw(10) << e.amount << endl;
    }
}

// Function to calculate total expenses
void calculateTotal(const vector<Expense> &expenses) {
    double total = 0;
    for (const auto &e : expenses) {
        total += e.amount;
    }
    cout << "Total Expenses: $" << fixed << setprecision(2) << total << endl;
}

// Function to save expenses to a file
void saveExpenses(const vector<Expense> &expenses) {
    ofstream file("expenses.txt");
    if (!file) {
        cout << "Error saving expenses!\n";
        return;
    }

    for (const auto &e : expenses) {
        file << e.date << " " << e.category << " " << e.amount << endl;
    }
    file.close();
    cout << "Expenses saved successfully!\n";
}

// Function to load expenses from a file
void loadExpenses(vector<Expense> &expenses) {
    ifstream file("expenses.txt");
    if (!file) return;  // No file means no previous expenses

    Expense e;
    while (file >> e.date >> e.category >> e.amount) {
        expenses.push_back(e);
    }
    file.close();
}

// Main function
int main() {
    vector<Expense> expenses;
    loadExpenses(expenses);

    int choice;
    do {
        cout << "\n===== Expense Tracker =====\n";
        cout << "1. Add Expense\n";
        cout << "2. View Expenses\n";
        cout << "3. Calculate Total Expenses\n";
        cout << "4. Save & Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
            case 1: addExpense(expenses); break;
            case 2: displayExpenses(expenses); break;
            case 3: calculateTotal(expenses); break;
            case 4: saveExpenses(expenses); cout << "Exiting...\n"; break;
            default: cout << "Invalid choice! Try again.\n";
        }
    } while (choice != 4);

    return 0;
}
