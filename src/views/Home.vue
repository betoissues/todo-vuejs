<template>
  <div class="colums">
    <div class="column is-6 is-offset-3">
      <form v-on:submit.prevent="addTask">
        <div class="field has-addons">
          <div class="control is-expanded">
            <input
              class="input"
              type="text"
              placeholder="Task name"
              v-model="name"
            >
          </div>
          <div class="control">
            <button class="button is-primary">+</button>
          </div>
        </div>
      </form>
    </div>
  </div>
  <div class="colums">
    <div class="column is-6 is-offset-3">
      <div
        class="media"
        v-for="task in tasks"
        v-bind:key="task.id"
      >
        <div class="media-content has-text-left">{{ task.name }}</div>
        <div class="media-right"><button
            class="delete"
            @click="deleteTask(task.id)"
          ></button></div>
      </div>
    </div>
  </div>

</template>

<script>
import axios from 'axios'
export default {
  name: 'Home',
  data() {
    return {
      tasks: [],
      name: ''
    }
  },
  mounted() {
    this.getTasks()
  },
  methods: {
    getTasks() {
      axios({
        method: 'get',
        url: process.env.VUE_APP_API+'tasks'
      }).then(response => this.tasks=response.data)
    },
    addTask() {
      if(this.name) {
        axios({
          method: 'post',
          url: process.env.VUE_APP_API+'tasks',
          data: {
            name: this.name
          }
        }).then((response) => {
          this.tasks.push(response.data)
          this.name=''
        }).catch(error => console.log(error))
      }
    },
    deleteTask(task_id) {
      axios({
        method: 'delete',
        url: process.env.VUE_APP_API+'tasks/'+task_id
      }).then((response) => {
        for(var i=0;i<this.tasks.length;i++) {
          if(this.tasks[i].id==response.data.removed) {
            this.tasks.splice(i, 1);
            break;
          }
        }
      }).catch(error => console.log(error))
    }
  }
}
</script>
