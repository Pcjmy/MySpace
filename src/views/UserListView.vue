<template>
  <ContentBase>
    <div class="card" v-for="user in users" :key="user.id" @click="open_user_profile(user.id)">
        <div class="card-body">
          <div class="row">
            <div class="col-1 img-field">
              <img class="img-fluid" :src="user.photo" @error="setDefaultImage">
            </div>
            <div class="col-11">
              <div class="username">{{ user.username }}</div>
              <div class="follower-count">{{ user.followerCount }}</div>
            </div>
          </div>
        </div>
      </div>
  </ContentBase>
</template>

<script>
import defaultImage from  '../assets/default.png'
import ContentBase from '../components/ContentBase';
import $ from 'jquery';
import { reactive } from 'vue';
import { useStore } from 'vuex'
import router from '@/router';

export default {
  name: "UserList",
  components: {
    ContentBase
  },
  setup() {
    const store = useStore();
    // let users = [{id: 18, username: 'yxc8', photo: 'https://cdn.acwing.com/media/user/profile/photo/1_lg_844c66b332.jpg', followerCount: 202}]

    let users = reactive([])

    $.ajax({
      url: "https://app165.acapp.acwing.com.cn/myspace/userlist/",
      type: "get",
      success(res) {
        for(let item of res) {
          users.push(item);
        }
        // console.log(reactive(res));
        // console.log(typeof reactive(res));
        // let obj = reactive(res);
        // for(let item of obj) {
        //   console.log(item,item.id);
        // }
        // users = reactive(res);
        // console.log(users);
        // users.info = res;
        // console.log(users.info);
        // console.log(reactive(res));
      }
    })

    const setDefaultImage = (e) => {
      e.target.src = defaultImage;
    }

    const open_user_profile = (userId) => {
      if (store.state.user.is_login) {
        router.push({
          name: 'userprofile',
          params: {
            userId: userId
          }
        })
      } else {
        router.push({name: 'login'})
      }
    }

    return {
      users,
      setDefaultImage,
      open_user_profile
    }
  }
}
</script>

<style scoped>
img {
  border-radius: 50%;
}

.username {
  font-weight: bold;
  height: 50%;
}

.follower-count {
  font-size: 12px;
  color: gray;
  height: 50%;
}

.card {
  margin-bottom: 20px;
  cursor: pointer;
}

.card:hover {
  box-shadow: 2px 2px 10px lightgrey;
  transition: 500ms;
}
</style>