<template>
     <ul class="list">
          <li v-for="task in taskList" mv-bind:class="{done: task.checked}">
               <input type="checkbox" class="checkbox" v-model="task.checked">
               <input type="text" v-show="task.edit" v-model="task.edit">
               <label for="checkbox" v-on:click="editTask(task)">{{ task.title }}</label>
               <button class="delete" v-on:click="removeTask(task)">delete</button>
          </li>
     </ul>
</template>