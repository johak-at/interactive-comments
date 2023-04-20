<script setup>
import { useStore } from "~/store/store";
import { storeToRefs } from "pinia";

// useStore() and name handling:
const store = useStore();
const name = storeToRefs(store).name;

//add the data of data.json to a new arraw using onmounted() and log the date of the comments

const data = ref([]);
onMounted(async () => {
  const res = await fetch("data.json");
  data.value = await res.json();
});
console.log(data);

const newName = ref("");
function setName() {
  store.name = newName.value;
  newName.value = "";
}

// object for comments
const comments = ref([{ name: "John", comment: "Hello" }]);
</script>

<template>
  <div class="">
    <h1 class="text-black">{{ comments[0].comment }}</h1>
    <div text-black>{{ data }}</div>
  </div>
</template>

<style>
* {
  border: 1px solid red;
}
</style>
