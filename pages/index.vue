<script setup>
import { useStore } from "~/store/store";
import { storeToRefs } from "pinia";
import { Icon } from "@iconify/vue";

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

//add a like counter to the comments

const likes = ref(0);
function addLike() {
  data.score.value++;
}

const newName = ref("");
function setName() {
  store.name = newName.value;
  newName.value = "";
}

// object for comments
const comments = ref([{ name: "John", comment: "Hello" }]);
</script>

<template>
  <div class="flex flex-col items-center">
    <div
      v-for="(comment, index) in data.comments"
      :key="comment.id"
      text-black
      pa-10
      w-2xl
      text-center
      text-left
      flex
      flex-row
      rounded-lg
      my-3
      bg-white
      min-h-45
      items-center
    >
      <div
        class="flex flex-col justify-center px-2 w-18 items-center h-27 rounded-lg mr-3 bg-[#f5f6fa] text-blue-900 font-semibold"
      >
        <button class="space-y-3.5">
          <Icon text-5 icon="ic:round-plus" />
          <p>{{ comment.score }}</p>
          <Icon text-5 icon="ic:round-minus" />
        </button>
      </div>
      <div class="flex flex-col">
        <div class="flex flex-row items-center justify-between pb-4">
          <div class="flex flex-row items-center">
            <!-- load the profile pictures from the json file -->
            <img :src="comment.user.image.webp" class="w-10 pl-2" />
            <p class="ml-3 font-medium">{{ comment.user.username }}</p>
            <p class="ml-3 italic text-gray-600">
              {{ comment.createdAt }}
            </p>
          </div>
          <div class="ml-3">
            <button
              class="flex flex-row text-blue-900 items-center font-semibold"
            >
              <Icon text-5 icon="fa:mail-reply" class="text-blue-900 pr-1.5" />
              Reply
            </button>
          </div>
        </div>
        <div class="pl-2">{{ comment.content }}</div>
      </div>
    </div>

    <!-- <div v-for="reply in comment.replies">
      <div
        class="flex flex-col justify-center px-2 w-18 items-center h-27 rounded-lg mr-3 bg-[#f5f6fa] text-blue-900 font-semibold"
      >
        <button class="space-y-3.5">
          <Icon text-5 icon="ic:round-plus" />
          <p>{{ reply.score }}</p>
          <Icon text-5 icon="ic:round-minus" />
        </button>
      </div>
      <div class="flex flex-col">
        <div class="flex flex-row items-center justify-between pb-4">
          <div class="flex flex-row items-center">
            <img :src="reply.user.image.webp" class="w-10 pl-2" />
            <p class="ml-3 font-medium">{{ reply.user.username }}</p>
            <p class="ml-3 italic text-gray-600">
              {{ reply.createdAt }}
            </p>
          </div>
          <div class="ml-3">
            <button
              class="flex flex-row text-blue-900 items-center font-semibold"
            >
              <Icon text-5 icon="fa:mail-reply" class="text-blue-900 pr-1.5" />
              Reply
            </button>
          </div>
        </div>
        <div class="pl-2">{{ reply.content }}</div>
      </div>
    </div> -->
  </div>
</template>

<style>
* {
  /* border: 1px solid red; */
}
</style>
