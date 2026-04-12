<!-- eslint-disable vue/require-v-for-key -->
<script setup lang="ts">
  import { ref, useTemplateRef } from 'vue'
  const userInput = useTemplateRef('userInput')
  const tasks = ref(<string[]>[])
  function addTask(task: string | undefined) {
    if (task != undefined) {
      tasks.value.push(task)
    }
  }
  function addTaskOnClick() {
    addTask(userInput.value?.value)
    if (userInput?.value != null) {
      userInput.value.value = ""
    }
    localStorage.setItem("tasks", JSON.stringify(tasks.value))
  }
  function removeTask(task: number) {
    tasks.value.splice(task,1)
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
