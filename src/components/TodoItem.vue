<template>
    <div>
        <li>
            <span v-bind:class="{done: todo.completed}">
                <input v-if="todo.completed" checked type="checkbox" v-on:change="todo.completed = !todo.completed">
                <input v-else type="checkbox" v-on:change="todo.completed = !todo.completed">
                <strong> {{index+1}}</strong>
                {{todo.title | uppercase}}
            </span>
            <button
                class="rm"
                v-on:click="$emit('remove-todo', todo.id)"
            >&times;</button>
        </li>
    </div>
</template>

<script>
    export default {
        name: "TodoItem",
        props: {
            todo: {
                type: Object,
                required: true
            },
            index: Number,
        },
        filters: {
            uppercase(value) {
                return value.toUpperCase();
            }
        }
    }
</script>

<style scoped>
    li {
        border: 1px solid #ccc;
        display: flex;
        justify-content: space-between;
        padding: .5rem 2rem;
        margin-bottom: 1rem;
    }
    .rm {
        background: red;
        color: #fff;
        border-radius: 50%;
        font-weight: bold;
    }
    input {
        margin-right: 1rem;
    }
    .done {
        text-decoration: line-through;
    }
</style>