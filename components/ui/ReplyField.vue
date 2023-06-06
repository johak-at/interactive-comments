<script setup>
const prop = defineProps(["commentId", "data", "server"]);
const text = ref("");
async function addReplies() {
    if (text.value === "") {
        return;
    }

    prop.data.find((comment) => comment.id === prop.commentId).replies.push({
        id: Math.random().toString(36).substr(2, 9),
        content: text.value,
        createdAt: new Date().toLocaleString(),
        score: 0,
        user: {
            image: {
                png: "image-amyrobson.png",
                webp: "image-amyrobson.webp",
            },
            username: "User",
        },
    });
    console.log(prop.commentId)
    await fetch(`${prop.server}/${prop.commentId}`, {
        method: "PUT",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify(
            prop.data.find(comment => comment.id === prop.commentId)
        ),
    });

    text.value = "";
}
</script>
<template>
    <div
        class="text-black pa-10 text-center text-left flex flex-row rounded-lg mt-3 mb-15 bg-white min-h-35 items-center justify-between ml-18">
        <textarea name="replies" v-model="text" placeholder="Add a reply..." maxrows="6" class="text"></textarea>
        <button class="btn bg-blue-900 w-23 text-size-4.25" @click="addReplies">
            Send
        </button>
    </div>
</template>