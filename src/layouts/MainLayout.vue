<template>
  <q-layout view="hHh lpR fFf">

    <q-header elevated class="bg-primary text-white">
      <q-toolbar>
        <q-btn dense flat round icon="menu" @click="toggleLeftDrawer"/>

        <q-toolbar-title>
          <q-avatar>
            <img src="https://cdn.quasar.dev/logo-v2/svg/logo-mono-white.svg">
          </q-avatar>
          Todo List
        </q-toolbar-title>

        <q-btn dense flat round icon="menu" @click="toggleRightDrawer"/>
      </q-toolbar>
    </q-header>

    <q-drawer show-if-above v-model="leftDrawerOpen" side="left" bordered>
      <todolist-sidebar-left/>
    </q-drawer>

    <q-drawer show-if-above v-model="rightDrawerOpen" side="right" bordered>
      <todo-list-sidebar/>
    </q-drawer>

    <q-page-container>
      <q-page>
        <router-view/>
      </q-page>
    </q-page-container>

  </q-layout>
</template>

<script setup>
import {provide, reactive, ref} from 'vue'

import TodoListSidebar from "components/TodoListSidebar.vue";
import TodolistSidebarLeft from "components/TodolistSidebarLeft.vue";

const rightDrawerOpen = ref(false)
const leftDrawerOpen = ref(false)

const todoListStore = reactive({
  tasks: [],
  tags: [],
  historyTasks: []
});

provide('tag', todoListStore);

const toggleRightDrawer = () => {
  rightDrawerOpen.value = !rightDrawerOpen.value
}

const toggleLeftDrawer = () => {
  leftDrawerOpen.value = !leftDrawerOpen.value
}


</script>
