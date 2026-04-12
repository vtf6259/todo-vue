<script setup lang="ts">
import { onMounted, ref, useTemplateRef } from 'vue';
  const tasks = ref(<string[]>["Hello this is a example task you will see this if you are visiting this for the first time or your tasks are invalid"])
  const userInput = useTemplateRef("userInput")
  
  onMounted(() => {
    userInput.value?.addEventListener("keypress", (e) => {
      if (e.key == "Enter") {
        addTaskOnClick()
      }
    })
    const tasksLocalStorage = localStorage.getItem("tasks")
    if (tasksLocalStorage != null) {
      console.log(tasksLocalStorage)
      try {
        if (!Array.isArray(JSON.parse(tasksLocalStorage))) {
          localStorage.removeItem("tasks")
          location.reload()
        }
        tasks.value = JSON.parse(tasksLocalStorage)
      } catch {
        localStorage.removeItem("tasks")
      }
      
      console.log(tasks)
    }
  })
  function removeTask(tasknum: number) {
    console.log(tasknum)
    console.log(tasks)
    tasks.value.splice(tasknum, 1)
    console.log(tasks)
    localStorage.setItem("tasks", JSON.stringify(tasks.value))
  }
  function addTaskOnClick() {
    if (userInput.value?.value == undefined || userInput.value?.value == "") return
    tasks.value.push(userInput.value?.value)
    if (userInput.value != null) userInput.value.value = "" 
    localStorage.setItem("tasks", JSON.stringify(tasks.value))
  }
</script>

<template>
  <div class="flexrow">
    <input class="flexrowMember" placeholder="New task" ref="userInput"/>
    <button v-on:click="addTaskOnClick()" class="flexrowMember">Create</button>
  </div>
  <ul>
    <li style="list-style-type: none;" v-for="(task, tasknum) in tasks">
      {{ task }} <button style="user-select: none; -webkit-user-select: none;" v-on:click="removeTask(tasknum)">X</button>
    </li>
  </ul>
</template>

<style scoped></style>
