<script setup lang="ts">
import liff from "@line/liff";
import { onMounted, ref } from "vue";

const message = ref('')
const profile = ref({})

onMounted(async () => {
    try {
        await liff.init({
            liffId: import.meta.env.VITE_LIFF_ID
        })
        if (!liff.isLoggedIn()) {
            await liff.login()
            return;
        }
        message.value = "LIFF init succeeded.";
        profile.value = await liff.getProfile()

    } catch (e) {
        message.value = "LIFF init failed.";
        console.log(e);
    }
})
</script>

<template>
    <div class="break-words">
        <h1>create-liff-app</h1>
        <div v-if="message">
            <p>{{ message }}</p>
            <div>
                {{ profile }}
                <img :src="profile.pictureUrl" alt="pictureUrl">
            </div>
        </div>

    </div>
</template>

<style lang="scss" scoped></style>