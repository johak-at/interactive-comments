<script setup>

const user = "http://localhost:3333/User";
const userName = ref('');
const newEmail = ref('');
const newPassword = ref('');
const email = ref('');
const password = ref('');
let login = ref(false);

async function makeAccount() {
    await fetch(user, {
        method: "POST",
        headers: {
            "Content-Type": "application/json",
        },
        body: JSON.stringify({
            username: userName.value,
            email: newEmail.value,
            password: newPassword.value,
        }),
    });
    userName.value = "";
    newEmail.value = "";
    newPassword.value = "";
}
</script>




<template>
    <div text-black>
        <div v-if="!login">
            <h2>Create your very own account for free!</h2>

            <input type="text" v-model="userName" placeholder="username" />
            <input type="text" v-model="newEmail" placeholder="email" />
            <input type="text" v-model="newPassword" placeholder="password" />

            <button @click="makeAccount">Create Account</button>
            <button @click="login = true">Already have an account?</button>
        </div>
        <div v-if="login">
            <input type="text" v-model="email" placeholder="email" />
            <input type="text" v-model="password" placeholder="password" />
            <button @click="login = false">Want to create an account?</button>
        </div>
    </div>
</template>