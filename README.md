import json
import datetime

EXPENSE_FILE = "expenses.json"

def load_expenses():
    """Load expenses from the file."""
    try:
        with open(EXPENSE_FILE, "r") as file:
            return json.load(file)
    except FileNotFoundError:
        return []

def save_expenses(expenses):
    """Save expenses to the file."""
    with open(EXPENSE_FILE, "w") as file:
        json.dump(expenses, file, indent=4)

def add_expense():
    """Add a new expense."""
    date = str(datetime.date.today())
    amount = float(input("Enter amount: "))
    category = input("Enter category (Food, Transport, Entertainment, etc.): ")
    description = input("Enter description: ")
    
    expense = {"date": date, "amount": amount, "category": category, "description": description}
    
    expenses = load_expenses()
    expenses.append(expense)
    save_expenses(expenses)
    print("Expense added successfully!")

def view_expenses():
    """View all expenses."""
    expenses = load_expenses()
    if not expenses:
        print("No expenses recorded.")
        return
    
    for exp in expenses:
        print(f"{exp['date']} | {exp['category']} | ${exp['amount']} | {exp['description']}")

def category_summary():
    """View expenses by category."""
    expenses = load_expenses()
    summary = {}
    
    for exp in expenses:
        category = exp['category']
        summary[category] = summary.get(category, 0) + exp['amount']
    
    print("Category-wise expenses:")
    for category, total in summary.items():
        print(f"{category}: ${total}")

def main():
    """Main menu."""
    while True:
        print("\nExpense Tracker")
        print("1. Add Expense")
        print("2. View Expenses")
        print("3. Category Summary")
        print("4. Exit")
        
        choice = input("Enter choice: ")
        if choice == "1":
            add_expense()
        elif choice == "2":
            view_expenses()
        elif choice == "3":
            category_summary()
        elif choice == "4":
            print("Exiting...")
            break
        else:
            print("Invalid choice. Try again.")
            
if __name__ == "__main__":
    main() 
