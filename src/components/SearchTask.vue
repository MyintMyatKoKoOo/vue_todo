<template lang="">
    <div class="todo-add-task-leftside">
         <input
              type="text"
              placeholder="Searching..."
                @input="(event)=>$emit('update:searchTask',event.target.value)"
              @keyup="()=> $emit('searching')"
            />
            <button class="searchbtn" @click="()=>$emit('searchandtask')">Task</button>
    </div>
</template>
<script>


export default {
    name:"AddTask",
}
</script>
<style lang="">
    
</style>