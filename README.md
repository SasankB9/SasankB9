- 👋 Hi, I’m @SasankB9
public class User {
    private Long id;
    private String username;
    private String email;
    // Other user-specific properties, constructors, getters, setters, etc.
}
public class TodoItem {
    private Long id;
    private String taskName;
    private boolean completed;
    private User user;
    // Other todo item properties, constructors, getters, setters, etc.
}
import org.springframework.data.repository.CrudRepository;

public interface UserRepository extends CrudRepository<User, Long> {
    // Custom queries can be added here if needed
}
import org.springframework.data.repository.CrudRepository;

public interface TodoItemRepository extends CrudRepository<TodoItem, Long> {
    // Custom queries can be added here if needed
}
import javax.persistence.*;

@Entity
public class User {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String username;
    private String email;
    // Other user-specific properties, constructors, getters, setters, etc.
}
import javax.persistence.*;

@Entity
public class TodoItem {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String taskName;
    private boolean completed;
    @ManyToOne
    private User user;
    // Other todo item properties, constructors, getters, setters, etc.
}
@RestController
@RequestMapping("/users")
public class UserController {
    // Inject the UserRepository
    private final UserRepository userRepository;

    public UserController(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    // Implement methods to handle GET, POST, PUT, and DELETE requests for users
}
@RestController
@RequestMapping("/todo-items")
public class TodoItemController {
    // Inject the TodoItemRepository
    private final TodoItemRepository todoItemRepository;

    public TodoItemController(TodoItemRepository todoItemRepository) {
        this.todoItemRepository = todoItemRepository;
    }

    // Implement methods to handle GET, POST, PUT, and DELETE requests for todo items
}
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Task Management Application</title>
    <!-- Include Bootstrap CSS -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
    <!-- Add your custom CSS (optional) -->
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <!-- Your UI elements will go here -->
    <div class="container">
        <!-- Task list -->
        <div id="task-list">
            <!-- Display tasks here -->
        </div>
        <!-- Form for adding a new task -->
        <form id="add-task-form">
            <div class="form-group">
                <input type="text" class="form-control" id="task-name" placeholder="Task name" required>
            </div>
            <button type="submit" class="btn btn-primary">Add Task</button>
        </form>
    </div>

    <!-- Include Bootstrap JS and your custom JS -->
    <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.9.1/dist/umd/popper.min.js"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>
    <script src="app.js"></script>
</body>
</html>
// Sample data (replace this with API calls)
const tasks = [
    { id: 1, name: "Task 1", completed: false },
    { id: 2, name: "Task 2", completed: true },
    // Add more sample tasks here
];

// Function to display tasks
function displayTasks() {
    const taskList = document.getElementById("task-list");
    taskList.innerHTML = "";

    tasks.forEach((task) => {
        const taskElement = document.createElement("div");
        taskElement.className = "task-item";

        // Customize the appearance based on task status
        if (task.completed) {
            taskElement.classList.add("completed");
        }

        taskElement.innerHTML = `
            <span>${task.name}</span>
            <button class="btn btn-danger btn-sm" onclick="deleteTask(${task.id})">Delete</button>
        `;

        taskList.appendChild(taskElement);
    });
}

// Function to add a new task
function addTask(event) {
    event.preventDefault();

    const taskNameInput = document.getElementById("task-name");
    const taskName = taskNameInput.value.trim();

    if (taskName !== "") {
        const newTask = {
            id: tasks.length + 1, // Replace this with the generated ID from the backend
            name: taskName,
            completed: false,
        };

        tasks.push(newTask);
        taskNameInput.value = "";
        displayTasks();
    }
}

// Function to delete a task
function deleteTask(taskId) {
    const index = tasks.findIndex((task) => task.id === taskId);
    if (index !== -1) {
        tasks.splice(index, 1);
        displayTasks();
    }
}

// Attach event listeners
document.getElementById("add-task-form").addEventListener("submit", addTask);

// Initial display of tasks
displayTasks();
