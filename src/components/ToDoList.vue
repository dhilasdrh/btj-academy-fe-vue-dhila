<template>
    <div class="flex min-h-screen p-5 justify-center items-center bg-gradient-to-br from-purple-300 to-blue-300">
        <!-- Main Container -->
        <div class="w-[90%] justify-center items-center p-6 bg-gray-200/70 shadow-2xl rounded-xl">
            <div class="flex items-center gap-3 mb-3">
                <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1" stroke="currentColor" data-slot="icon" class="w-8 h-8">
                <path stroke-linecap="round" stroke-linejoin="round" d="M9 12h3.75M9 15h3.75M9 18h3.75m3 .75H18a2.25 2.25 0 0 0 2.25-2.25V6.108c0-1.135-.845-2.098-1.976-2.192a48.424 48.424 0 0 0-1.123-.08m-5.801 0c-.065.21-.1.433-.1.664 0 .414.336.75.75.75h4.5a.75.75 0 0 0 .75-.75 2.25 2.25 0 0 0-.1-.664m-5.8 0A2.251 2.251 0 0 1 13.5 2.25H15c1.012 0 1.867.668 2.15 1.586m-5.8 0c-.376.023-.75.05-1.124.08C9.095 4.01 8.25 4.973 8.25 6.108V8.25m0 0H4.875c-.621 0-1.125.504-1.125 1.125v11.25c0 .621.504 1.125 1.125 1.125h9.75c.621 0 1.125-.504 1.125-1.125V9.375c0-.621-.504-1.125-1.125-1.125H8.25ZM6.75 12h.008v.008H6.75V12Zm0 3h.008v.008H6.75V15Zm0 3h.008v.008H6.75V18Z" />
                </svg>

                <h1 class="text-3xl font-bold">
                TO DO APP
                </h1>
            </div>

            <!-- SECTION SUMMARY -->
            <div class="container flex py-5 px-4 gap-4">
                <div class="basis-1/4 card py-3 text-center bg-green-500/50 shadow-md shadow-green-500/20 rounded">
                    <h4 class="text-slate-700">Pending</h4>
                    <h2 class="text-4xl font-semibold"> {{ totalPendingTask }} </h2>
                </div>

                <div class="basis-1/4 card py-3 text-center bg-blue-500/50 shadow-md shadow-blue-500/20 rounded">
                    <h4 class="text-slate-700">Low Priority</h4>
                    <h2 class="text-4xl font-semibold"> {{ totalLowPriorityTask }} </h2>
                </div>

                <div class="basis-1/4 card py-3 text-center bg-yellow-500/50 shadow-md shadow-yellow-500/20 rounded">
                    <h4 class="text-slate-700">Medium Priority</h4>
                    <h2 class="text-4xl font-semibold"> {{ totalMediumPriorityTask }} </h2>
                </div>

                <div class="basis-1/4 card py-3 text-center bg-red-500/50 shadow-md shadow-red-500/20 rounded">
                    <h4 class="text-slate-700">High Priority</h4>
                    <h2 class="text-4xl font-semibold"> {{ totalHighPriorityTask }} </h2>
                </div>
            </div>


            <!-- SECTION ADD NEW TASK -->
            <div class="py-5 px-8 bg-slate-50/80 mx-4 my-5">
                <h3 class="text-2xl text-gray-800 font-medium">Add New Task</h3>
                
                <!-- error message -->
                <div v-if="showErrorMessage" class="border border-red-700 bg-red-200 mt-2 p-2">
                    <p class="text-red-800"><strong>Error!</strong> Task name must not be empty.</p>
                </div>

                <!-- success message -->
                <div v-if="showSuccessMessage" class="border border-green-700 bg-green-200 mt-2 p-2">
                    <p class="text-green-800"><strong>Success!</strong> Task successfully added.</p>
                </div>

                <form @submit.prevent="addNewTask" class="flex my-4 space-x-5 justify-center">     
                    <div class="basis-2/5 w-full">
                        <label for="taskName" class="block mb-2 font-medium text-gray-800 dark:text-white">Name</label>
                        <input v-model="newTask.name" type="text" id="taskName" placeholder="Enter new task name here" class=" w-full bg-gray-50 form-input border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500" required>
                    </div>

                    <div class="basis-2/5 w-full">
                    <label for="taskPriority" class="block mb-2 font-medium text-gray-800 dark:text-white">Priority</label>
                    <select v-model="newTask.priority" id="taskPriority" class="w-full bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500">
                        <!-- <option selected>Choose priority</option> -->
                        <option selected value="Low">Low</option>
                        <option value="Medium">Medium</option>
                        <option value="High">High</option>
                    </select>
                    </div>

                    <div class="flex basis-1/5 items-end">
                    <button @click="addNewTask" class="w-full middle none center rounded-sm bg-blue-500 py-2 px-10 text-white shadow-md shadow-indigo-500/20 transition-all hover:shadow-lg hover:shadow-indigo-500/40 focus:opacity-[0.85] focus:shadow-none active:opacity-[0.85] active:shadow-none disabled:pointer-events-none disabled:opacity-50 disabled:shadow-none">
                        Save
                    </button>
                    </div>
                </form>
            </div>


            <!-- SECTION LIST TODO & DONE TASKS -->
            <div class="grid md:grid-cols-2">
                <!-- To Do -->
                <div class="py-5 px-4 bg-slate-50/80 mx-4 my-5 rounded-lg">
                    <h3 class="text-2xl text-gray-800 font-medium">To Do</h3>
                    <div v-if="todoTasks.length === 0">
                        <p class="py-5">No task to do at the moment. </p>
                    </div>
                    <!-- Task Item Card -->
                    <div v-else>
                        <div v-for="(task, index) in todoTasks" :key="index" class="flex justify-between bg-gray-200 bg-opacity-60 p-4 mt-3">
                            <div> 
                                <p class="text-lg font-semibold mb-2">{{ task.name }}</p>
                                <div :class="[getPriorityColorClass(task.priority), 'flex items-center rounded-2xl py-1 px-3 bg-opacity-40']">
                                    <div :class="[getPriorityColorClass(task.priority), 'w-3 h-3 rounded-full mr-2']"></div>
                                    <span class="text-xs font-bold text-gray-700 uppercase">{{ task.priority }}</span>
                                </div>
                            </div>
                            <div class="flex items-center gap-3">
                                <button @click="deleteTask(task.idTask)" class="text-sm bg-red-500/80 rounded-sm px-3 py-2 text-white shadow-md shadow-red-500/20 transition-all hover:shadow-lg hover:shadow-red-500/40 ">Delete</button>
                                <button @click="markAsDone(task.idTask)" class="text-sm bg-green-500/80 rounded-sm px-3 py-2 text-white shadow-md shadow-green-500/20 transition-all hover:shadow-lg hover:shadow-green-500/40">Done</button>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Done -->
                <div class="py-5 px-4 bg-slate-50/80 mx-4 my-5 rounded-lg">
                    <h3 class="text-2xl text-gray-800 font-medium">Done</h3>
                    <div v-if="completedTasks.length === 0">
                        <p class="py-5">No completed tasks yet. </p>
                    </div>
                    <div v-else>
                        <div v-for="(doneTask, doneIndex) in completedTasks" :key="doneIndex" class="flex justify-between bg-gray-200 bg-opacity-60 p-4 mt-3">
                            <div > 
                                <p class="text-lg font-semibold mb-2">{{ doneTask.name }}</p>
                                <div :class="[getPriorityColorClass(doneTask.priority), 'flex items-center rounded-2xl py-1 px-3 bg-opacity-40']">
                                    <div :class="[getPriorityColorClass(doneTask.priority), 'w-3 h-3 rounded-full mr-2']"></div>
                                    <span class="text-xs font-semibold text-gray-700 uppercase ">{{ doneTask.priority }}</span>
                                </div>
                            </div>
                            
                            <div class="flex items-center">
                                <button @click="deleteTask(doneTask.idTask)" class="text-sm bg-red-500/80 rounded-sm px-3 py-2 text-white shadow-md shadow-red-500/20 transition-all hover:shadow-lg hover:shadow-red-500/40 ">Delete</button>
                            </div>
                        </div>
                    </div>
                </div>

            </div>
        </div>
    </div>

