<script setup lang="ts">
import liff from "@line/liff";
import { onMounted, ref } from "vue";
export interface Profile {
    userId: string;
    displayName: string;
    pictureUrl?: string;
    statusMessage?: string;
}

const message = ref('')
const profile = ref<Profile>({
    userId: "",
    displayName: "",
})

const context = ref<Object | null>({})
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
        context.value = liff.getContext();

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
            <div>
                {{ context }}
            </div>
        </div>

    </div>
</template>

<style lang="scss" scoped></style>