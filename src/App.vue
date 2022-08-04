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
        addTask(task) {
            this.tasks = [...this.tasks, task];
        },
        deleteTask(taskId) {
            if (confirm("Are you sure?"))
                this.tasks = this.tasks.filter((task) => task.id !== taskId);
        },
        toggleTask(taskId) {
            this.tasks = this.tasks.map((task) =>
                task.id === taskId
                    ? { ...task, reminder: !task.reminder }
                    : task
            );
        },
    },
    data() {
        return {
            showAddTask: false,
            tasks: [],
        };
    },
    created() {
        this.tasks = [
            {
                id: 1,
                text: "Task 1",
                day: "Monday",
                reminder: true,
            },
            {
                id: 2,
                text: "Task 2",
                day: "Tuesday",
                reminder: true,
            },
            {
                id: 3,
                text: "Task 3",
                day: "Wednesday",
                reminder: false,
            },
        ];
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
