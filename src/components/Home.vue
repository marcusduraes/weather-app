<script setup>
import Search from "./Search.vue";
import { ref } from "vue";

const endpoint = ref({location: "https://ipapi.co/json/"});


const result = ref();
const isLoading = ref(true);
const getLocation = async () => {
  try {
    const res = await fetch(endpoint.value.location);
    const data = await res.json();
    result.value = data;
    isLoading.value = false;
  } catch (error) {
    console.log(error);
  }
}
getLocation();

</script>

<template>
  <div v-if="isLoading">
    Carregando...
  </div>
  <Search v-else :inputval="result"/>
</template>


<style scoped>

</style>