<template>
  <div class="dashboard">
    <HeaderView/>
    <main>
      <ToolbarView :toggleForm="toggleForm" />
      <TeamView :users="users" :toggleForm="toggleForm" :fetchUsers="fetchUsers" />
      <UserFormView v-if="formActive" :toggleForm="toggleForm" :fetchUsers="fetchUsers" :userId="userId" />
    </main>
  </div>
</template>

<script>
  import { ref } from 'vue';
  import APIController from '@/controllers/api.js';

  import HeaderView from '@/components/HeaderView.vue';
  import ToolbarView from '@/components/ToolbarView.vue';
  import TeamView from '@/components/TeamView.vue';
  import UserFormView from '@/components/UserFormView.vue';

  export default {
    name: 'App',
    components:{
      HeaderView,
      ToolbarView,
      TeamView,
      UserFormView,
    },
    setup(){
      const users = ref([]);
      var formActive = ref(false);
      var userId = ref(false);

      const fetchUsers = async () => {
        users.value = await APIController.FetchUsers();
      }

      const toggleForm = (id = false) => {
        formActive.value = !formActive.value;
        userId.value = false;

        if (id) {
          userId.value = id;
        }
      }

      return {
        users, fetchUsers, toggleForm, formActive, userId
      }
    },
    mounted() {
      this.fetchUsers();
    }
  }
</script>

<style>
  :root {
    --primary: #8A4CFC;
    --primary-alt: #702fe7;
    --light: #EEEEEE;
    --light-alt: #F8F8F8;
    --grey: #888888;
    --dark: #131A26;
  }

  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: "Fira sans", sans-serif;
  }

  body {
    background-color: var(--light);
  }

  main {
    width: 100vw;
    overflow: hidden;
  }

  input, button {
    appearance: none;
    outline: none;
    border: none;
    background: none;
  }

  .button {
    display: inline-block;
    padding: 8px 16px;
    background-color: var(--primary);
    color: #fff;
    font-size: 20px;
    font-weight: 700;
    text-transform: uppercase;
    cursor: pointer;
    border-radius: 6px;
    transition: cubic-bezier(0.39, 0.575, 0.565, 1);
  }

  .button:hover {
    background-color: var(--primary-alt);
  }

  .button.button-outline {
    background-color: transparent;
    border: 2px solid var(--primary);
    padding: 6px 14px;
    color: var(--primary);
  }

  .button.button-outline:hover {
    color: #fff;
    background-color: var(--primary);
  }

  .button.button-small {
    padding: 4px 8px;
    font-size: 18px;
    font-weight: 600;
  }

  .button.button-alert {
    background-color: crimson;
  }

  .button.button-alert:hover {
    background-color: rgb(189, 0, 38);
  }

  .button-group {
    display: -ms-flex;display: -webkit-flex;display: flex;
    margin: 0 -8px;
  }

  .button-group.group-end {
    -webkit-justify-content: flex-end;justify-content: flex-end;
  }

  .button-group .button {
    margin: 0 8px;
  }
</style>
