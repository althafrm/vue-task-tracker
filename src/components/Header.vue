<script>
import Button from "./Button";

export default {
    name: "Header",
    // props: ['title'],
    props: {
        title: String,
        showAddTask: Boolean,
    },
    // props: {
    //     title: {
    //         type: String,
    //         default: "Hello Vue",
    //     },
    // },
    components: {
        Button,
    },
    computed: {
        isHomepage() {
            if (this.$route.path === "/") {
                return true;
            }

            return false;
        },
    },
};
</script>

<template>
    <header>
        <h1>{{ title }}</h1>
        <Button
            v-show="isHomepage"
            :text="showAddTask ? 'Close' : 'Add Task'"
            :color="showAddTask ? 'red' : 'green'"
            @btn-click="$emit('toggle-add-task')"
        />
    </header>
</template>

<style scoped>
header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 20px;
}
</style>