<template>
  <form action="" method="POST" class="tw-p-5 tw-bg-gray-100" @submit.prevent="saveUser" enctype="multipart/form-data">
    <div class="tw-flex tw-items-center">
      <label for="email" class="tw-mr-5 tw-min-w-[100px]">Email</label>
      <input type="text" id="email" name="email" class="tw-border" v-model="person.email">
    </div>
    <div class="tw-flex tw-items-center tw-mt-2">
      <label for="name" class="tw-mr-5 tw-min-w-[100px]">Name</label>
      <input type="text" id="name" name="name" class="tw-border" v-model="person.name">
    </div>
    <div class="tw-mt-2 tw-flex tw-items-center">
      <label for="dob" class="tw-mr-5 tw-min-w-[100px]">Dob</label>
      <input type="date" id="dob" name="dob" class="tw-border" v-model="person.dob">
    </div>
    <div class="tw-mt-2 tw-flex tw-items-center">
      <label for="user-role" class="tw-mr-5 tw-min-w-[100px]">User Role</label>
      <select name="role" id="user-role" v-model="person.role">
        <option value="user">User</option>
        <option value="admin">Admin</option>
      </select>
    </div>
    <div class="tw-mt-2 tw-flex tw-items-center">
      <label for="avatar" class="tw-mr-5 tw-min-w-[100px]">Picture</label>
      <input type="file" id="avatar" name="avatar" class="tw-border" @change="onFileChange">
    </div>

    <div class="tw-mt-2 tw-flex" v-if="message !== 'loading...'">
      <input type="submit" id="avatar" name="submit" class="tw-border tw-p-2 tw-bg-amber-400">
    </div>
    <div v-if="message" class="tw-mt-2 tw-bg-slate-500 tw-p-2">
      {{  message }}
    </div>
  </form>
</template>

<script setup>
import { reactive, ref } from 'vue';

const person = reactive({
  name: '',
  email: '',
  dob: '',
  role: 'user',
});

const avatar = ref([]);
const message = ref('');

function onFileChange(e) {
  avatar.value = e.target.files || e.dataTransfer.files;
}

async function saveUser() {
  const {
    name,
    email,
    dob,
  } = person;

  if (!name || !email || !dob || !avatar.value.length) {
    message.value = 'incompleted data';
    return;
  }

  message.value = 'loading...';
  const formData = new FormData();
  for(const name in person) {
    formData.append(name, person[name]);
  }

  formData.append('avatar', avatar.value[0]);
  try {
    const response = await fetch(`${import.meta.env.VITE_APP_API_URL}/register-user`, {
      method: 'POST',
      body: formData,
    });

    message.value = response.message;

  } catch (error) {
    message.value = error;
    console.error(error);
  }
}
</script>

<style lang="scss" scoped>

</style>