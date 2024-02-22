<template>
    <div class="friendPage">
        <profileHeader class="ph"></profileHeader>
        <post class="pst"></post>
    </div>
</template>
  
<script>
import post from './post.vue'
import postEditor from './postEditor.vue'
import profileHeader from './profileHeader.vue'

export default {
    components: { postEditor, post, profileHeader },
}

</script>
  
<style scoped></style>