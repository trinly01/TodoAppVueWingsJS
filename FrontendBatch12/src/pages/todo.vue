<template>
  <div>
    <q-toolbar color="primary">
      <q-btn @click="$router.push('/')" flat round dense icon="playlist_add_check" />
      <q-toolbar-title>
        Todo App
      </q-toolbar-title>
    </q-toolbar>
    <div class="row layout-padding">
      <div class="column">
        <q-input v-model="task" />
      </div>
      <div class="column">
        <q-btn :disabled="!task.length" @click="addTask" round dense icon="save" />
      </div>
    </div>
    <q-list highlight>
      <q-item v-for="(t, i) in tasks" :key="i">
        <q-item-side>
          <q-checkbox :value="t.done" @input="updateTask(t)" />
        </q-item-side>
        <q-item-main :label="t.desc" />
        <q-item-side right>
          <q-btn @click="removeTask(t._id)" size="xs" round icon="delete" color="negative" />
        </q-item-side>
      </q-item>
    </q-list>
  </div>
</template>

<script>
export default {
  mounted () {
    this.$dbCon.services.todos.onDataChange(data => {
      this.tasks = data
    })
  },
  data () {
    return {
      task: '',
      tasks: []
    }
  },
  methods: {
    addTask () {
      let taskObj = {done: false, desc: this.task}
      // this.tasks.push(taskObj)
      this.$dbCon.services.todos.create(taskObj)
      this.task = ''
    },
    removeTask (index) {
      // this.tasks.splice(index, 1)
      this.$dbCon.services.todos.remove(index)
    },
    updateTask (t) {
      this.$dbCon.services.todos.patch(t._id, {done: !t.done})
    }
  }
}
</script>
