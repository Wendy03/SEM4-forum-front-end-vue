<template>
  <div class="container py-5">
    <NavTabs />
    <h1 class="mt-5">人氣餐廳</h1>

    <hr />
    <div
      v-for="restaurant in restaurants"
      :key="restaurant.id"
      class="card mb-3"
      style="max-width: 540px;margin: auto;"
    >
      <div class="row no-gutters">
        <div class="col-md-4">
          <router-link :to="{name: 'restaurant', params: {id: restaurant.id }}">
            <img class="card-img" :src="restaurant.image" />
          </router-link>
        </div>
        <div class="col-md-8">
          <div class="card-body">
            <h5 class="card-title">{{ restaurant.name }}</h5>
            <span class="badge badge-secondary">收藏數：{{ restaurant.FavoritedUsers.length }}</span>
            <p class="card-text">{{ restaurant.description }}</p>
            <router-link
              class="btn btn-primary mr-2"
              :to="{name: 'restaurant', params: {id: restaurant.id }}"
            >Show</router-link>

            <button
              v-if="restaurant.isFavorited"
              type="button"
              class="btn btn-danger mr-2"
              @click.stop.prevent="removeFavorite(restaurant.id)"
            >移除最愛</button>
            <button
              v-else
              type="button"
              class="btn btn-primary"
              @click.stop.prevent="addFavorite(restaurant.id)"
            >加到最愛</button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import NavTabs from "../components/NavTabs";

const dummyData = {
  restaurants: [
    {
      id: 51,
      name: "Jolie Hackett",
      tel: "542.600.9620",
      address: "2439 Tiffany Heights",
      opening_hours: "08:00",
      description: "magni ut voluptas",
      image:
        "https://loremflickr.com/320/240/restaurant,food/?random=27.32287991372693",
      createdAt: "2019-11-20T06:25:42.934Z",
      updatedAt: "2020-01-21T12:34:18.056Z",
      CategoryId: 3,
      FavoritedUsers: [],
      isFavorited: false,
      FavoriteCount: 0
    },
    {
      id: 14,
      name: "Rahul Kilback",
      tel: "1-573-027-3190",
      address: "13538 Lula Roads",
      opening_hours: "08:00",
      description: "Ut ea repellat cumque illo sint. Possimus a magni",
      image:
        "https://loremflickr.com/320/240/restaurant,food/?random=66.19683181847653",
      createdAt: "2019-11-20T06:25:42.925Z",
      updatedAt: "2020-02-16T05:36:07.120Z",
      CategoryId: 7,
      FavoritedUsers: [],
      isFavorited: false,
      FavoriteCount: 0
    },
    {
      id: 1,
      name: "123",
      tel: "02-8888-0000",
      address: "01313 ketlch deive",
      opening_hours: "08:00",
      description: "dsfsafdsadfasasdfasdf",
      image:
        "https://loremflickr.com/320/240/restaurant,food/?random=22.148585495422335",
      createdAt: "2019-11-20T06:25:42.921Z",
      updatedAt: "2020-02-26T10:22:26.517Z",
      CategoryId: 3,
      FavoritedUsers: [],
      isFavorited: false,
      FavoriteCount: 0
    },
    {
      id: 6,
      name: "Liliane Dibbert",
      tel: "1-827-478-9971 x690",
      address: "2762 Devon Run",
      opening_hours: "08:00",
      description: "facilis",
      image:
        "https://loremflickr.com/320/240/restaurant,food/?random=98.6709769179636",
      createdAt: "2019-11-20T06:25:42.923Z",
      updatedAt: "2020-01-08T14:12:46.473Z",
      CategoryId: 7,
      FavoritedUsers: [],
      isFavorited: false,
      FavoriteCount: 0
    },
    {
      id: 99,
      name: "cool",
      tel: "457291867",
      address: "26 Bluegrass Street",
      opening_hours: "13:02",
      description: "coolcoolcool",
      image: "https://i.imgur.com/OC8nekt.png",
      createdAt: "2019-12-09T02:50:06.794Z",
      updatedAt: "2019-12-09T02:50:06.794Z",
      CategoryId: 3,
      FavoritedUsers: [],
      isFavorited: false,
      FavoriteCount: 0
    },
    {
      id: 7,
      name: "Kari Sporer",
      tel: "584-434-6976",
      address: "941 Jakubowski Stream",
      opening_hours: "08:00",
      description: "sunt dolorem voluptate",
      image:
        "https://loremflickr.com/320/240/restaurant,food/?random=80.77439621389972",
      createdAt: "2019-11-20T06:25:42.923Z",
      updatedAt: "2019-11-23T14:30:22.808Z",
      CategoryId: 4,
      FavoritedUsers: [],
      isFavorited: false,
      FavoriteCount: 0
    },
    {
      id: 26,
      name: "Dayna Roberts",
      tel: "759-389-2695",
      address: "9850 Kaia Circle",
      opening_hours: "08:00",
      description: "ipsum",
      image:
        "https://loremflickr.com/320/240/restaurant,food/?random=3.7178880115385837",
      createdAt: "2019-11-20T06:25:42.928Z",
      updatedAt: "2019-11-20T06:25:42.928Z",
      CategoryId: 3,
      FavoritedUsers: [],
      isFavorited: false,
      FavoriteCount: 0
    },
    {
      id: 5,
      name: "Abagail Dietrich",
      tel: "638.352.6637 x5805",
      address: "011 Glenna Camp",
      opening_hours: "08:00",
      description: "Cumque quasi nihil. Dicta accusantium officiis ass",
      image:
        "https://loremflickr.com/320/240/restaurant,food/?random=60.98596097768254",
      createdAt: "2019-11-20T06:25:42.923Z",
      updatedAt: "2019-11-23T14:18:04.644Z",
      CategoryId: 3,
      FavoritedUsers: [],
      isFavorited: false,
      FavoriteCount: 0
    },
    {
      id: 25,
      name: "Webster Dietrich",
      tel: "1-653-478-8294 x018",
      address: "229 Predovic Lodge",
      opening_hours: "08:00",
      description: "Qui consequuntur est quo.",
      image:
        "https://loremflickr.com/320/240/restaurant,food/?random=37.893799230224204",
      createdAt: "2019-11-20T06:25:42.928Z",
      updatedAt: "2020-01-18T11:44:28.915Z",
      CategoryId: 4,
      FavoritedUsers: [],
      isFavorited: false,
      FavoriteCount: 0
    }
  ]
};

export default {
  name: "RestaurantsTop",
  components: {
    NavTabs
  },
  data() {
    return {
      restaurants: []
    };
  },
  created() {
    this.fetchTopRestaurants();
  },
  methods: {
    fetchTopRestaurants() {
      this.restaurants = dummyData.restaurants;
    },
    addFavorite(restaurantId) {
      this.restaurants = this.restaurants.map(restaurant => {
        if (restaurant.id !== restaurantId) {
          return restaurant;
        }
        return {
          ...restaurant,
          isFavorited: true
        };
      });
    },
    removeFavorite(restaurantId) {
      this.restaurants = this.restaurants.map(restaurant => {
        if (restaurant.id !== restaurantId) {
          return restaurant;
        }
        return {
          ...restaurant,
          isFavorited: false
        };
      });
    }
  }
};
</script>