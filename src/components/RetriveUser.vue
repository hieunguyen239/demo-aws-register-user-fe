<template>
  <div class="tw-p-5">
    <button
    :class="{
      'tw-opacity-50 tw-cursor-none': isLoading
    }"
    type="button" class="tw-p-2 tw-border" @click="getUsers">
      Retrive User Lists
    </button>
    <table v-if="users.length" class="tw-border-collapse tw-border border-slate-500 tw-mt-5">
      <thead>
        <th class="tw-border tw-border-slate-600">Email</th>
        <th class="tw-border tw-border-slate-600">Role</th>
        <th class="tw-border tw-border-slate-600">User Data</th>
      </thead>
      <tbody>
        <tr v-for="user in users" :key="user.email">
          <td class="tw-border tw-border-slate-600 tw-p-2">{{ user.email }}</td>
          <td class="tw-border tw-border-slate-600 tw-p-2">{{ user.role }}</td>
          <td class="tw-border tw-border-slate-600 tw-p-2">{{ JSON.parse(user.userData) }}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const users = ref([]);
const isLoading = ref(false);

async function getUsers() {
  isLoading.value = true;
  try {
    const response = await fetch(`${import.meta.env.VITE_APP_API_URL}/get-users`, {
      method: 'GET',
    });

    const json = await response.json();
    users.value = json.data;
  } catch (error) {
    console.error(error);
  } finally {
    isLoading.value = false;
  }
}
</script>

<style lang="scss" scoped>

</style>