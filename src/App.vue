<script setup lang="ts">
import TaskForm from './components/TaskForm.vue';
import TaskList from './components/TaskList.vue';
import FilterButton from './components/FilterButton.vue';
import type { Task, TaskFilter } from './types';
import { computed, ref } from 'vue';
const message = ref('Vue Tasks App')
const tasks = ref<Task[]>([])
const filter =ref<TaskFilter>("all")


const totalDone = computed(()=> tasks.value.reduce((total, task)=>task.done? total + 1: total,0))
function addTask(newTask:string){
    tasks.value.unshift({
        id:crypto.randomUUID(),
        title:newTask,
        done:false
    })
}

const filteredTask=computed(()=>{
    switch(filter.value){
        case 'all':
        return tasks.value;
        case 'todo':
            return tasks.value.filter(task=>!task.done);
        case 'done':
            return tasks.value.filter(task=>task.done)
    }
    return tasks.value;
})

function toggleDone(taskId:string){
    const task = tasks.value.find(task=>task.id ===taskId)
    if(task){
        task.done=!task.done
    }
}
function removeTask(id:string){
    const index = tasks.value.findIndex(task => task.id === id)
    if(index !== -1){
        tasks.value.splice(index,1)
    }
}
function setFilter(value:TaskFilter){
filter.value= value;
}

</script>

<template>
    <main>
        <h1>{{ message }}</h1>
        <task-form @add-task="addTask"/>
        <h3 v-if="!tasks.length">Add a task to get started.</h3>
        <h3 v-else>{{ totalDone }} / {{ tasks.length }} tasks completed.</h3>
        <div class="button-container" v-if="tasks.length">
            <FilterButton :currentFilter="filter" filter="all" @set-filter="setFilter"/>
            <FilterButton :currentFilter="filter" filter="todo" @set-filter="setFilter"/>
            <FilterButton :currentFilter="filter" filter="done" @set-filter="setFilter"/>
           
        </div>
        <task-list :tasks="filteredTask" @toggle-done="toggleDone" @delete-task="removeTask"/>
          
    </main>
</template>

<style >

main{
    max-width: 600px;
    margin:1rem auto;
}
.button-container{
    display: flex;
    justify-content: end;
    gap: .5rem;
}

</style>
