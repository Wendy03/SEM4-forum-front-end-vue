<template>
  <div>
    <h2 class="my-4">所有評論：</h2>

    <div v-for="comment in restaurantComments" :key="comment.id">
      <blockquote class="blockquote mb-0">
        <button
          v-if="currentUser.isAdmin"
          type="button"
          class="btn btn-danger float-right"
          @click.stop.prevent="handleDeleteButtonClick(comment.id)"
        >Delete</button>
        <h3>
          <router-link :to="{ name: 'user', params: {id: comment.User.id }}">{{comment.User.name}}</router-link>
        </h3>
        <p>{{comment.text}}</p>
        <footer class="blockquote-footer">{{ comment.createdAt | fromNow }}</footer>
      </blockquote>
      <hr />
    </div>
  </div>
</template>

<script>
import { mapState } from "vuex";
import commentsAPI from "./../apis/comments";
import { Toast } from "./../utils/helpers";
import { fromNowFilter } from "../utils/mixins";

export default {
  mixins: [fromNowFilter],
  props: {
    restaurantComments: {
      type: Array,
      required: true
    }
  },
  computed: {
    ...mapState(["currentUser"])
  },
  methods: {
    async handleDeleteButtonClick(commentId) {
      try {
        const { data, statusText } = await commentsAPI.delete({ commentId });
        if (statusText !== "OK" || data.status !== "success") {
          throw new Error(statusText);
        }
        this.$emit("after-delete-comment", commentId);
        Toast.fire({
          icon: "success",
          title: "移除評論成功"
        });
      } catch (error) {
        Toast.fire({
          icon: "error",
          title: "無法刪除餐廳資料，請稍後再試"
        });
      }
    }
  }
};
</script>