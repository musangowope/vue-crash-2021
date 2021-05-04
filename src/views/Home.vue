<template>
  <div v-show="showAddTask">
    <AddTask @add-task="addTask"/>
  </div>
  <Tasks
      @delete-task="deleteTask"
      @toggle-reminder="toggleReminder"
      :tasks="tasks"
  />

</template>

<script>

import AddTask from "@/components/AddTask";
import Tasks from "@/components/Tasks";

export default {
  name: "Home",
  components: {
    AddTask,
    Tasks,
  },

  props: {
    showAddTask: Boolean,
  },

  data() {
    return {
      tasks: [],
    };
  },

  methods: {
    async deleteTask(id) {
      if (confirm("Are you sure?")) {
        const res = await fetch(`api/tasks/${id}`, {
          method: 'DELETE',
        });

        if(res.status === 200) {
          return this.tasks = this.tasks.filter(task => task.id !== id)
        }
        return alert('Error deleting task')
      }
    },

    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id);
      console.log(taskToToggle)
      const updTask = {...taskToToggle, reminder: !taskToToggle.reminder };

      const res = await fetch(`api/tasks/${id}`, {
        method: "PUT",
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updTask)
      });

      const data = await res.json();
      this.tasks = this.tasks.map(task => task.id === id ?
          { ...task, reminder: data.reminder } : task );
    },

    async addTask(newTask = {}) {
      const res =  await fetch('api/tasks', {
        method: "POST",
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify(newTask)
      })

      const data = await (res.json());
      this.tasks = [...this.tasks, data];
    },

    async fetchTasks() {
      const res = await fetch('api/tasks');
      const data = res.json();
      return data;
    },

    async fetchTask(id = "") {
      const res = await fetch(`api/tasks/${id}`)
      const data = res.json();
      return data;
    }
  },

  async created() {
    this.tasks = await this.fetchTasks()
  },
}
</script>

<style scoped>

</style>