<template>
  <user-card-list :user-list="userList" />
  <van-empty v-if="!userList || userList.length < 1" description="搜索结果为空"/>
</template>

<script setup>
import {onMounted, ref} from "vue";
import { useRoute } from "vue-router";
import myAxios from "../plugins/myAxios.ts";
import qs from 'qs';
import UserCardList from "../components/UserCardList.vue";

const route = useRoute();
const { tags } = route.query;
const userList = ref([]);

onMounted(async () => {
  const userListData = await myAxios.get('/user/search/tags', {
    withCredentials: false,
    params: {
      tagNameList: tags
    },
    paramsSerializer: params => {
      return qs.stringify(params, { indices: false })
    }
  })
      .then(function (response) {
        console.log('/user/search/tags succeed', response);
        return response?.data
      })
      .catch(function (error) {
        console.error('/user/search/tags error', error);
        return [];
      })
  if (userListData) {
    userListData.forEach(user => {
      if (user.tags) {
        user.tags = JSON.parse(user.tags);
      }
    })
    userList.value = userListData;
  }
})



// const mockUser = ref({
//   id: 2131,
//   username: 'Octaver',
//   userAccount: 'Octaver',
//   profile: '一条咸鱼',
//   gender: 0,
//   phone: '11111111111',
//   email: 'Octaver2131@outlook.com',
//   planetCode: '2131',
//   avatarUrl: 'https://avatars.githubusercontent.com/u/199230757?v=4',
//   tags: ['c++', 'java', 'python'],
//   createTime: new Date(),
// })

</script>

<style scoped>

</style>