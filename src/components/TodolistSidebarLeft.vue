<template>

  <q-toolbar class="col-8 bg-white-3">
    <q-toolbar-title>History</q-toolbar-title>
  </q-toolbar>
  <q-separator/>

  <div class="history-search-div">
    <q-input dense filled class="history-search" type="text" v-model="searchHistory" placeholder="Search History">
      <template v-slot:append>
        <q-icon name="search"/>
      </template>
    </q-input>
  </div>
  <q-list>
    <q-item v-for="(task, index) in filteredHistory" :key="task">
      <q-item-section>
        <q-item-label>
          {{ task.title }}
        </q-item-label>
      </q-item-section>

      <q-item-section side>
        <q-btn flat rounded no-caps dense class="clear-btn" icon-right="clear" @click="clearFromHistory(index)">
          <q-tooltip>
            Remove from history
          </q-tooltip>
        </q-btn>
      </q-item-section>

      <q-item-section side>
        <q-btn flat rounded no-caps dense icon-right="restore" @click="dataRecovery(task, index)">
          <q-tooltip>
            restore
          </q-tooltip>
        </q-btn>
      </q-item-section>

    </q-item>
  </q-list>
</template>

<script setup>
import {computed, inject, ref} from 'vue';

const historyStore = inject('tag');
const searchHistory = ref('')


const filteredHistory = computed(() => {
  return historyStore.historyTasks?.filter((task) => {
    return task.title.toLowerCase().includes(searchHistory.value.toLowerCase());
  })
})

const clearFromHistory = (index) => {
  historyStore.historyTasks = historyStore.historyTasks.filter((task, taskIndex) => {
    if (index != taskIndex)
      return task;
  });
}

const dataRecovery = (task, index) => {
  historyStore.tasks.push(task);
  historyStore.historyTasks = historyStore.historyTasks.filter((task, taskIndex) => {
    if (index != taskIndex)
      return task;
  });
}


</script>

<style>


.history-search {
  margin-top: 20px;
}

</style>
