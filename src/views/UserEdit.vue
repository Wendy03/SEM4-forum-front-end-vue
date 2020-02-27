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
const dummyData = {
  profile: {
    id: 1,
    name: "root",
    email: "root@example.com",
    image: "https://i.imgur.com/JtQJRMZ.png"
  }
};

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
    fetchUser(userId) {
      console.log("fetchUser id:", userId);
      const { profile } = dummyData;
      this.id = profile.id;
      this.name = profile.name;
      this.email = profile.email;
      this.image = profile.image;
    },
    handleFileChange(e) {
      const files = e.target.files;
      if (!files.length) return; // 如果沒有檔案則離開此函式
      // 否則產生預覽圖...
      const imageURL = window.URL.createObjectURL(files[0]);
      this.image = imageURL;
    },
    handleSubmit(e) {
      const form = e.target;
      const formData = new FormData(form);
      for (let [name, value] of formData.entries()) {
        console.log(name + ": " + value);
      }
    }
  }
};
</script>