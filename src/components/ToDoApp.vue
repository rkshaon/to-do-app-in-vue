<template>
  <div class="container">
    <!-- title -->
    <h1 class="text-center">{{msg}}</h1>
    <h2 class="text-center mt-5">To Do App with Vue</h2>
    <!-- input -->
    <div class="d-flex mb-3">
      <input v-model="task" type="text" placeholder="Enter a to do task..." class="form-control">
      <button @click="addTask" class="btn btn-warning rounded-3">{{buttonText}}</button>
    </div>
    <!-- task list table -->
    <table class="table table-bordered table-striped">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col">Status</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(task, index) in tasks" :key="index">
          <td :class="{'finished': task.status === 'finished'}">{{task.name}}</td>
          <td style="width: 180px;">
            <span 
              class="pointer" 
              @click="updateTaskStatus(index)"
              :class="{'text-danger': task.status === 'to-do',
              'text-warning': task.status === 'work in progress',
              'text-success': task.status === 'finished'}"
            >
              {{firstLetterUpperCase(task.status)}}
            </span>
          </td>
          <td>
            <div class="text-center pointer" @click="editTask(index)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center pointer" @click="deleteTask(index)">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },

  data() {
    return {
      task: '',
      editTaskIndex: null,
      availableStatuses: ['to-do', 'work in progress', 'finished'],
      buttonText: 'Add',
      tasks: [
        {
          name: 'First task',
          status: 'to-do'
        }
      ]
    }
  },

  methods: {
    addTask() {
      if (this.task.length === 0) {
        alert('You have to write something to insert a task!')
      } else if(this.editTaskIndex === null) {
        this.tasks.push({
          name: this.task,
          status: 'to-do'
        })
      } else {
        this.tasks[this.editTaskIndex].name = this.task
        this.editTaskIndex = null
      }

      this.task = ''
      this.buttonText = 'Add'
    },

    deleteTask(index) {
      this.tasks.splice(index, 1)
      this.editTaskIndex = null
    },

    editTask(index) {
      this.task = this.tasks[index].name
      this.editTaskIndex = index
      this.buttonText = 'Update'
    },

    updateTaskStatus(index) {
      let newStatusIndex = this.availableStatuses.indexOf(this.tasks[index].status)
      
      if (newStatusIndex === 2) {
        alert('This task is already marked as finished!')
      } else {
        if (++newStatusIndex > 2) newStatusIndex = 0

        this.tasks[index].status = this.availableStatuses[newStatusIndex]
      }      
    },

    firstLetterUpperCase(fullText) {
      return fullText.charAt(0).toUpperCase() + fullText.slice(1)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.pointer {
  cursor: pointer;
}
.finished {
  text-decoration: line-through;
}
</style>
