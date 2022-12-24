<script setup lang="ts">
import { ref } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();
const username = ref("");
const email = ref("");
const password = ref("");

const backRoute = () => router.go(-1);
const registerAction = async () => {
  const url = "http://127.0.0.1:3000/api/v1/";
  await fetch(`${url}users`, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      // 'Content-Type': 'application/x-www-form-urlencoded',
    },
    mode: "cors",
    body: JSON.stringify({
      email: email.value,
      username: username.value,
      password: password.value,
    }),
  })
    .then((res) => res.json())
    .then((response) => {
      if (response?.error) {
        throw new Error(JSON.stringify(response.error));
      }
      router.replace("/")
      alert("success regist")
    })
    .catch((error) => {
      alert(error);
    });
};
</script>

<template>
  <div>
    <h2>Register</h2>
    <div style="display: flex; flex-direction: column; width: 50%">
      <label for="Username">Username </label>
      <input id="Username" type="text" v-model="username" />
      <label for="Email">Email </label>
      <input id="Email" type="text" v-model="email" />
      <label for="Password">Password </label>
      <input id="Password" type="password" v-model="password" />
      <br />
      <div>
        <button @click="registerAction">Register</button>
        <button @click="backRoute">Back</button>
      </div>
    </div>
  </div>
</template>