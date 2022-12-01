<template>
  <ContentBase>
  <div class="card">
        <div class="card-body">
            <div class="row">
                <div class="col-3">
                    <UserProfileInfo @follow="follow" @unfollow="unfollow" :user="user" />
                    <UserProfileWrite @post_a_post="post_a_post" />
                </div>
                <div class="col-9">
                    <UserProfilePosts :posts="posts" @delete_a_post="delete_a_post" />
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

    const posts = reactive({});

    const follow = () => {
      if (user.is_followed) return ;
      user.is_followed = true;
      user.followerCount ++ ;
    };

    const unfollow = () => {
      if (!user.is_followed) return ;
      user.is_followed = false;
      user.followerCount -- ;
    };

    const post_a_post = (content) => {
      if (content.length==0) return ;
      posts.count ++ ;
      posts.posts.unshift({
        id: posts.count,
        userId: 1,
        content: content,
      })
    };

    const delete_a_post = post_id => {
      posts.posts = posts.posts.filter(post=>post.id!==post_id);
      posts.count = posts.posts.length;
    }

    return {
      user,
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