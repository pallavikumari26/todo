# todoimport os
import json

TASKS_FILE = 'tasks.json'

def load_tasks():
    if os.path.exists(TASKS_FILE):
        with open(TASKS_FILE, 'r') as file:
            return json.load(file)
    return []

def save_tasks(tasks):
    with open(TASKS_FILE, 'w') as file:
        json.dump(tasks, file, indent=4)

def show_menu():
    print("\n--- TO-DO LIST MENU ---")
    print("1. View Tasks")
    print("2. Add Task")
    print("3. Mark Task as Completed")
    print("4. Delete Task")
    print("5. Exit")

def view_tasks(tasks):
    if not tasks:
        print("\nNo tasks found.")
    else:
        print("\n--- Your Tasks ---")
        for index, task in enumerate(tasks, start=1):
            status = "✅" if task['completed'] else "❌"
            print(f"{index}. {task['title']} [{status}]")

def add_task(tasks):
    title = input("Enter task title: ").strip()
    if title:
        tasks.append({'title': title, 'completed': False})
        print("Task added.")
    else:
        print("Task title cannot be empty.")

def mark_task_completed(tasks):
    view_tasks(tasks)
    try:
        index = int(input("Enter task number to mark as completed ")) - 1
        if 0 <= index < len(tasks):
            tasks[index]['completed'] = True
            print("Task marked as completed")
        else:
            print("Invalid task number")
    except ValueError:
        print("Please enter a valid number")

def delete_task(tasks):
    view_tasks(tasks)
    try:
        index = int(input("Enter task number to delete ")) - 1
        if 0 <= index < len(tasks):
            removed = tasks.pop(index)
            print(f"Deleted task: {removed['title']}")
        else:
            print("Invalid task number")
    except ValueError:
        print("Please enter a valid number")

def main():
    tasks = load_tasks()

    while True:
        show_menu()
        choice = input("Enter your choice (1-5) ")

        if choice == '1':
            view_tasks(tasks)
        elif choice == '2':
            add_task(tasks)
        elif choice == '3':
            mark_task_completed(tasks)
        elif choice == '4':
            delete_task(tasks)
        elif choice == '5':
            save_tasks(tasks)
            print(" Tasks saved")
            break
        else:
            print("Invalid choice")

if __name__ == "__main__":
    main()
