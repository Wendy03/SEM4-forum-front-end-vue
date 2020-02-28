<template>
  <div class="container py-5">
    <NavTabs />
    <!-- 餐廳類別標籤 RestaurantsNavPills -->
    <RestaurantsNavPills :categories="categories" />

    <div class="row">
      <!-- 餐廳卡片 RestaurantCard-->
      <RestaurantCard
        v-for="restaurant in restaurants"
        :key="restaurant.id"
        :initial-restaurant="restaurant"
      />
    </div>

    <!-- 分頁標籤 RestaurantPagination -->
    <RestaurantsPagination
      v-if="totalPage > 1"
      :category-id="categoryId"
      :current-page="currentPage"
      :total-page="totalPage"
    />
  </div>
</template>

<script>
import NavTabs from "../components/NavTabs";
import RestaurantCard from "../components/RestaurantCard";
import RestaurantsNavPills from "../components/RestaurantsNavPills";
import RestaurantsPagination from "../components/RestaurantsPagination";
// STEP 1：透過 import 匯入剛剛撰寫好用來呼叫 API 的方法
import restaurantsAPI from "./../apis/restaurants";
import { Toast } from "./../utils/helpers";

export default {
  components: {
    NavTabs,
    RestaurantCard,
    RestaurantsNavPills,
    RestaurantsPagination
  },
  data() {
    return {
      categories: [],
      categoryId: -1,
      currentPage: 1,
      restaurants: [],
      totalPage: -1
    };
  },
  created() {
    // STEP 2：在 created 的時候呼叫 fetchRestaurants 方法
    // 這裡會向伺服器請求第一頁且不分餐廳類別的資料
    const { page = 1, categoryId = "" } = this.$route.query;
    this.fetchRestaurants({
      page,
      categoryId
    });
  },
  // 使用 beforeRouteUpdate 方法取得使用者路由變化
  beforeRouteUpdate(to, from, next) {
    const { page = 1, categoryId = "" } = to.query;
    this.fetchRestaurants({ page, categoryId });
    next();
  },
  methods: {
    // STEP 1：將 fetchRestaurants 改成 async...await 的語法
    // 並且可以帶入參數 page 與 categoryId
    // 呼叫 API 後取得 response
    async fetchRestaurants({ page, categoryId }) {
      try {
        const response = await restaurantsAPI.getRestaurants({
          page,
          categoryId
        });
        // STEP 3：將 response 中的 data 和 statusText 取出
        const { data, statusText } = response;
        // STEP 4s：如果 statusText 不是 OK 的話則進入錯誤處理
        if (statusText !== "OK") {
          throw new Error(statusText);
        }
        // STEP 4：將從伺服器取得的 data 帶入 Vue 內
        this.categories = data.categories;
        this.categoryId = data.categoryId;
        this.currentPage = data.page;
        this.restaurants = data.restaurants;
        this.totalPage = data.totalPage.length;
      } catch (error) {
        Toast.fire({
          type: "error",
          title: "無法取得餐廳資料，請稍後再試",
          icon: "error"
        });
      }
    }
  }
};
</script>