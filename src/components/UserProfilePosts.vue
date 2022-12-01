<template>
    <div class="card">
        <div class="card-body">
            <div v-for="post in posts.posts" :key="post.id">
                <div class="card single-post">
                    <div class="card-body">
                        {{ post.content }}
                        <button @click="delete_a_post(post.id)" class="btn btn-danger btn-sm">删除</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "UserProfilePosts",
    props: {
        posts: {
            type: Object,
            required: true,
        },
    },
    setup(props, context) {
        const delete_a_post = post_id => {
            context.emit('delete_a_post',post_id)
        }
        return {
            delete_a_post
        }
    }
}
</script>


<style scoped>
.single-post {
    margin-bottom: 10px;
}
button {
    float: right;
}
</style>
