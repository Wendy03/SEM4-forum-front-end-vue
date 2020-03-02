<template>
  <div class="container py-5">
    <form @submit.stop.prevent="handleSubmit">
      <div class="form-group">
        <label for="name">Name</label>
        <input
          id="name"
          v-model="name"
          type="text"
          name="name"
          class="form-control"
          placeholder="Enter Name"
          required
        />
      </div>

      <div class="form-group">
        <label for="image">Image</label>
        <img v-if="image" :src="image" class="d-block img-thumbnail mb-3" width="200" height="200" />

        <input
          id="image"
          type="file"
          name="image"
          accept="image/*"
          class="form-control-file"
          @change="handleFileChange"
        />
      </div>

      <button type="submit" class="btn btn-primary">Submit</button>
    </form>
  </div>
</template>

<script>
import usersAPI from "./../apis/users";
import { Toast } from "./../utils/helpers";

export default {
  data() {
    return {
      id: -1,
      name: "",
      email: "",
      image: ""
    };
  },
  created() {
    const { id } = this.$route.params;
    this.fetchUser(id);
  },
  methods: {
    async fetchUser(userId) {
      try {
        const { data, statusText } = await usersAPI.get({ userId });
        const { profile } = data;

        if (statusText !== "OK") {
          throw new Error(statusText);
        }

        this.id = profile.id;
        this.image = profile.image;
        this.name = profile.name;
        this.email = profile.email;
      } catch (error) {
        Toast.fire({
          icon: "error",
          title: "無法取得使用者資料，請稍後再試"
        });
      }
    },
    handleFileChange(e) {
      const files = e.target.files;
      if (!files.length) return; // 如果沒有檔案則離開此函式
      // 否則產生預覽圖...
      const imageURL = window.URL.createObjectURL(files[0]);
      this.image = imageURL;
    },
    async handleSubmit(e) {
      if (!this.name) {
        Toast.fire({
          icon: "warning",
          title: "您尚未填寫姓名"
        });
        return;
      }
      const form = e.target;
      const formData = new FormData(form);
      try {
        const { data, statusText } = await usersAPI.updateUser({
          userId: this.id,
          formData
        });

        if (statusText !== "OK" || data.status !== "success") {
          throw new Error(statusText);
        }

        this.$router.push({ name: "user", params: { id: this.id } });
      } catch (error) {
        Toast.fire({
          icon: "error",
          title: "無法變更使用者資料，請稍後再試"
        });
      }
    }
  }
};
</script>