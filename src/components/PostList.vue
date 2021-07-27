<template>
    <div
          v-if="posts.length"
    >
        <h3>Posts list</h3>
        <transition-group name="post-list">
            <post-item
                :key="post.id"
                v-for="post in posts"
                :post="post"
                @remove="$emit('remove', post)"
            />
        </transition-group>
    </div>
        <h2 v-else style="color: red">List Empty</h2>
</template>

<script>
import PostItem from '@/components/PostItem'
export default {
    components: { 
        PostItem 
        },
    props: {
        posts: {
            type: Object,
            required: true
        }
    }
}
</script>

<style scoped>
.post-list-item {
  display: inline-block;
  margin-right: 10px;
}
.post-list-enter-active,
.post-list-leave-active {
  transition: all .4s ease;
}
.post-list-enter-from,
.post-list-leave-to {
  opacity: 0;
  transform: translateX(130px);
}
.post-list-move {
  transition: transform 0.5s ease;
}

</style>