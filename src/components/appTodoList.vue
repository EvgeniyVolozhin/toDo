<template>
        <ul class="todo-list">
            <AppTodoItem v-for="todo in todos" 
            :key="todo.id" 
            :todo="todo"
            @toggleTodo="toggleTodo"
            @removeTodo="removeTodo"
            />
        </ul>

     
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';
import AppTodoItem from './appTodoItem.vue';
import { Todo } from '@/types/todo';


export default defineComponent({
    components: {
        AppTodoItem,
    },
    props: {
        todos: {
            type:Array as PropType<Todo[]>
        }
    },
    methods: {
        toggleTodo(id:number) {
            this.$emit('toggleTodo',id)
        },
        removeTodo(id:number) {
            this.$emit('removeTodo',id)
    }
},
emits:{
            toggleTodo: (id:number) => Number.isInteger(id),
            removeTodo: (id:number) => Number.isInteger(id),
}
})
</script>