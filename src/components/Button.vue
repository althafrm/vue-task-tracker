<script>
export default {
    name: "Button",
    props: {
        text: String,
        color: String,
    },
    methods: {
        onClick() {
            console.log("click");
        },
    },
};
</script>

<template>
    <!-- <button @click="onClick()" class="btn" :style="{ background: color }">
        {{ text }}
    </button> -->
    <button
        class="btn"
        :style="{ background: color }"
        @click="$emit('btn-click')"
    >
        {{ text }}
    </button>
</template>