<template>
    <form @submit.prevent>
      <h4>Add post</h4>
      <my-input 
        v-model="post.title"
        type="text" 
        placeholder="Post name"
      />
      <my-input 
        v-model="post.body"
        type="text" 
        placeholder="About post"
      />
      <my-button 
        style="align-self: flex-end; margin-top: 15px;"
        @click="createPost"
      >Create</my-button>
    </form>

</template>

<script>
    export default {
        data() {
            return {
                post: {
                    title: '',
                    body: '',
                }
            }
        },
        methods: {
            createPost() {
                if (this.post.title.length && this.post.body.length) {
                    this.post.id = Date.now()
                    this.$emit('create', this.post)
                    this.post = {
                        title: '',
                        body: '',
                }
            }
        }
    },
}
</script>

<style scoped>


  form {
    display: flex;
    flex-direction: column;
  }



</style>