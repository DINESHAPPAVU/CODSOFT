import json

class ToDoList:
    def __init__(self, filename='tasks.json'):
        self.filename = filename
        self.tasks = self.load_tasks()

    def load_tasks(self):
        try:
            with open(self.filename, 'r') as file:
                return json.load(file)
        except FileNotFoundError:
            return []

    def save_tasks(self):
        with open(self.filename, 'w') as file:
            json.dump(self.tasks, file, indent=4)

    def add_task(self, title, description=''):
        self.tasks.append({'title': title, 'description': description, 'status': 'Pending'})
        self.save_tasks()

    def view_tasks(self):
        for i, task in enumerate(self.tasks, start=1):
            print(f"{i}. {task['title']} - {task['status']}")
            if task['description']:
                print(f"   Description: {task['description']}")

    def update_task(self, index, status):
        if 0 <= index < len(self.tasks):
            self.tasks[index]['status'] = status
            self.save_tasks()

    def delete_task(self, index):
        if 0 <= index < len(self.tasks):
            self.tasks.pop(index)
            self.save_tasks()

if __name__ == "__main__":
    app = ToDoList()
    while True:
        print("\nTo-Do List:")
        app.view_tasks()
        print("\nOptions:")
        print("1. Add Task")
        print("2. Update Task")
        print("3. Delete Task")
        print("4. Exit")

        choice = input("Enter your choice: ")
        if choice == '1':
            title = input("Task Title: ")
            description = input("Task Description: ")
            app.add_task(title, description)
        elif choice == '2':
            index = int(input("Task Number to Update: ")) - 1
            status = input("New Status (Pending/In Progress/Completed): ")
            app.update_task(index, status)
        elif choice == '3':
            index = int(input("Task Number to Delete: ")) - 1
            app.delete_task(index)
        elif choice == '4':
            break
        else:
            print("Invalid choice. Try again!")

