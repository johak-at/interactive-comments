<script setup>
import { useStore } from "~/store/store";
import { storeToRefs } from "pinia";
import { Icon } from "@iconify/vue";
import ReplyField from "~/components/ui/ReplyField.vue";
import LoginRegister from "~/components/ui/LoginRegister.vue";

// useStore() and name handling:
const store = useStore();
const name = storeToRefs(store).name;
const server = "http://localhost:3333/comments";
// const data = storeToRefs(store).data;

//add the data of data.json to a new array using onMounted() and log the date of the comments

const data = ref([]);
onMounted(async () => {
  getComments();
});

async function getComments() {
  const response = await fetch(server);
  data.value = await response.json();
}

//make a function to add comments to the database using post

async function addComments() {
  console.log(commentInput.value);
  if (commentInput.value === "") {
    return;
  }

  await fetch(server, {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      content: commentInput.value,
      rated: 0,
      createdAt: new Date().toLocaleString(),
      score: 0,
      user: {
        image: {
          png: "image-amyrobson.png",
          webp: "image-amyrobson.webp",
        },
        username: "User",
      },
      replies: [],
    }),
  });
  commentInput.value = "";
  getComments();
}

//make a function to be able to like comments when clicking the + icon use the id of the icon

async function addLike(id) {
  if (data.value.find((comment) => comment.id === id).rated == -1) {
    await fetch(`${server}/${id}`, {
      method: "PATCH",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        score: data.value.find((comment) => comment.id === id).score + 1,
        rated: 0,
      }),
    });
  } else if (data.value.find((comment) => comment.id === id).rated == 0) {
    await fetch(`${server}/${id}`, {
      method: "PATCH",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        score: data.value.find((comment) => comment.id === id).score + 1,
        rated: 1,
      }),
    });
  }

  getComments();
}
async function addDisLike(id) {
  if (data.value.find((comment) => comment.id === id).rated == 0) {
    await fetch(`${server}/${id}`, {
      method: "PATCH",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        score: data.value.find((comment) => comment.id === id).score - 1,
        rated: -1,
      }),
    });
  }

  if (data.value.find((comment) => comment.id === id).rated == 1) {
    await fetch(`${server}/${id}`, {
      method: "PATCH",
      headers: {
        "Content-Type": "application/json",
      },
      body: JSON.stringify({
        score: data.value.find((comment) => comment.id === id).score - 1,
        rated: 0,
      }),
    });
  }

  getComments();
}

async function addSubLike(commentId, replyId) {
  if (
    data.value
      .find((comment) => comment.id === commentId)
      .replies.find((reply) => reply.id === replyId).rated == -1
  ) {
    data.value
      .find((comment) => comment.id === commentId)
      .replies.find((reply) => reply.id === replyId).score += 1;
    data.value
      .find((comment) => comment.id === commentId)
      .replies.find((reply) => reply.id === replyId).rated = 0;
    await fetch(`${server}/${commentId}`, {
      method: "PUT",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(
        data.value.find((comment) => comment.id === commentId)
      ),
    });
  } else if (
    data.value
      .find((comment) => comment.id === commentId)
      .replies.find((reply) => reply.id === replyId).rated == 0
  ) {
    data.value
      .find((comment) => comment.id === commentId)
      .replies.find((reply) => reply.id === replyId).score += 1;
    data.value
      .find((comment) => comment.id === commentId)
      .replies.find((reply) => reply.id === replyId).rated = 1;
    await fetch(`${server}/${commentId}`, {
      method: "PUT",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(
        data.value.find((comment) => comment.id === commentId)
      ),
    });
  }

  getComments();
}

async function addSubDislike(commentId, replyId) {
  if (
    data.value
      .find((comment) => comment.id === commentId)
      .replies.find((reply) => reply.id === replyId).rated == 0
  ) {
    data.value
      .find((comment) => comment.id === commentId)
      .replies.find((reply) => reply.id === replyId).score -= 1;
    data.value
      .find((comment) => comment.id === commentId)
      .replies.find((reply) => reply.id === replyId).rated = -1;
    await fetch(`${server}/${commentId}`, {
      method: "PUT",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(
        data.value.find((comment) => comment.id === commentId)
      ),
    });
  } else if (
    data.value
      .find((comment) => comment.id === commentId)
      .replies.find((reply) => reply.id === replyId).rated == 1
  ) {
    data.value
      .find((comment) => comment.id === commentId)
      .replies.find((reply) => reply.id === replyId).score -= 1;
    data.value
      .find((comment) => comment.id === commentId)
      .replies.find((reply) => reply.id === replyId).rated = 0;
    await fetch(`${server}/${commentId}`, {
      method: "PUT",
      headers: { "Content-Type": "application/json" },
      body: JSON.stringify(
        data.value.find((comment) => comment.id === commentId)
      ),
    });
    getComments();
  }
}

