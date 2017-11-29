<template>
     <div class="container" id="app">
          <section class="panel">
               <input type="checkbox" id="mark_all" v-bind:checked="areAllSelected" v-on:click="selectAll">
               <input type="text" placeholder="What Do You Need To Do?" autofocus class="text-input" v-model="newTask" v-on:keyup.enter="addTask">
               <button  v-on:click="addTask">Submit</button>
               <button v-on:click="clearList">Clear List</button>
          </section>
          <ul class="list">
               <li v-for="task in taskList" mv-bind:class="{done: task.checked}">
                    <input type="checkbox" class="checkbox" v-model="task.checked">
                    <input type="text" v-show="task.edit" v-model="task.edit">
                    <label for="checkbox" v-on:click="editTask(task)">{{ task.title }}</label>
                    <button class="delete" v-on:click="removeTask(task)">X</button>
               </li>
          </ul>
     </div>
</template>

<script>
     import axios from 'axios';
     export default {
          data() {
               return{
               newTask: "",
               taskList: [],
               }
          },
          computed: {
               areAllSelected: function(){
                    return this.taskList.every(function(task){
                         return task.checked;
                    }) && this.taskList.length > 0;
               },
          },

          methods: {
               //adding the tasks to the list
               addTask: function(){
                    var task = this.newTask.trim();
                    if(task) {
                         this.taskList.push({
                              title: task,
                              checked: false,
                              edit: false,
                         });
                         this.newTask = "";
                    }
               },

               removeTask: function(task) {
                    var index = this.taskList.indexOf(task); 
                    this.taskList.splice(index, 1);
               },

               clearList: function() {
                    this.taskList = [];
               },

               selectAll: function(task){
                    var targetValue = this.areAllSelected ? false : true;
                    for (var i = 0; i < this.taskList.length; i++) {
                         this.taskList[i].checked = targetValue;
                    }
               }
          },
          mounted() {
               axios.get('http://fjolbraut.org/api/tasks', {
                    params: {
                         api_token: 'CKbVlNg9qBzQQDfHNdceG001988xKoWGWEGsRFm6FtUOrhPts4W7nWQgIPiO'
                    }
               })
               .then(function(response) {
                    console.log(response.data);
               }).
               catch(function(error) {
                    console.log(error);
               });
          }
     }
</script>

<style lang="scss">
     .container {
          width: 70%;
          border: 1px solid #efefef;
     }

     button {
          color : #915196;
          background-color: #f8a3ff;
          border: 1px solid #c4a3c6;
          outline: 0;
          width: 100px;
          height: 38px;
          cursor: pointer;
          font-size: 15px;
     }

     .text-input {
          border: 1px solid #7c99ff;
          padding-left: 10px;
          width: 70%;
          height: 35px;
          color: #0037ff;
     }

     .list li{
          background-color: #7c99ff;
     }     

     .list li.done label {
          color: #d9d9d9;
          text-decoration: line-through;
     }
     .list li .delete {
          background-color: transparent;
          border: 1px solid #7c99ff;
          color: #0037ff;
          visibility: hidden;
          font-size: 20px;
          font-weight: bold;
     }
     .list li:hover > .delete {
          visibility: visible;
     }
     .list label {
          padding-right: 10px;
          display: inline-block;
          width: 70%;
          font-size: 18px;
          line-height: 24px;
          color: #192551;
          z-index: 2;
          overflow: hidden;
     }
</style>
