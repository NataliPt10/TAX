from typing import Dict
from decimal import Decimal

def calculate_annual_expenses(expenses: Dict[str, float]) -> Decimal:
    """
    Calculate total annual expenses from monthly expense categories.

    Args:
        expenses: Dictionary with expense categories as keys and monthly amounts as values.
                 Values should be positive numbers.

    Returns:
        Total annual expense amount as Decimal for precise calculation.

    Raises:
        ValueError: If expenses contain negative values or non-numeric data.
        TypeError: If input is not a dictionary.
    """
    if not isinstance(expenses, dict):
        raise TypeError("Expenses must be a dictionary")

    if not expenses:
        return Decimal('0.0')

    # Validate input data
    for category, amount in expenses.items():
        if not isinstance(amount, (int, float)):
            raise ValueError(f"Expense amount for '{category}' must be a number, got {type(amount)}")
        if amount < 0:
            raise ValueError(f"Expense amount for '{category}' cannot be negative: {amount}")

    # Use Decimal for precise financial calculations
    total_annual = sum(Decimal(str(expense)) * 12 for expense in expenses.values())
    return total_annual

def main():
    """Main function to demonstrate expense calculation."""
    monthly_expenses = {
        "Rent": 1200.00,
        "Utilities": 150.50,
        "Internet": 49.99,
        "Groceries": 400.00,
        "Transportation": 100.75,
        "Entertainment": 200.00
    }

    try:
        total = calculate_annual_expenses(monthly_expenses)
        # Format output with 2 decimal places and thousands separator
        formatted_total = f"${total:,.2f}"
        print(f"Total annual expenses: {formatted_total}")
        
        # Optional: Print breakdown
        print("\nMonthly expense breakdown:")
        for category, amount in monthly_expenses.items():
            annual_amount = Decimal(str(amount)) * 12
            print(f"{category:>13}: ${amount:>7.2f} monthly / ${annual_amount:>8.2f} yearly")
            
    except (ValueError, TypeError) as e:
        print(f"Error calculating expenses: {e}")

if __name__ == "__main__":
    main()
