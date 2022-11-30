<template>
  <q-toolbar>
    <q-toolbar-title>Tags</q-toolbar-title>
  </q-toolbar>
  <q-separator/>
  <div class="add-tag">
    <q-input square filled class="addTag-input" type="text" @keyup.enter="addTag" placeholder="Add tag" v-model="tag">
      <template v-slot:append>
        <q-icon name="add" @click="addTag">
          <q-tooltip>
            Add
          </q-tooltip>
        </q-icon>
        <q-icon name="delete" @click="removeAllTags">
          <q-tooltip>
            Clear all
          </q-tooltip>
        </q-icon>
        <q-icon name="colorize">
          <q-tooltip>
            Choose color
          </q-tooltip>
          <q-popup-proxy transition-show="scale" transition-hide="scale">
            <q-color v-model="tagColor"/>
          </q-popup-proxy>
        </q-icon>
      </template>
    </q-input>
  </div>
  <div>
    <q-list>
      <div class="row">
        <q-item v-for="(tag, index) in tagsStore.tags" :key="tag">
          <div class="col">
            <q-badge class="badge" outline align="middle" :style="{color:tag.color}">
              {{ tag.label }}
              <q-icon name="clear" @click="removeTag(index)"/>
            </q-badge>
          </div>
        </q-item>
      </div>
    </q-list>
  </div>
</template>


<script setup>
import {inject, ref} from 'vue';

const tag = ref('')
const tagColor = ref('#000000')
const counter = ref(0)
const tagsStore = inject('tag');


const removeTag = (index) => {
  tagsStore.tags = tagsStore.tags.filter((task, taskIndex) => {
    if (index != taskIndex)
      return task;
  });
}

const addTag = () => {

  if (tag.value !== '')
    tagsStore.tags.push({label: tag.value, id: counter.value, color: tagColor.value});
  tag.value = '';
  counter.value += 1;
}

const removeAllTags = () => {
  tagsStore.tags = tagsStore.tags.filter(tag => {
    if (tag != tag)
      return tag;
  });
}
</script>


<style>

.badge {
  margin: 2px;
}

.add-tag {
  margin-top: 20px;
}

</style>

