<template>
  <div class="column items-center">
    <q-list bordered separator class="list">
      <div class="add-task">
        <q-select class="select-tags"
                  :options="todoListStore.tags"
                  filled
                  v-model="todoTags"
                  multiple
                  use-chips
                  clearable
                  label="Choose tags"
        >
          <template v-slot:option="{ itemProps, opt, selected, toggleOption }">
            <q-item v-bind="itemProps">
              <q-item-section>
                <q-item-label v-html="opt.label"/>
              </q-item-section>
              <q-item-section side>
                <q-toggle :model-value="selected" @update:model-value="toggleOption(opt)"/>
              </q-item-section>
            </q-item>
          </template>
        </q-select>
        <q-input outlined class="task-input" type="text" @keyup.enter="addNewTodo" v-model="todo"
                 placeholder="To add task write here"/>
        <q-btn flat rounded no-caps dense class="add-btn" icon-right="add" @click="addNewTodo">
          <q-tooltip>
            Add
          </q-tooltip>
        </q-btn>
        <q-btn flat rounded no-caps dense class="add-btn" icon-right="clear" @click="clearTodoList">
          <q-tooltip>
            Clear all
          </q-tooltip>
        </q-btn>
        <q-btn flat rounded no-caps dense class="clear-btn" icon-right="delete" @click="clearDoneTasks">
          <q-tooltip>
            Clear done tasks
          </q-tooltip>
        </q-btn>
      </div>
      <div class="search">
        <q-input dense filled class="search-input" type="text" v-model="searchTask" placeholder="Search">
          <template v-slot:append>
            <q-icon name="search"/>
          </template>
        </q-input>
      </div>
      <q-item class="items" v-for="(task, index) in filteredList" :key="task">
        <q-item-section>
          <q-checkbox class="check" v-model="task.completed"/>
        </q-item-section>

        <q-item-section>
          <q-item-label class="task" v-if="!task.edit" :class="{complete : task.completed}">
            {{ task.title }}
          </q-item-label>
        </q-item-section>

        <q-item-label class="row" v-for="(tag, tagIndex) in task.tagList" :key="tag">
          <q-badge class="badge-task" outline align="middle" :style="{color:tag.color}">
            {{ tag.label }}
            <q-icon name="clear" @click="removeTag(tag.id, index, tagIndex)"/>
          </q-badge>
        </q-item-label>

        <q-item-section class="edit">
          <q-input v-if="task.edit" type="text" v-model="task.title"/>
        </q-item-section>

        <q-item-section side>
          <q-btn flat round>
            <q-icon name="delete" @click="remove(index)"/>
          </q-btn>
        </q-item-section>

        <q-item-section side>
          <q-btn flat round>
            <q-icon name="edit" @click="editTodo(task)"/>
          </q-btn>
        </q-item-section>

        <q-item-section side>
          <q-btn flat round>
            <q-icon name="save" @click="editDone(task)"/>
          </q-btn>
        </q-item-section>
        <q-separator :style="{margin: '10px'}"/>
      </q-item>
    </q-list>
  </div>
</template>


<script setup>
import {computed, inject, ref} from 'vue';

const todoTags = ref([])
const todo = ref('')
const searchTask = ref('')
const todoListStore = inject('tag')
let counter = ref(0)


const filteredList = computed(() => {
  return todoListStore.tasks?.filter((task) => {
    return task.title.toLowerCase().includes(searchTask.value.toLowerCase());
  })
})

const addNewTodo = () => {
  if (todo.value !== '')
    todoListStore.tasks.push(
      {
        title: todo.value,
        completed: false,
        edit: false,
        tagList: todoTags.value
      }
    );
  counter.value += 1;
  todo.value = '';
  todoTags.value = [];
}

const removeTag = (idTag, index,) => {
  todoListStore.tasks[index].tagList = todoListStore.tasks[index].tagList.filter(tag => {
    if (tag.id != idTag)
      return tag;
  })

}

const remove = (index) => {
  todoListStore.tasks = todoListStore.tasks.filter((task, taskIndex) => {
    if (index != taskIndex)
      return task;
    else {
      todoListStore.historyTasks.push(task);
    }
    console.log(todoListStore.historyTasks[0]);
  });
}

const clearTodoList = () => {
  todoListStore.tasks = todoListStore.tasks.filter(task => {
    if (task != task)
      return task;
  });
}

const editTodo = (task) => {
  task.edit = true;
}

const editDone = (task) => {
  task.edit = false;
}

const clearDoneTasks = () => {
  todoListStore.tasks = todoListStore.tasks.filter(t => {
    if (!t.completed)
      return t;
  });
}

</script>

<style>

.complete {
  text-decoration: line-through;
}

.task {
  position: absolute;
  left: 55px;
  font-size: large;
}

.task-input {
  width: 380px;
  margin: 7px;
}

.add-btn {
  margin: 7px;
}

.clear-btn {
  margin: 7px;
}

.edit {
  top: -10px;
  left: 50px;
  position: absolute;
}

.add-task {
  margin: 10px 10px 10px 60px;
  display: flex;
  flex-direction: row;
}

.list {
  margin-top: 50px;
  padding: 30px;
  width: 850px;
}

.search-input {
  width: 380px;
  margin-left: 140px;
}

.search {
  display: flex;
  flex-direction: row;
  margin-left: 50px;
}

.select-tags {
  margin-top: 8px;
  width: 170px;
}

.badge-task {
  margin: 10px;
}


</style>
