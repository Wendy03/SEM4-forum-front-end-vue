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
          <button
            v-if="user.isFollowed"
            type="button"
            class="btn btn-danger"
            @click.stop.prevent="removeFollowing(user.id)"
          >取消追蹤</button>
          <button
            v-else
            type="button"
            class="btn btn-primary"
            @click.stop.prevent="addFollowing(user.id)"
          >追蹤</button>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import NavTabs from "../components/NavTabs";

const dummyData = {
  users: [
    {
      id: 1,
      name: "root",
      email: "root@example.com",
      password: "$2a$10$alLLwv1Kn0tC9euHs6Llwen8uif7jQfU9DPaybXRuGn83ZfKzW56G",
      isAdmin: true,
      image: "https://i.imgur.com/JtQJRMZ.png",
      createdAt: "2019-11-20T06:25:42.685Z",
      updatedAt: "2019-11-21T09:55:30.970Z",
      Followers: [
        {
          id: 1,
          name: "root1",
          email: "root@example.com",
          password:
            "$2a$10$alLLwv1Kn0tC9euHs6Llwen8uif7jQfU9DPaybXRuGn83ZfKzW56G",
          isAdmin: true,
          image: null,
          createdAt: "2019-11-20T06:25:42.456Z",
          updatedAt: "2019-11-25T06:53:28.542Z",
          Followship: {
            followerId: 1,
            followingId: 2,
            createdAt: "2020-01-21T12:52:03.002Z",
            updatedAt: "2020-01-21T12:52:03.002Z"
          }
        },
        {
          id: 2,
          name: "user1",
          email: "user1@example.com",
          password:
            "$2a$10$ESv6iQjQ8oEe3/XGjw00PuSh1kjmG6Dkhd4YXa50boTlncJDxljAy",
          isAdmin: false,
          image: null,
          createdAt: "2019-11-20T06:25:42.685Z",
          updatedAt: "2019-11-21T09:55:30.970Z",
          Followship: {
            followerId: 2,
            followingId: 2,
            createdAt: "2019-12-08T15:48:13.364Z",
            updatedAt: "2019-12-08T15:48:13.364Z"
          }
        }
      ],
      FollowerCount: 2,
      isFollowed: true
    },
    {
      id: 2,
      name: "user1",
      email: "user1@example.com",
      password: "$2a$10$ESv6iQjQ8oEe3/XGjw00PuSh1kjmG6Dkhd4YXa50boTlncJDxljAy",
      isAdmin: false,
      image: "https://i.imgur.com/XooCt5K.png",
      createdAt: "2019-11-20T06:25:42.685Z",
      updatedAt: "2019-11-21T09:55:30.970Z",
      Followers: [
        {
          id: 1,
          name: "root1",
          email: "root@example.com",
          password:
            "$2a$10$alLLwv1Kn0tC9euHs6Llwen8uif7jQfU9DPaybXRuGn83ZfKzW56G",
          isAdmin: true,
          image: null,
          createdAt: "2019-11-20T06:25:42.456Z",
          updatedAt: "2019-11-25T06:53:28.542Z",
          Followship: {
            followerId: 1,
            followingId: 2,
            createdAt: "2020-01-21T12:52:03.002Z",
            updatedAt: "2020-01-21T12:52:03.002Z"
          }
        },
        {
          id: 2,
          name: "user1",
          email: "user1@example.com",
          password:
            "$2a$10$ESv6iQjQ8oEe3/XGjw00PuSh1kjmG6Dkhd4YXa50boTlncJDxljAy",
          isAdmin: false,
          image: null,
          createdAt: "2019-11-20T06:25:42.685Z",
          updatedAt: "2019-11-21T09:55:30.970Z",
          Followship: {
            followerId: 2,
            followingId: 2,
            createdAt: "2019-12-08T15:48:13.364Z",
            updatedAt: "2019-12-08T15:48:13.364Z"
          }
        }
      ],
      FollowerCount: 1,
      isFollowed: true
    },
    {
      id: 3,
      name: "user2",
      email: "user2@example.com",
      password: "$2a$10$0Izt5W6Q4iphLq1ORgsKQu9jbEFdRD3.UaX/HQQg0M4Hp8yWV.XuS",
      image: "https://i.imgur.com/1T9ddHb.png",
      isAdmin: false,
      createdAt: "2019-06-22T09:00:43.000Z",
      updatedAt: "2019-06-23T01:16:52.000Z",
      Followers: [],
      FollowerCount: 0,
      isFollowed: false
    }
  ]
};
export default {
  name: "RestaurantsFeeds",
  components: {
    NavTabs
  },
  filters: {
    emptyImage(src) {
      if (!src) {
        return "http://via.placeholder.com/300x300?text=No+Image";
      }
      return src;
    }
  },
  data() {
    return {
      users: []
    };
  },
  created() {
    this.fetchTopUsers();
  },
  methods: {
    fetchTopUsers() {
      this.users = dummyData.users;
    },
    addFollowing(userId) {
      this.users = this.users.map(user => {
        if (user.id !== userId) {
          return user;
        }
        return {
          ...user,
          isFollowed: true
        };
      });
    },
    removeFollowing(userId) {
      this.users = this.users.map(user => {
        if (user.id !== userId) {
          return user;
        }
        return {
          ...user,
          isFollowed: false
        };
      });
    }
  }
};
</script>