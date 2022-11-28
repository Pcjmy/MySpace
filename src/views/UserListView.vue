<template>
  <ContentBase>
    <div class="card" v-for="user in users.info" :key="user.id" @click="open_user_profile(user.id)">
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
    const users = reactive({
      info: []
    })

    $.ajax({
      url: 'https://app165.acapp.acwing.com.cn/myspace/userlist/',
      type: "get",
      success(res) {
        users.info=res;
      }
    })

    const setDefaultImage = (e) => {
      e.target.src = defaultImage;
    }

    const open_user_profile = (userId) => {
      if (store.state.user.is_login) {
        router.push({
          name: 'useprofile',
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