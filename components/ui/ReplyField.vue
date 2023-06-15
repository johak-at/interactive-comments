<script setup>
const prop = defineProps(["commentId", "data", "server", "username"]);
const text = ref("");
let showTextarea = ref(false);
async function addReplies() {
  if (text.value === "") {
    return;
  }

  prop.data
    .find((comment) => comment.id === prop.commentId)
    .replies.push({
      id: Math.random().toString(36).substr(2, 9),
      rated: 0,
      content: text.value,
      createdAt: new Date().toLocaleString(),
      score: 0,
      user: {
        image: {
          png: "Profile.jpg",
          webp: "Profile.jpg",
        },
        username: prop.username,
      },
    });
  await fetch(`${prop.server}/${prop.commentId}`, {
    method: "PUT",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(
      prop.data.find((comment) => comment.id === prop.commentId)
    ),
  });

  text.value = "";
}
</script>
<template>
  
  <div v-if="showTextarea==false">
    <button class="btn text-black bg-white w-23 text-size-4.25 ml-18 min-w-150" @click="showTextarea=true">Reply</button>
  </div>
  <div v-else
    class="text-black pa-10 text-center text-left flex flex-row rounded-lg mt-3 mb-15 bg-white min-h-35 items-center justify-between ml-18"
  >
  <button @click="showTextarea=false">Hide Reply</button>
    <textarea
      name="replies"
      v-model="text"
      placeholder="Add a reply..."
      maxrows="6"
      class="text"
      @keyup.enter="addReplies"
    ></textarea>
    <button class="btn bg-blue-900 w-23 text-size-4.25 ml-2" @click="addReplies">
      Send
    </button>
  </div>
</template>
