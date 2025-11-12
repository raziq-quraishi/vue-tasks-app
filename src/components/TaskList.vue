<script lang="ts" setup>
import type { Task } from '../types';
const props = defineProps<{
    tasks:Task[]
}>()
const emit = defineEmits<{
    toggleDone:[id:string],
    deleteTask:[id:string]
}>()
</script>
<template>
        <TransitionGroup name="list" tag="div" class="task-list">
            <article class="task" v-for="task in props.tasks" :key="task.id">
                <label>
                    <input type="checkbox" @input ="emit('toggleDone', task.id)" :checked="task.done">
                    <span :class="{done:task.done}">{{ task.title }}</span>
                </label>
    
                <button @click="emit('deleteTask',task.id)" class="outline round">Remove</button>
                    
            </article>

        </TransitionGroup>
   
</template>

<style scoped>
.task-list{
    margin-top: 1rem;
}
.done{
    text-decoration: line-through;
}
.task {
    display: flex;
    justify-content: space-between;
    align-items: center;
}
.list-enter-active,.list-leave-active{
    transition: all .5s ease;

}
.list-enter-from,.list-leave-to{
    opacity: 0;
    transform: translateX(300px);
}
</style>