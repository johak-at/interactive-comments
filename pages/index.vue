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
    >
      <div class="flex flex-col justify-center px-2 w-20 items-center">
        <button>+</button> {{ comment.score }}
        <button>-</button>
      </div>
      <div class="flex flex-col">
        <div>
          <div>
            <!-- load the profile pictures from the json file -->
            <img :src="comment.user.image.webp" class="w-12 pl-2" />
          </div>
          <div></div>
        </div>
        <div class="pl-2">{{ comment.content }}</div>
      </div>

      <!-- <div v-for="reply in comment.replies" class="pl-15 py-4">
        <div class="flex flex-col px-2 w-20 items-center">
          <button>+</button> {{ reply.score }}
          <button>-</button>
        </div>
        <div class="flex flex-row">
          <div>
            <div><img src="" alt="" /></div>
            <div></div>
          </div>
          <div class="pl-2">{{ reply.content }}</div>
        </div>
      </div> -->
    </div>
  </div>
</template>

<style>
* {
  border: 1px solid red;
}
</style>
