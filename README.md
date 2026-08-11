# Vardaan-Atm
it is only for entertairment 
# 💰 Vardaan ATM

**Vardaan ATM** is a simple, colorful, and beginner-friendly personal money management system made in Python.

It was created to help **Shakuntla** maintain her money easily. The program automatically manages the balance and keeps a complete transaction history.

## ✨ Features

* 💰 **Credit Money** — Add money and record who gave it.
* 💸 **Debit Money** — Subtract money and record where it was spent.
* 💵 **Check Balance** — See the current balance anytime.
* 📜 **Transaction History** — See all credit and debit transactions.
* 🕐 **Date & Time** — Every transaction automatically gets a date and time.
* 💾 **Automatic Saving** — Balance and history are saved automatically.
* 🎨 **Colorful Interface** — Different colors for different operations.
* 🔢 **Starting Balance** — ₹1,130.
* 📦 **No External Libraries** — Uses only Python's built-in modules.

## 🧮 How It Works

The program starts with:

**₹1,130**

For example:

```text
Credit Money
Amount: ₹500
From: Papa

New Balance: ₹1,630
```

Then:

```text
Debit Money
Amount: ₹200
For: Grocery

New Balance: ₹1,430
```

The program automatically calculates and saves the new balance.

## 📜 Transaction History

Example:

```text
Transaction 1
Type   : CREDIT (+)
Amount : ₹500
From   : Papa
Date   : 12-08-2026 10:30:20 PM

Transaction 2
Type   : DEBIT (-)
Amount : ₹200
To     : Grocery
Date   : 12-08-2026 10:35:10 PM

Current Balance: ₹1,430
```

## ▶️ How To Run

Make sure Python is installed on your computer.

Save the following code as:

```text
vardaan_atm.py
```

Then open Command Prompt in the same folder and run:

```text
python vardaan_atm.py
```

## 💻 Complete Python Code

