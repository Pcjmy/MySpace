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

export default {
  name: "UserList",
  components: {
      ContentBase,
      UserProfileInfo,
      UserProfilePosts,
      UserProfileWrite
  },
  setup() {
    const user = reactive({
      id: 1,
      username: "Pcjmy",
      lastName: "ZZU",
      firstName: "Pcjmy",
      followerCount: 0,
      is_followed: false,
    });

    const posts = reactive({
      count: 3,
      posts: [
        {
          id: 1,
          userId: 1,
          content: "Vue是一套用于构建用户界面的渐进式框架",
        },
        {
          id: 2,
          userId: 1,
          content: "Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统",
        },
        {
          id: 3,
          userId: 1,
          content: "组件系统是 Vue 的另一个重要概念，因为它是一种抽象，允许我们使用小型、独立和通常可复用的组件构建大型应用,几乎任意类型的应用界面都可以抽象为一个组件树",
        },
      ]
    });

    const follow = () => {
      if (user.is_followed) return;
      user.is_followed = true;
      user.followerCount ++ ;
    };

    const unfollow = () => {
      if (!user.is_followed) return;
      user.is_followed = false;
      user.followerCount -- ;
    };

    const post_a_post = (content) => {
      if(content.length==0) return ;
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