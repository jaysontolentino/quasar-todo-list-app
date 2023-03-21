<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        @keyup.enter="addTask"
        v-model="newTask"
        class="col"
        square
        filled
        bg-color="white"
        placeholder="Add Task"
        dense
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-ripple
        v-for="(task, index) in tasks"
        :key="task.title"
        clickable
        @click="task.done = !task.done"
        :class="{ 'done bg-blue-1': task.done }"
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            color="primary"
            class="no-pointer-events"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>

        <q-item-section v-if="task.done" side>
          <q-btn
            @click.stop="deleteTask(index)"
            flat
            round
            dense
            color="negative"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h5 text-primary text-center">No Tasks</div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent, ref } from "vue";
import { useQuasar } from "quasar";

export default defineComponent({
  name: "TodoPage",
  setup() {
    const $q = useQuasar();

    const newTask = ref("");

    const tasks = ref([]);

    const addTask = () => {
      tasks.value.push({
        title: newTask.value,
        done: false,
      });
      newTask.value = "";
    };

    const deleteTask = (index) => {
      $q.dialog({
        title: "Confirm",
        message: "Really want to delete?",
        cancel: true,
        persistent: true,
      }).onOk(() => {
        tasks.value.splice(index, 1);
        $q.notify({
          message: "Task deleted!",
          color: "green",
        });
      });
    };

    return {
      tasks,
      newTask,
      addTask,
      deleteTask,
    };
  },
});
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}

.no-tasks {
  opacity: 0.5;
}
</style>
