# Task 1 JS Framework (Vue)

Make simple ToDo App with requirements below, using **Vue.js** framework for the frontend logic and **Tailwind CSS** for styling.

### Summary Section

Shows an overview of the Todo App with the following details:

 - Pending: Total number of tasks to do.
 - Low Priority: Total Todo with Low priority.
 - Medium Priority: Total Todo with Medium priority.
 - High Priority: Total Todo with High priority.

### Form Section (Add Task)

 - Name: input field in the form of text for the name of new task. this input must be filled in.
 - Priority: select to choose a priority type (Low, Medium, High) 
 - Save: button to save todo data 
 - Forms can be added to the todo list only if the input name has a value

### List Section (To do & Done)

Each item in the List has a Delete and Done button

 - **Delete** will remove the item from the Todo list.
 - **Done** will remove items from the Todo list and then add them to the Done list.

There is a priority label at the bottom of the Todo name, where each priority has a box with the following color:

 - **Blue** for Low Priority. 
 - **Yellow** for Medium Priority.
 - **Red** for High Priority.

## Code Overview
### **Template**

-   **Summary Section:**
    -   Displays statistics for pending and priority-based tasks.
-   **Add New Task Section:**
    -   Allows users to input a new task's name and priority, with error/success messages and a form for adding tasks.
-   **List of To-Do and Done Tasks:**
    -   Separates tasks into "To Do" and "Done" sections, displaying task cards with options to delete or mark tasks as done.
    -   Utilizes `v-for` to dynamically iterate through tasks and display them in the respective sections.
    -   Uses `:class` to dynamically apply priority-based background colors for visual distinction.
    
### **Bindings**

-   Utilizes `v-model` for two-way data binding to input fields.
-   Uses `v-if` and `v-else` to conditionally display error/success messages and task lists.

### **Data**

-   `counter`: Keeps track of the tasks' IDs for unique identification.
-   `tasks`: Stores the array of tasks. It initializes by fetching stored tasks from local storage or an empty array.
-   `newTask`: Represents the task being added with name and priority attributes.
-   `showErrorMessage` and `showSuccessMessage`: Control display of error and success messages for task addition.
 
### **Computed Properties**

   -   `todoTasks`: Filters tasks that are not completed.
   -   `completedTasks`: Filters completed tasks.
   -   `totalPendingTask`, `totalLowPriorityTask`, `totalMediumPriorityTask`, `totalHighPriorityTask`: Compute counts based on task status and priority.
    
### **Methods**

-   `addNewTask()`: Adds a new task to the `tasks` array, validates input, shows messages, and updates local storage.
-   `getPriorityColorClass()`: Returns the CSS class based on task priority for styling.
-   `deleteTask()`: Deletes a task based on its ID and updates local storage.
-   `markAsDone()`: Moves a task to the "Done" section by updating its completion status and updates local storage.
-   `updateLocalStorage()`: Updates the local storage with the current tasks.
-   `getStoredTasks()`: Retrieves tasks from local storage or returns an empty array if no tasks are stored.
