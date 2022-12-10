<template>
    <div class="card">
        <div class="card-body">
            <div v-for="post in posts.posts" :key="post.id">
                <div class="card single-post">
                    <div class="card-body">
                        {{ post.content }}
                        <button @click="delete_a_post(post.id)" class="btn btn-danger btn-sm" v-if="flag">删除</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { useStore } from 'vuex'
import { computed } from 'vue'
import $ from 'jquery'

export default {
    name: 'UserProfilePosts',
    props: {
        posts: {
            type: Object,
            required: true
        },
        user: {
            type: Object,
            required: true
        }
    },
    setup(props, context) {
        const store = useStore()
        const flag = computed(()=>{
            return store.state.user.id === props.user.id;
        })
        const delete_a_post = post_id => {
            $.ajax({
                url: 'https://app165.acapp.acwing.com.cn/myspace/post/',
                type: 'DELETE',
                headers: {
                    'Authorization': 'Bearer ' + store.state.user.access
                },
                data: {
                    post_id: post_id
                },
                success: (res) => {
                    if (res.result === 'success') {
                        context.emit('delete_a_post', post_id)
                    }
                }
            })
        }
        return {
            delete_a_post,
            flag
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