async function deleteComment(id) {
  await fetch(`${server}/${id}`, {
    method: "DELETE",
  });
  getComments();
}

async function deleteReply(commentId, replyId) {
  data.value
    .find((comment) => comment.id === commentId)
    .replies.splice(
      data.value
        .find((comment) => comment.id === commentId)
        .replies.findIndex((reply) => reply.id === replyId),
      1
    );
  await fetch(`${server}/${commentId}`, {
    method: "PUT",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify(
      data.value.find((comment) => comment.id === commentId)
    ),
  });
  getComments();
}

const newName = ref("");
function setName() {
  store.name = newName.value;
  newName.value = "";
}
</script>

<template>
  <!-- <LoginRegister></LoginRegister> -->
  <div class="flex items-center flex-col text-black">
    <div class="flex flex-col w-2xl">
      <div v-for="comment in data" :key="comment.id">
        <div
          class="text-black pa-10 text-center text-left flex flex-row rounded-lg my-3 bg-white min-h-45 items-center"
        >
          <div
            class="flex flex-col justify-center px-2 w-18 items-center h-27 rounded-lg mr-3 bg-[#f5f6fa] text-blue-900 font-semibold"
          >
            <button class="space-y-3.5">
              <Icon v-if="comment.rated==1" text-5 icon="ic:round-plus" @click="addLike(comment.id)" class="text-red" />
              <Icon v-else text-5 icon="ic:round-plus" @click="addLike(comment.id)" />
              <p>{{ comment.score }}</p>
              <Icon v-if="comment.rated==-1"
                text-5
                icon="ic:round-minus"
                @click="addDisLike(comment.id)"
                class="text-red"
              />
              <Icon v-else
                text-5
                icon="ic:round-minus"
                @click="addDisLike(comment.id)"
              />
            </button>
          </div>
          <div class="flex flex-col text-left min-w-128">
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
                  :value="comment.id"
                  class="btn btn-outline btn-error flex flex-row"
                  @click="deleteComment(comment.id)"
                >
                  Delete
                </button>
              </div>
            </div>
            <div class="pl-2">{{ comment.content }}</div>
          </div>
        </div>

        <div
          v-for="reply in comment.replies"
          class="text-black pa-10 text-center text-left flex flex-row rounded-lg my-3 bg-white min-h-45 items-center ml-18"
        >
          <div
            class="flex flex-col justify-center px-2 w-18 items-center h-27 rounded-lg mr-3 bg-[#f5f6fa] text-blue-900 font-semibold"
          >
            <button class="space-y-3.5">
              <Icon v-if="reply.rated==1"
                text-5
                icon="ic:round-plus"
                @click="addSubLike(comment.id, reply.id)"
                class="text-red"
              />
              <Icon v-else
                text-5
                icon="ic:round-plus"
                @click="addSubLike(comment.id, reply.id)"
              />
              <p>{{ reply.score }}</p>
              <Icon v-if="reply.rated==-1"
                text-5
                icon="ic:round-minus"
                @click="addSubDislike(comment.id, reply.id)"
                class="text-red"
              />
              <Icon v-else
                text-5
                icon="ic:round-minus"
                @click="addSubDislike(comment.id, reply.id)"
              />
            </button>
          </div>
          <div class="flex flex-col text-left">
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
                  :value="comment.id"
                  class="btn btn-outline btn-error flex flex-row"
                  @click="deleteReply(comment.id, reply.id)"
                >
                  Delete
                </button>
              </div>
            </div>
            <div class="pl-2">{{ reply.content }}</div>
          </div>
        </div>
        <ReplyField
        :commentId="comment.id"
        :data="data"
        :server="server"
      ></ReplyField>
      </div>
      

      <div
        class="text-black pa-10 text-center text-left flex flex-row rounded-lg mt-3 mb-15 bg-white min-h-35 items-center justify-between"
      >
        <textarea
          name="comments"
          id="commentInput"
          placeholder="Add a comment..."
          maxrows="6"
          class="text"
          @keyup.enter="addComments"
        ></textarea>
        <button
          class="btn bg-blue-900 w-23 text-size-4.25"
          @click="addComments"
        >
          Send
        </button>
      </div>
    </div>
  </div>
</template>

<style>
* {
  /* border: 1px solid red; */
}

.text {
  border: 1px solid #c8ccda;
  border-radius: 10px;
  resize: none;
  width: 400px;
  height: 100px;
  margin-left: 25px;
  padding-left: 10px;
  padding-top: 10px;
}
</style>
