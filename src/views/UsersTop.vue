<template>
  <div class="container py-5">
    <NavTabs />
    <h1 class="mt-5">美食達人</h1>
    <hr />
    <div class="row text-center">
      <div v-for="user in users" :key="user.id" class="col-3">
        <router-link :to="{name: 'user', params: {id: user.id}}">
          <img :src="user.image | emptyImage" width="140px" height="140px" />
        </router-link>
        <h2>{{user.name}}</h2>
        <span class="badge badge-secondary">追蹤人數：{{user.FollowerCount}}</span>
        <p class="mt-3">
          <router-link
            v-if="currentUser.id === user.id"
            :to="{name: 'user-edit', params: {id: user.id}}"
            role="button"
            class="btn red-btn btn-sm mt-0"
          >編輯</router-link>
          <button
            v-else-if="user.isFollowed"
            @click.stop.prevent="removeFollowing(user.id)"
            type="button"
            class="btn red-btn btn-sm mt-0"
          >取消追蹤</button>
          <button
            v-else
            @click.stop.prevent="addFollowing(user.id)"
            type="button"
            class="btn red-btn btn-sm mt-0"
          >追蹤</button>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from "vuex";
import NavTabs from "../components/NavTabs";
import usersAPI from "./../apis/users";
import { Toast } from "./../utils/helpers";
import { emptyImageFilter } from "./../utils/mixins";

export default {
  name: "RestaurantsFeeds",
  components: {
    NavTabs
  },
  mixins: [emptyImageFilter],
  data() {
    return {
      users: []
    };
  },
  computed: {
    ...mapState(["currentUser"])
  },
  created() {
    this.fetchTopUsers();
  },
  methods: {
    async fetchTopUsers() {
      try {
        const { data, statusText } = await usersAPI.getTopUsers();

        if (statusText !== "OK") {
          throw new Error(statusText);
        }

        this.users = data.users;
      } catch (error) {
        Toast.fire({
          type: "error",
          title: "無法取得美食達人，請稍後再試",
          icon: "error"
        });
      }
    },
    async addFollowing(userId) {
      try {
        const { data, statusText } = await usersAPI.addFollowing({
          userId
        });

        if (statusText !== "OK" || data.status !== "success") {
          throw new Error(statusText);
        }

        this.users = this.users
          .map(user => {
            if (user.id !== userId) {
              return user;
            }

            return {
              ...user,
              FollowerCount: user.FollowerCount + 1,
              isFollowed: true
            };
          })
          .sort((a, b) => b.FollowerCount - a.FollowerCount);
      } catch (error) {
        Toast.fire({
          type: "error",
          title: "無法加入追蹤，請稍後再試",
          icon: "error"
        });
      }
    },
    async removeFollowing(userId) {
      try {
        const { data, statusText } = await usersAPI.deleteFollowing({
          userId
        });

        if (statusText !== "OK" || data.status !== "success") {
          throw new Error(statusText);
        }

        this.users = this.users
          .map(user => {
            if (user.id !== userId) {
              return user;
            }

            return {
              ...user,
              FollowerCount: user.FollowerCount - 1,
              isFollowed: false
            };
          })
          .sort((a, b) => b.FollowerCount - a.FollowerCount);
      } catch (error) {
        Toast.fire({
          type: "error",
          title: "無法取消追蹤，請稍後再試",
          icon: "error"
        });
      }
    }
  }
};
</script>