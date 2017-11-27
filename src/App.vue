<template>
  <div id="app" class="container">
    <section>
      <input type="checkbox" id="mark_all">
      <input type="text" autofocus class="text-input">
      <button>Submit</button>
    </section>
    <ul class="list">
      <li>
        <input type="checkbox" class="checkbox">
        <!--<input type="text">-->
        <label></label>
        <button class="delete">X</button>
      </li>
    </ul>
  </div>
</template>

<script>
import axios from 'axios';
     export default {
      data() {
        return{
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
      },
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

.container {
  width: 70%;
  margin: 1em auto 3em;
  border: 1px solid #efefef;
}

.panel,
li {
  /* Use flexbox */
  display: flex;
  /* Center everything inside .panel and li vertically */
  align-items: center;
  /* Distribute space evenly between the contents*/
  justify-content: space-between;
  list-style-type: none;
  padding: 10px;
  border-bottom: 1px solid #efefef;
  background-color: #E7E8EB;
}

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
  background-color: #3465A4;
}

.list li.done label {
  color: #d9d9d9;
  text-decoration: line-through;
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

.list li.done label {
  color: #d9d9d9;
  text-decoration: line-through;
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
