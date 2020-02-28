<template>
  <div class="container py-5">
    <!-- 1. 使用先前寫好的 AdminNav -->
    <AdminNav />

    <table class="table">
      <thead class="thead-dark">
        <tr>
          <th scope="col">#</th>
          <th scope="col">Email</th>
          <th scope="col">Role</th>
          <th scope="col" width="140">Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="user in users" :key="user.id">
          <th scope="row">{{user.id}}</th>
          <td>{{user.email}}</td>
          <td>{{user.isAdmin ? 'admin' : 'user'}}</td>
          <td>
            <button
              v-if="currentUser !== user.id"
              type="button"
              class="btn btn-link"
              @click.stop.prevent="toggleRole({userId: user.id, isAdmin:user.isAdmin} )"
            >{{user.isAdmin ? 'set as user' : 'set as admin'}}</button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import AdminNav from "@/components/AdminNav";

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

const dummyUser = {
  currentUser: {
    id: 1,
    name: "管理者",
    email: "root@example.com",
    image: "https://i.pravatar.cc/300",
    isAdmin: true
  },
  isAuthenticated: true
};

export default {
  components: {
    AdminNav
  },
  data() {
    return {
      users: [],
      currentUser: dummyUser.currentUser
    };
  },
  created() {
    this.fetchUser();
  },
  methods: {
    fetchUser() {
      this.users = dummyData.users;
    },
    toggleRole({ userId, isAdmin }) {
      this.users = this.users.map(user => {
        if (user.id !== userId) {
          return user;
        }
        return {
          ...user,
          isAdmin: !isAdmin
        };
      });
    }
  }
};
</script>