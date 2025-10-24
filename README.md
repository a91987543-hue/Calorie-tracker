# Calorie-tracker
import json
import os
from datetime import datetime

DATA_FILE = "calorie_log.json"

def load_data():
    if os.path.exists(DATA_FILE):
        with open(DATA_FILE, "r") as f:
            return json.load(f)
    return {}

def save_data(data):
    with open(DATA_FILE, "w") as f:
        json.dump(data, f, indent=4)

def add_entry(data):
    today = datetime.today().strftime("%Y-%m-%d")
    food = input("Enter food name: ").strip()
    try:
        calories = int(input("Enter calories (kcal): "))
    except ValueError:
        print("Invalid calorie value. Please enter a number.")
        return

    if today not in data:
        data[today] = []

    data[today].append({"food": food, "calories": calories})
    save_data(data)
    print(f"Added: {food} ({calories} kcal)")

def view_daily_total(data):
    today = datetime.today().strftime("%Y-%m-%d")
    if today not in data or not data[today]:
        print("No entries for today.")
        return

    total = sum(item["calories"] for item in data[today])
    print(f"\n--- {today} ---")
    for item in data[today]:
        print(f"{item['food']}: {item['calories']} kcal")
    print(f"Total: {total} kcal")

def main():
    data = load_data()
    while True:
        print("\n--- Calorie Tracker ---")
        print("1. Add food")
        print("2. View today's total")
        print("3. Exit")
        choice = input("Choose an option: ").strip()

        if choice == "1":
            add_entry(data)
        elif choice == "2":
            view_daily_total(data)
        elif choice == "3":
            print("Goodbye!")
            break
        else:
            print("Invalid choice. Try again.")

if __name__ == "__main__":
    main()
