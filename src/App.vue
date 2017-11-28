<template>
     <div id="app" class="container">
          <section class="panels">
               <input type="radio" id="mark_all" v-bind:checked="areAllSelected" v-on:click="selectAll">
               <input type="text" placeholder="What Do You Need To Do?" autofocus class="text-input" v-model="newTask" v-on:keyup.enter="addTask">
               <button  v-on:click="addTask">Submit</button>
               <button v-on:click="clearList">Clear List</button>
          </section>
          <ul class="list">
               <li v-for="task in taskList" mv-bind:class="{done: task.checked}">
                    <input type="radio" class="checkbox" v-model="task.checked">
                    <input type="text" v-show="task.edit" v-model="task.edit">
                    <label for="checkbox" v-on:click="editTask(task)">{{ task.title }}</label>
                    <button class="delete is-large" v-on:click="removeTask(task)">X</button>
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
               //if all the tasks are selected procceed to write them off
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

               //removing the tasks from the list
               removeTask: function(task) {
                    var index = this.taskList.indexOf(task); 
                    this.taskList.splice(index, 1);
               },

               //deleting the tasks from the list
               clearList: function() {
                    this.taskList = [];
               },

               //selecting all the created tasks in the list
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
                    console.log(response);
               }).
               catch(function(error) {
                    console.log(error);
               });
          }
     }
</script>

<style lang="scss">
     ul,
     li {
          margin: 0;
          padding: 0;
          border: 0;
     }


     /* Global Styles */
     body {
          line-height: 1;
          font-family: "Lato", sans-serif;
          background-color: #EFF1F2;
     }

     #app  {
          width: 70%;
          margin: 1em auto 3em;
          border: 1px solid #efefef;
     }

     .panels,
     li {
          /* Use flexbox */
          display: flex;
          /* Center everything inside .panels and li vertically */
          align-items: center;
          /* Distribute space evenly between the contents*/
          justify-content: space-between;
          list-style-type: none;
          padding: 10px;
          border-bottom: 1px solid #efefef;
          background-color: #E7E8EB;
     }

     panels {}

     .text-input {
          border: 1px solid #dedede;
          padding-left: 10px;
          width: 70%;
          height: 35px;
          color: #555;
     }

     button {
          color: #555;
          background-color: #FFFFFF;
          border: 1px solid #bbb;
          outline: 0;
          width: 100px;
          height: 38px;
          cursor: pointer;
          font-size: 12px;
     }

     .list li {
          background-color: #CC0099;
     }

     .list li.done label {
          color: #d32828;
          text-decoration-line: line-through;
     }

     .list li .delete {
          background-color: transparent;
          border: 1px solid #3465A4;
          color: #ddd;
          /* Hide the delete button by default*/
          visibility: hidden;
          font-size: 20px;
          font-weight: bold;
     }

     .list li:hover > .delete {
          /* Show the delete button when hovering over each list item */
          visibility: visible;
     }

     .list label {
          padding-right: 10px;
          display: inline-block;
          width: 70%;
          font-size: 18px;
          line-height: 24px;
          color: #fcfcfc;
          z-index: 2;
          overflow: hidden;
     }

     @media screen and (max-width: 768px) {
          .container {
               width: 90%;
               max-width: 90%;
          }
          button {
               width: 80px;
          }
     }
</style>
