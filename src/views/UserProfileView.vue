<template>
  <ContentBase>
  <div class="card">
        <div class="card-body">
            <div class="row">
                <div class="col-3">
                    <UserProfileInfo @follow="follow" @unfollow="unfollow" :user="user" />
                    <UserProfileWrite @post_a_post="post_a_post" v-if="flag" />
                </div>
                <div class="col-9">
                    <UserProfilePosts :posts="posts" :user="user" @delete_a_post="delete_a_post" />
                </div>
            </div>
        </div>
    </div>
  </ContentBase>
</template>

<script>
import ContentBase from '../components/ContentBase';
import UserProfileInfo from '../components/UserProfileInfo';
import UserProfilePosts from '../components/UserProfilePosts';
import UserProfileWrite from '../components/UserProfileWrite';
import { reactive } from 'vue';
import { useStore } from 'vuex';
import { useRoute } from 'vue-router';
import { computed } from 'vue'
import { inject } from 'vue'
import $ from 'jquery';

export default {
  name: "UserList",
  components: {
      ContentBase,
      UserProfileInfo,
      UserProfilePosts,
      UserProfileWrite
  },
  setup() {
    const user = reactive({});
    const store = useStore();
    const route = useRoute();
    const userId = parseInt(route.params.userId);
    const fn = inject('reload', () => {}, true)

    $.ajax({
      url: "https://app165.acapp.acwing.com.cn/myspace/getinfo/",
      type: "GET",
      data: {
        user_id: userId
      },
      headers: {
        "Authorization": "Bearer " + store.state.user.access
      },
      success: (res) => {
        user.id = res.id;
        user.username = res.username;
        user.photo = res.photo;
        user.followerCount = res.followerCount;
        user.is_followed = res.is_followed;
      }
    })

    const posts = reactive({
      posts: []
    })

    $.ajax({
      url: "https://app165.acapp.acwing.com.cn/myspace/post/",
      type: "GET",
      data: {
        user_id: userId
      },
      headers: {
        "Authorization": "Bearer " + store.state.user.access
      },
      success: (res) => {
        posts.posts = res;
        posts.count = posts.posts.length;
      }
    })

    const follow = () => {
      if (user.is_followed) return ;
      user.is_followed = true;
      user.followerCount ++ ;
    }

    const unfollow = () => {
      if (!user.is_followed) return ;
      user.is_followed = false;
      user.followerCount -- ;
    }

    const post_a_post = (content) => {
      if (content.length === 0) return ;
      posts.count ++ ;

      $.ajax({
        type: "POST",
        data: {
          content: content
        },
        url: "https://app165.acapp.acwing.com.cn/myspace/post/",
        headers: {
          "Authorization": "Bearer " + store.state.user.access
        },
        success: (res) => {
          if (res.result === "success") {
            fn()
          }
        }
      })
    }

    const delete_a_post = () => {
      fn()
    }

    const flag = computed(() => {
      return userId === store.state.user.id
    })

    return {
      user,
      flag,
      follow,
      unfollow,
      posts,
      post_a_post,
      delete_a_post
    }
  }
};
</script>

<style scoped>
</style>