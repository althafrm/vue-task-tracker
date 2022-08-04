<script>
import Header from "./components/Header";
import Tasks from "./components/Tasks";
import AddTask from "./components/AddTask";

export default {
    name: "App",
    components: {
        Header,
        Tasks,
        AddTask,
    },
    methods: {
        toggleAddTask() {
            this.showAddTask = !this.showAddTask;
        },
        async addTask(task) {
            const response = await fetch("api/tasks", {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify(task),
            });

            const data = await response.json();

            this.tasks = [...this.tasks, data];
        },
        async deleteTask(taskId) {
            if (confirm("Are you sure?")) {
                const response = await fetch(`api/tasks/${taskId}`, {
                    method: "DELETE",
                });

                response.status === 200
                    ? (this.tasks = this.tasks.filter(
                          (task) => task.id !== taskId
                      ))
                    : alert("Error while deleting task!");
            }
        },
        async toggleTask(taskId) {
            const taskToToggle = await this.fetchTask(taskId);
            const updatedTask = {
                ...taskToToggle,
                reminder: !taskToToggle.reminder,
            };

            const response = await fetch(`api/tasks/${taskId}`, {
                method: "PUT",
                headers: {
                    "Content-Type": "application/json",
                },
                body: JSON.stringify(updatedTask),
            });

            const data = await response.json();

            this.tasks = this.tasks.map((task) =>
                task.id === taskId ? data : task
            );
        },
        async fetchTasks() {
            const response = await fetch("api/tasks");
            const tasks = await response.json();

            return tasks;
        },
        async fetchTask(taskId) {
            const response = await fetch(`api/tasks/${taskId}`);
            const tasks = await response.json();

            return tasks;
        },
    },
    data() {
        return {
            showAddTask: false,
            tasks: [],
        };
    },
    async created() {
        this.tasks = await this.fetchTasks();
    },
};
</script>

<template>
    <div class="container">
        <!-- <Header /> -->
        <Header
            title="Task Tracker"
            :showAddTask="showAddTask"
            @toggle-add-task="toggleAddTask"
        />
        <AddTask v-show="showAddTask" @add-task="addTask" />
        <Tasks
            :tasks="tasks"
            @delete-task="deleteTask"
            @toggle-task="toggleTask"
        />
    </div>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:wght@300;400&display=swap");
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}
body {
    font-family: "Poppins", sans-serif;
}
.container {
    max-width: 500px;
    margin: 30px auto;
    overflow: auto;
    min-height: 300px;
    border: 1px solid steelblue;
    padding: 30px;
    border-radius: 5px;
}
.btn {
    display: inline-block;
    background: #000;
    color: #fff;
    border: none;
    padding: 10px 20px;
    margin: 5px;
    border-radius: 5px;
    cursor: pointer;
    text-decoration: none;
    font-size: 15px;
    font-family: inherit;
}
.btn:focus {
    outline: none;
}
.btn:active {
    transform: scale(0.98);
}
.btn-block {
    display: block;
    width: 100%;
}
</style>
