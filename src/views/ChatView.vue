<script setup lang="ts">
import { onMounted } from "vue";
import { useRouter } from "vue-router";

const router = useRouter();

const logout = () => {
  localStorage.removeItem("token");
  router.replace("/");
};

onMounted(() => {
  const checkToken = localStorage.getItem("token");
  if (!checkToken) {
    router.replace({ name: "home" });
  }
});
</script>
<template>
  <div>
    <h2>Chat App List</h2>
    <button @click="logout">Logout</button>

    <ul>
      <li>Samuel</li>
      <li>Samsudin</li>
    </ul>
  </div>
</template>