</template>

<script>
export default {
  data() {

    return {
      counter: 0,
    //   tasks: [],
      tasks: this.getStoredTasks(),
      newTask: {
        name: "",
        priority: "Low",
      },
      showErrorMessage: false,
      showSuccessMessage: false,
    };
  },
  computed: {
    todoTasks() {
        return this.tasks.filter(task => !task.completed);
    },
    completedTasks() {
        return this.tasks.filter(task => task.completed);
    },
    totalPendingTask() {
      return this.tasks.filter((task) => !task.completed).length;
    },
    totalLowPriorityTask() {
      return this.tasks.filter(
        (task) => !task.completed && task.priority === "Low"
      ).length;
    },
    totalMediumPriorityTask() {
      return this.tasks.filter(
        (task) => !task.completed && task.priority === "Medium"
      ).length;
    },
    totalHighPriorityTask() {
      return this.tasks.filter(
        (task) => !task.completed && task.priority === "High"
      ).length;
    },
  },
  methods: {
    addNewTask(e) {
      e.preventDefault();
      if (this.newTask.name.trim() === "") {
        this.showErrorMessage = true;
      } else {
        this.tasks.push({
          idTask: this.counter,
          name: this.newTask.name,
          priority: this.newTask.priority,
          completed: false,
        });
        this.counter += 1;
        this.newTask.name = "";
        this.newTask.priority = "Low";
        this.showErrorMessage = false;
        this.showSuccessMessage = true;
        this.updateLocalStorage();
        setTimeout(() => {
          this.showSuccessMessage = false;
        }, 3000);
      }
    },
    getPriorityColorClass(priority) {
      switch (priority) {
        case "Low":
          return "bg-blue-500";
        case "Medium":
          return "bg-yellow-500";
        case "High":
          return "bg-red-500";
        default:
          return "bg-gray-500";
      }
    },
    deleteTask(idTask) {
        let index = this.tasks.findIndex(task => task.idTask == idTask);
        this.tasks.splice(index, 1);
        this.updateLocalStorage(); // Update local storage after removing the task
        alert('Task deleted successfully');
    },
    markAsDone(idTask) {
        let index = this.tasks.findIndex(task => task.idTask == idTask);
        const taskToMove = this.tasks[index];

        this.tasks.splice(index, 1); // remove the task from the "To Do" list
        taskToMove.completed = true; // mark the task as completed
        this.tasks.push(taskToMove); // add the task to the "Done" list
        this.updateLocalStorage(); // update local storage after moving the task
        alert('Task marked as done.');
    },
    updateLocalStorage() {
        localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },
    getStoredTasks() {
            const storedTasks = localStorage.getItem('tasks');
            return storedTasks ? JSON.parse(storedTasks) : [];
    },
  },
  mounted() {
    // fetch tasks data from local storage on component mount
    const storedTasks = localStorage.getItem('tasks');
    if (storedTasks) {
        this.tasks = JSON.parse(storedTasks);
    }
  },
};
</script>
