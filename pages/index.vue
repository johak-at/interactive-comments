<script setup>
import { Icon } from "@iconify/vue";
import { Camera, CameraResultType } from "@capacitor/camera";
import { useStore } from "~/store/store";
import { storeToRefs } from "pinia";

// useStore() and name handling:
const store = useStore();
const name = storeToRefs(store).name;

const newName = ref("");
function setName() {
  store.name = newName.value;
  newName.value = "";
}

// object for comments
const comments = ref([{ name: "John", comment: "Hello" }]);

//geolocation via VueUse:
const { coords, locatedAt, error, resume, pause } = useGeolocation();

// taking a picture and displaying it:
const imageUrl = ref(null);
async function takePic() {
  const image = await Camera.getPhoto({
    quality: 90,
    allowEditing: true,
    resultType: CameraResultType.Uri,
  });
  imageUrl.value = image.webPath;
}

definePageMeta({
  alias: "/home",
});
</script>

<template>
  <div class="">
    <h1 class="text-black">{{ comments[0].comment }}</h1>
  </div>
</template>

<style>
* {
  border: 1px solid red;
}
</style>
