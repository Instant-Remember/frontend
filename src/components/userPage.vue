<template>
    <div class="userPage">
        <profileHeader></profileHeader>
        <postEditor></postEditor>
        <post></post>
    </div>
</template>
  
<script>
import post from './post.vue'
import postEditor from './postEditor.vue'
import profileHeader from './profileHeader.vue'

export default {
    components: {postEditor, post, profileHeader}
}

</script>
  
<style scoped>
    @media(max-width: 375px){
        .userPage{
            height: calc(100vh + 550px);
        }
    }
</style>