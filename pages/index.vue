<script setup>
import { useStore } from "~/store/store";
import { storeToRefs } from "pinia";
import { Icon } from "@iconify/vue";
import ReplyField from "~/components/ui/ReplyField.vue";
import LoginRegister from "~/components/ui/LoginRegister.vue";

const store = useStore();
const name = storeToRefs(store).name;
const server = "http://localhost:3333/comments";
let username = ref("Guest");

const data = ref([]);
onMounted(async () => {
  getComments();
});

async function getComments() {
  const response = await fetch(server);
  data.value = await response.json();
}


async function addComments() {
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
          png: "data:image/jpeg;base64,/9j/4AAQSkZJRgABAQAAAQABAAD/2wCEAAkGBw0NDQ8NDQ0NDg4PDQ0PDxAODRANFQ4NFRUWFhUXFRgYHSggGBsxGxUVITEhJSkrLi4uFx8zODMtNygtLisBCgoKBQUFDgUFDisZExkrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrKysrK//AABEIAOEA4QMBIgACEQEDEQH/xAAaAAEAAwEBAQAAAAAAAAAAAAAAAQUGBAMC/8QAOxABAAIBAAcEBwUGBwAAAAAAAAECAwQFESExQVESImFxEzJSgZGh0QZicrHBQoKSouHwIzNDg7LC8f/EABQBAQAAAAAAAAAAAAAAAAAAAAD/xAAUEQEAAAAAAAAAAAAAAAAAAAAA/9oADAMBAAIRAxEAPwDegAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABD0rgyTwpef3ZB5j1nRssccd/4JedqzHGJjziYBAAAAAAAAAAAAAAAAAAAAAAA+8OK17RWkbbTyaDQNVUxbLX2Xv8AKvl9QVOiary5d+zsV62j8oWuDU+Gvrbck+O6PhCxAfGPDSvq0rXyiIeiAEvm1YndMRPnG1IDkzatwX/Yis9a938lZpOpL1347duPZndPx4SvgGNvSazstE1mOUxslDW6VomPNGy8eUxumPKWd0/QL4J396kzutH5T0kHIAAAAAAAAhIAAAAAAA+8OK2S0UrG2Zl8NJqjQvRU7Vo79ojb92OgPbQNCrhrsjfafWt1n6OkAAAAAAAAAEXpFomtoiYmNkxPNIDM6z0CcNtsbZxzPdnpPSXE2GfFXJWaWjbExvZTStHnFeaW5cJ615SDyAAAAAAAAAAAAAB36n0b0mWJmO7TvT4zyj++jSODUuDsYYnnee1Ply+TvABIIEgIEgIEgIEgIEgIVmvdG7eP0kR3qcfGnP6rRFqxMTE8JiYnyBjB6aRi7F7U9m0x7uXyeYAAAAAAAAAACa12zERxmYiPOUPfQa7c2OPv1Bq6VisRWOERER5QkAEoSAAAAAAAAAAAADOa9x9nNt9qsT743K5c/aOu/FPhkj/ipgAAAAAAAAAAHTq6f8fH+OHM9MF+zelul6z7toNeAAlCQAAAAAAAAAAAAUv2j/0v9z/qpVt9ob7clK+zWZ+M/wBFSAAAAAAAAAAAADWaBm9JipbrWNvnG6Xupvs/pHrYp/FX9VyAlCQAAAAAAAAAAAc+n6R6LFa/PZsr+KeAM9rTL28955RPZj3bvz2uQAAAAAAAAAAAAAemDLOO9b141nb59YavR81clIvXhMfDwZB36p070Nuzaf8ADtO/7s9QaRKInbvSAAAAAAAAAAAzuutL9JfsVnuU+d+bv1xrD0cejpPfmN8+xH1Z4AAAAAAAAAAAAAAAAFlqzWc4u5ffj5Txmn9GgpeLRE1mJieExO3axrp0PTcmGe7O2vOs8J+gNWODRNa4sm6Z7Fulp5+Eu8AAAAAHhpGl48Ud+0R4cZn3A91ZrLWkY9tMey2ThM8Yp5+Lh03W98m2uPbSvXb3p+isBNrTMzMztmZ2zM85QAAAAAAAAAAAAAAAAJrWZnZETM9IjaCBYYNUZr75iKR96d/wh9aTqbJSNtJjJHOI3T/UFa98GmZcfqXtEdPWj4S8bVmJ2TExPSY2IBa4teZI9albeW2r3rr2vPFb3WiVGAvZ17Tljv75rDyvr2f2ccR522qcB25taZ77u32Y6ViI+fFxzO2ds756ygAHXoursuXhXs19q27/ANdWbUmSI20tW3hPdBVD1zYL452XrNfON3xeQAAAAAAAAAAAAAmtZtMViJmZnZERzlodW6sriiL32WyfGK+Xj4g4dB1Pa+y2XbSvs/tT9F1o+jY8UbKViPHnPnL2AAAeWbR6ZI2XpW3nDgzakxT6trU/mj5rQBQX1Hkj1b0t5xNfq8p1Pn6Vn95pAGajVGkezH8UPSmpM08ZpHvmWhAU+LUUR6+SZ8K17P1d2DV+HH6tI29bd6fm6gAAHzasWjZMRMdJjaq9M1NW2/FPYn2Z3xP0WwDHZsVsduzes1mOv6dXw1ul6LTNXs3jynnWfBmtN0S2G3ZtvifVtHCY+oOcAAAAAAAAFlqXRPSX7do7tJ+N/wC9/wAAd+p9A9HX0l479o4exH1WSQECQAAAAAAAAAAAAAAB46Vo9ctJpbhPCek9YewDIaTgtivNLcY+ccpeTR650T0mPtVjv03x415wzgAAAAAAERyjj+rW6Fo8Ysdac4jf425qDU2Dt5omeFI7U+fL+/BpgEJAQJAAAAAAAAAAAAAAAAAGW1po3ostoj1bd6vlPJqVXr7B2scXjjSf5Z4/oDPgAAAAAuPs7xyeVP1XaQECQEAAAAAAAACQECQAAAAAABy6z/yMn4JAGVAAAB//2Q==",
          webp: "data:image/jpeg;base64,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",
        },
        username: username.value,
      },
      replies: [],
    }),
  });
  commentInput.value = "";
  getComments();
}

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
  <input type="text" v-model="username" class="text-[#f5f6fa]"> 
  <Button>Submit</Button>

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
          <div class="flex flex-col text-left ml-6 ">
            <div class="flex flex-row items-center justify-between pb-4 min-w-128">
              <div class="flex flex-row items-center">

                <img :src="comment.user.image.webp" class=" rounded-full w-8 " />
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
          class="text-black pa-10 text-center text-left flex flex-row rounded-lg my-3 bg-white min-h-45 items-center ml-18">
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
            <div class="flex flex-row items-center justify-between pb-4 min-w-118">
              <div class="flex flex-row items-center">
                <img :src="reply.user.image.webp" class="w-8 rounded-full" />
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
        :username="username"
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
