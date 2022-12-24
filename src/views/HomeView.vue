<script setup lang="ts">
import { onMounted, ref } from "vue";
import { useRouter } from "vue-router";
import TheWelcome from "../components/TheWelcome.vue";

const router = useRouter();

const username = ref("");
const password = ref("");

const loginAction = async () => {
  const url = "http://127.0.0.1:3000/api/v1/";
  await fetch(`${url}auth/login`, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      // 'Content-Type': 'application/x-www-form-urlencoded',
    },
    mode: "cors",
    body: JSON.stringify({
      username: username.value,
      password: password.value,
    }),
  })
    .then((res) => res.json())
    .then((response) => {
      console.log(response);
      if (response?.error) {
        throw new Error(response.error);
      }
      alert("eak " + JSON.stringify(response.token));

      localStorage.setItem("token", response.token);

      router.push({ name: "chats" });
    })
    .catch((error) => {
      alert(error);
    });
};

onMounted(() => {
  const checkToken = localStorage.getItem("token");
  if (checkToken) {
    router.replace({ name: "chats" });
  }
});

const pushToRegister = () => {
  router.push({name: "register"})
}
</script>

<template>
  <main>
    <!-- <TheWelcome /> -->
    <div style="display: flex; flex-direction: column; width: 50%">
      <label for="Username">Username </label>
      <input id="Username" type="text" v-model="username" />
      <label for="Password">Password </label>
      <input id="Password" type="password" v-model="password" />
      <br />
      <div>
        <button @click="loginAction">Login</button>
        <button @click="pushToRegister">Register</button>
      </div>
    </div>
  </main>
</template>
