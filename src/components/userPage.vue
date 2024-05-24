<template>
    <div class="userPage">
        <profileHeader :backendURL="backendURL"></profileHeader>
        <postEditor :backendURL="backendURL"></postEditor>
        <post :backendURL="backendURL"></post>
    </div>
</template>

<script>
import post from './post.vue'
import postEditor from './postEditor.vue'
import profileHeader from './profileHeader.vue'

export default {
    components: { postEditor, post, profileHeader },

    props: {
        backendURL: {
            type: String,
            required: true
        }
    }
}
</script>

<style scoped>
@media(max-width: 375px) {
    .userPage {
        height: calc(100vh + 550px);
    }
}
</style>
