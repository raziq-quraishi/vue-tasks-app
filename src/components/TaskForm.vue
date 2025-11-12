<script lang="ts" setup>
import { ref } from 'vue';
const newTasks= ref("")
const error = ref("")
const emit = defineEmits<{
    addTask:[newTask:string]
}>()
function submittedTask(){
    if(newTasks.value.trim()){
        emit("addTask", newTasks.value.trim())
        newTasks.value =''

    }else{
        error.value= "Input Can not be empty"
    }
}
</script>

<template>
      <form @submit.prevent="submittedTask">
            <label for="">
                New Task 
                <input 
                type="text" 
                v-model="newTasks"
                :aria-invalid="!!error || undefined"
                @input="error =''"
                >
                <small id="invalid-helper" v-if="error">{{ error }}</small>
            </label>
            <div class="button-container">
                <button>Add</button>
            </div>
        </form>
     
</template>