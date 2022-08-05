<script>
import Tasks from "../components/Tasks";
import AddTask from "../components/AddTask";

export default {
    name: "Home",
    props: {
        showAddTask: Boolean,
    },
    components: {
        Tasks,
        AddTask,
    },
    methods: {
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
            tasks: [],
        };
    },
    async created() {
        this.tasks = await this.fetchTasks();
    },
};
</script>

<template>
    <AddTask v-show="showAddTask" @add-task="addTask" />
    <Tasks :tasks="tasks" @delete-task="deleteTask" @toggle-task="toggleTask" />
</template>
