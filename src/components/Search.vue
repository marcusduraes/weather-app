<script setup>
import { ref} from 'vue';
import WeatherInterface from "./WeatherInterface.vue";

const props = defineProps({
  inputval: Object,
});

const show = ref(false);

const query = ref(`${props.inputval.city}, ${props.inputval.region_code}`);

const inputCity = ref();
const inputState = ref();

const search = () => {
  if(!query.value.includes(",")) return;
  [inputCity.value, inputState.value] = query.value.split(',');
  show.value = true;
}

</script>

<template>
  <div>
    <form action="" method="get" @submit.prevent="search()">
      <input type="text" name="query" id="query" class="search" v-model="query" placeholder="ex. Curitiba, PR">
      <button type="submit">Obter clima</button>
    </form>
  </div>
  <WeatherInterface v-if="show" :city="inputCity" :state="inputState"/>
</template>

<style scoped>
.search{
  padding: 10px 5px;
  border-radius: 5px;
  border: 1px solid lightgray;
}
</style>