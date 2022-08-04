<script>
import Task from "./Task";

export default {
    name: "Tasks",
    props: {
        tasks: Array,
    },
    components: {
        Task,
    },
    emits: ["delete-task", "toggle-task"],
    methods: {
        deleteTask(taskId) {
            this.$emit("delete-task", taskId);
        },
        toggleTask(taskId) {
            this.$emit("toggle-task", taskId);
        },
    },
};
</script>

<template>
    <div :key="task.id" v-for="task in tasks">
        <Task
            :task="task"
            @delete-task="deleteTask"
            @toggle-task="toggleTask"
        />
        <!-- <Task :task="task" @delete-task="$emit('delete-task')" /> -->
    </div>
</template>