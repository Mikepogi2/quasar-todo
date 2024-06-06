<template>
  <q-page class="bg-grey column">
    <q-list class="bg-white" separator bordered>
      <div class="column q-pa-sm bg-primary">
        <q-input square filled v-model="taskText" placeholder="Add task" dense bg-color="white" @keyup.enter="addTask">
          <template #append>
            <q-btn flat round color="primary" icon="add" @click="addTask"/>
          </template>
        </q-input>
      </div>
      <q-item v-ripple v-for="(task, index) in tasks" :key="task.title" @click="task.done = !task.done" clickable :class="{ 'isDone bg-grey-4': task.done }">
        <q-item-section avatar>
          <q-checkbox v-model="task.done" color="primary" class="no-pointer-events"/>
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section side v-if="task.done">
          <q-btn flat round color="primary" icon="delete" dense @click.stop="deleteTask(index)"/>
        </q-item-section>
      </q-item>
    </q-list>
    <div class="q-ma-md q-pa-md flex flex-center" v-if="!tasks.length">
      <div class="absolute-center containerCheck">
        <q-icon name="check" size="100px" color="primary"/>
        <div class="text-h5 text-center text-primary">No tasks</div>
      </div>
    </div>
  </q-page>
</template>

<script setup>
import { useQuasar } from 'quasar';
import { ref } from 'vue';

const $q = useQuasar()

// defineOptions({
//   name: 'IndexPage'
// });

const taskText = ref('')
const tasks = ref([
  // {
  //   title:'Learn HTML',
  //   done: false,
  // },
  // {
  //   title:'Learn Quasar',
  //   done: true,
  // },
  // {
  //   title:'Learn Vue 3',
  //   done: false,
  // },
])

const addTask = () => {
  tasks.value.push({ title: taskText.value, done: false })
  taskText.value = ''
}

const deleteTask = (index) => {
    $q.dialog({
      title: 'Confirm',
      message: 'Would you like to delete the current task?',
      cancel: true,
      persistent: true
    }).onOk(() => {
      tasks.value.splice(index, 1)
      $q.notify({
          type: 'positive',
          message: 'Task has been deleted'
        })
    })
}

</script>

<style lang="scss">
 .isDone{
  .q-item__label{
    text-decoration: line-through;
    color: #bbb;
  }
 }
.containerCheck{
  opacity: 0.5;
}
</style>