```python
import json
import os
from datetime import datetime

FILE_NAME = "vardaan_atm_data.json"


# ==============================
# COLORS
# ==============================

RESET = "\033[0m"
GREEN = "\033[92m"
RED = "\033[91m"
YELLOW = "\033[93m"
BLUE = "\033[94m"
CYAN = "\033[96m"
MAGENTA = "\033[95m"
BOLD = "\033[1m"


# ==============================
# CLEAR SCREEN
# ==============================

def clear_screen():
    os.system("cls")


# ==============================
# MONEY FORMAT
# ==============================

def money(amount):
    if amount == int(amount):
        return "Rs. " + str(int(amount))

    return "Rs. " + str(round(amount, 2))


# ==============================
# DATE AND TIME
# ==============================

def get_time():
    return datetime.now().strftime(
        "%d-%m-%Y %I:%M:%S %p"
    )


# ==============================
# SAVE DATA
# ==============================

def save_data(data):
    with open(FILE_NAME, "w") as file:
        json.dump(data, file, indent=4)


# ==============================
# LOAD DATA
# ==============================

def load_data():

    if os.path.exists(FILE_NAME):

        try:

            with open(FILE_NAME, "r") as file:
                data = json.load(file)

            if "balance" in data and "transactions" in data:
                return data

        except:
            pass

    data = {
        "balance": 1130,
        "transactions": []
    }

    save_data(data)

    return data


# ==============================
# CREDIT MONEY
# ==============================

def credit_money(data):

    clear_screen()

    print(CYAN + "=" * 60 + RESET)
    print(MAGENTA + BOLD + "                 VARDAAN ATM" + RESET)
    print(YELLOW + "              Welcome Shakuntla" + RESET)
    print(CYAN + "=" * 60 + RESET)

    while True:

        try:

            amount = float(
                input("\nEnter amount received: Rs. ")
            )

            if amount <= 0:

                print(
                    RED +
                    "Amount must be greater than 0." +
                    RESET
                )

                continue

            break

        except ValueError:

            print(
                RED +
                "Please enter a valid number." +
                RESET
            )

    person = input(
        "Money received from: "
    ).strip()

    if person == "":
        person = "Unknown"

    data["balance"] += amount

    transaction = {
        "type": "CREDIT",
        "amount": amount,
        "person": person,
        "date": get_time()
    }

    data["transactions"].append(transaction)

    save_data(data)

    print("\n" + GREEN + "=" * 60 + RESET)

    print(
        GREEN + BOLD +
        "        MONEY CREDITED SUCCESSFULLY" +
        RESET
    )

    print(GREEN + "=" * 60 + RESET)

    print(
        "Amount  :",
        GREEN + money(amount) + RESET
    )

    print(
        "From    :",
        CYAN + person + RESET
    )

    print(
        "Balance :",
        GREEN + money(data["balance"]) + RESET
    )

    print(
        "Date    :",
        BLUE + get_time() + RESET
    )

    input(
        "\nPress ENTER to continue..."
    )


# ==============================
# DEBIT MONEY
# ==============================

def debit_money(data):

    clear_screen()

    print(CYAN + "=" * 60 + RESET)
    print(MAGENTA + BOLD + "                 VARDAAN ATM" + RESET)
    print(YELLOW + "              Welcome Shakuntla" + RESET)
    print(CYAN + "=" * 60 + RESET)

    while True:

        try:

            amount = float(
                input("\nEnter amount to spend: Rs. ")
            )

            if amount <= 0:

                print(
                    RED +
                    "Amount must be greater than 0." +
                    RESET
                )

                continue

            if amount > data["balance"]:

                print(
                    RED + BOLD +
                    "\nINSUFFICIENT BALANCE!" +
                    RESET
                )

                print(
                    "Available Balance:",
                    GREEN +
                    money(data["balance"]) +
                    RESET
                )

                input(
                    "\nPress ENTER to continue..."
                )

                return

            break

        except ValueError:

            print(
                RED +
                "Please enter a valid number." +
                RESET
            )

    purpose = input(
        "Money spent on / given to: "
    ).strip()

    if purpose == "":
        purpose = "Unknown"

    data["balance"] -= amount

    transaction = {
        "type": "DEBIT",
        "amount": amount,
        "person": purpose,
        "date": get_time()
    }

    data["transactions"].append(transaction)

    save_data(data)

    print("\n" + RED + "=" * 60 + RESET)

    print(
        RED + BOLD +
        "        MONEY DEBITED SUCCESSFULLY" +
        RESET
    )

    print(RED + "=" * 60 + RESET)

    print(
        "Amount  :",
        RED + money(amount) + RESET
    )

    print(
        "To      :",
        CYAN + purpose + RESET
    )

    print(
        "Balance :",
        GREEN + money(data["balance"]) + RESET
    )

    print(
        "Date    :",
        BLUE + get_time() + RESET
    )

    input(
        "\nPress ENTER to continue..."
    )


# ==============================
# CHECK BALANCE
# ==============================

def check_balance(data):

    clear_screen()

    print(CYAN + "=" * 60 + RESET)

    print(
        MAGENTA + BOLD +
        "                 VARDAAN ATM" +
        RESET
    )

    print(
        YELLOW +
        "              Welcome Shakuntla" +
        RESET
    )

    print(CYAN + "=" * 60 + RESET)

    print("\nCURRENT BALANCE")

    print(
        GREEN + BOLD +
        "       " +
        money(data["balance"]) +
        RESET
    )

    print(
        "\nChecked:",
        get_time()
    )

    input(
        "\nPress ENTER to continue..."
    )


# ==============================
# TRANSACTION HISTORY
# ==============================

def transaction_history(data):

    clear_screen()

    print(CYAN + "=" * 65 + RESET)

    print(
        MAGENTA + BOLD +
        "                 VARDAAN ATM" +
        RESET
    )

    print(
        YELLOW +
        "            TRANSACTION HISTORY" +
        RESET
    )

    print(CYAN + "=" * 65 + RESET)

    if len(data["transactions"]) == 0:

        print(
            YELLOW +
            "\nNo transactions yet." +
            RESET
        )

    else:

        number = 1

        for transaction in data["transactions"]:

            print(
                "\n" +
                BLUE +
                "Transaction " +
                str(number) +
                RESET
            )

            print("-" * 50)

            if transaction["type"] == "CREDIT":

                print(
                    GREEN +
                    "Type   : CREDIT (+)" +
                    RESET
                )

                print(
                    "Amount :",
                    GREEN +
                    money(transaction["amount"]) +
                    RESET
                )

                print(
                    "From   :",
                    CYAN +
                    transaction["person"] +
                    RESET
                )

            else:

                print(
                    RED +
                    "Type   : DEBIT (-)" +
                    RESET
                )

                print(
                    "Amount :",
                    RED +
                    money(transaction["amount"]) +
                    RESET
                )

                print(
                    "To     :",
                    CYAN +
                    transaction["person"] +
                    RESET
                )

            print(
                "Date   :",
                BLUE +
                transaction["date"] +
                RESET
            )

            number += 1

    print(
        "\n" +
        CYAN +
        "=" * 65 +
        RESET
    )

    print(
        "Current Balance:",
        GREEN + BOLD +
        money(data["balance"]) +
        RESET
    )

    print(
        CYAN +
        "=" * 65 +
        RESET
    )

    input(
        "\nPress ENTER to continue..."
    )


# ==============================
# EXIT
# ==============================

def exit_program(data):

    clear_screen()

    print(
        CYAN +
        "=" * 60 +
        RESET
    )

    print(
        MAGENTA + BOLD +
        "                 VARDAAN ATM" +
        RESET
    )

    print(
        CYAN +
        "=" * 60 +
        RESET
    )

    print(
        GREEN + BOLD +
        "\n          Thank You, Shakuntla Ji!" +
        RESET
    )

    print(
        YELLOW +
        "\n          Final Balance:" +
        RESET
    )

    print(
        GREEN + BOLD +
        "              " +
        money(data["balance"]) +
        RESET
    )

    print(
        "\n" +
        CYAN +
        "=" * 60 +
        RESET
    )

    input(
        YELLOW +
        "\n          Press ENTER to exit..." +
        RESET
    )


# ==============================
# MAIN PROGRAM
# ==============================

def main():

    data = load_data()

    while True:

        clear_screen()

        print(
            CYAN +
            "=" * 60 +
            RESET
        )

        print(
            MAGENTA + BOLD +
            "                  VARDAAN ATM" +
            RESET
        )

        print(
            YELLOW +
            "               Welcome Shakuntla" +
            RESET
        )

        print(
            CYAN +
            "=" * 60 +
            RESET
        )

        print(
            "\nCurrent Balance:",
            GREEN + BOLD +
            money(data["balance"]) +
            RESET
        )

        print(
            "\n" +
            CYAN +
            "-" * 60 +
            RESET
        )

        print(
            GREEN +
            "1. Credit Money" +
            RESET
        )

        print(
            RED +
            "2. Debit Money" +
            RESET
        )

        print(
            YELLOW +
            "3. Check Balance" +
            RESET
        )

        print(
            BLUE +
            "4. Transaction History" +
            RESET
        )

        print(
            MAGENTA +
            "5. Exit" +
            RESET
        )

        print(
            CYAN +
            "-" * 60 +
            RESET
        )

        choice = input(
            "\nEnter your choice: "
        )

        if choice == "1":

            credit_money(data)

        elif choice == "2":

            debit_money(data)

        elif choice == "3":

            check_balance(data)

        elif choice == "4":

            transaction_history(data)

        elif choice == "5":

            exit_program(data)
            break

        else:

            print(
                RED +
                "\nInvalid choice. Please enter 1 to 5." +
                RESET
            )

            input(
                "\nPress ENTER to try again..."
            )


# ==============================
# START PROGRAM
# ==============================

if __name__ == "__main__":
    main()
```

