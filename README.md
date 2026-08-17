# StudyMate - Student Study Assistant

import json
from datetime import datetime

FILE = "studymate_data.json"


def load_data():
    try:
        with open(FILE, "r") as file:
            return json.load(file)
    except FileNotFoundError:
        return {"tasks": []}


def save_data(data):
    with open(FILE, "w") as file:
        json.dump(data, file, indent=4)


def add_task(data):
    subject = input("Enter subject: ")
    task = input("Enter study task: ")

    data["tasks"].append({
        "subject": subject,
        "task": task,
        "completed": False,
        "date": str(datetime.now().date())
    })

    save_data(data)
    print("\n✅ Task added successfully!")


def view_tasks(data):
    if not data["tasks"]:
        print("\n📭 No tasks added yet.")
        return

    print("\n📚 YOUR STUDY TASKS")
    print("-" * 45)

    for i, task in enumerate(data["tasks"], 1):
        status = "✅ Done" if task["completed"] else "⏳ Pending"

        print(f"{i}. {task['subject']} - {task['task']}")
        print(f"   {status} | Added: {task['date']}")


def complete_task(data):
    view_tasks(data)

    if not data["tasks"]:
        return

    try:
        number = int(input("\nEnter task number to complete: "))

        if 1 <= number <= len(data["tasks"]):
            data["tasks"][number - 1]["completed"] = True
            save_data(data)
            print("\n🎉 Great job! Task completed!")
        else:
            print("\n❌ Invalid task number.")

    except ValueError:
        print("\n❌ Please enter a number.")


def show_progress(data):
    total = len(data["tasks"])

    if total == 0:
        print("\n📊 No tasks available.")
        return

    completed = sum(
        1 for task in data["tasks"]
        if task["completed"]
    )

    progress = (completed / total) * 100

    print("\n📊 YOUR PROGRESS")
    print("-" * 30)
    print(f"Total Tasks : {total}")
    print(f"Completed   : {completed}")
    print(f"Pending     : {total - completed}")
    print(f"Progress    : {progress:.1f}%")

    if progress == 100:
        print("🏆 Amazing! Everything is completed!")
    elif progress >= 75:
        print("🔥 Excellent progress!")
    elif progress >= 50:
        print("💪 You're halfway there!")
    else:
        print("🌱 Keep going. Small progress is still progress!")


def main():
    data = load_data()

    while True:
        print("\n" + "=" * 45)
        print("        📚 STUDYMATE")
        print("=" * 45)

        print("1. ➕ Add Study Task")
        print("2. 📋 View Tasks")
        print("3. ✅ Complete Task")
        print("4. 📊 View Progress")
        print("5. 🚪 Exit")

        choice = input("\nChoose an option: ")

        if choice == "1":
            add_task(data)

        elif choice == "2":
            view_tasks(data)

        elif choice == "3":
            complete_task(data)

        elif choice == "4":
            show_progress(data)

        elif choice == "5":
            print("\n👋 Keep learning. See you next time!")
            break

        else:
            print("\n❌ Invalid choice. Try again.")


if __name__ == "__main__":
    main()
  
