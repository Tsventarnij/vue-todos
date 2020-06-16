<template>
    <div>
        <router-link to="/">Home</router-link>
        <hr>
        <AddTodo
                @add-todo="AddTodo"
        />
        <select v-model="filter">
            <option value="all">All</option>
            <option value="active">Active</option>
            <option value="completed">Completed</option>
        </select>
        <hr>
        <Loader v-if="loading" />
        <TodoList
                v-else-if="filteredTodos.length"
                v-bind:todos="filteredTodos"
                @remove-todo="RemoveTodo"
        />
        <p v-else>No todos!</p>
    </div>
</template>

<script>
    import TodoList from "@/components/TodoList";
    import AddTodo from "@/components/AddTodo";
    import Loader from "@/components/Loader";

    export default {
        name: 'Todos',
        data() {
            return {
                todos: [],
                loading: true,
                filter: 'all'
            }
        },
        components: {
            TodoList,
            AddTodo,
            Loader
        },
        methods: {
            RemoveTodo(id) {
                this.todos = this.todos.filter(t => t.id !== id);
            },
            AddTodo(newTodo) {
                this.todos.push(newTodo);
            }
        },
        mounted() {
            fetch('https://jsonplaceholder.typicode.com/todos?_limit=5')
                .then(response => response.json())
                .then(json => {
                    this.todos = json;
                    this.loading = false;
                })
        },
        // watch: {
        //     filter(value) {
        //         console.log(value);
        //     }
        // }
        computed: {
            filteredTodos() {
                switch(this.filter) {
                    case 'active':
                        return this.todos.filter(t => !t.completed);
                    case 'completed':
                        return this.todos.filter(t => t.completed);
                    default:
                        return this.todos;
                }
            }
        }
    }
</script>

<style scoped>

</style>