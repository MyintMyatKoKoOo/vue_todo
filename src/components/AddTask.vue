<template lang="">
    <div class="todo-add-task-leftside">
        <input type="text" :value="task" @input="(event)=>$emit('update:task',event.target.value)" @keyup.enter="()=>$emit('addtask')" />
        <button class="searchbtn" @click="()=>$emit('searchandtask')">Search</button>
    </div>
</template>
<script>
export default {
    name:"AddTask",
    props:['task'],
    data(){
        return {
            
        }
    }
}
</script>
<style lang="">
    
</style>