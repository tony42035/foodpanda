<template>
  <div class="container text-center pt-3">
    <h1>Foodpanda 餐廳查詢</h1>
    <button type="button" class="btn btn-primary" @click.prevent="searchbtn">查詢餐廳</button>
    <hr>
  </div>
  <div class="container" v-if="storeCount != 0">
    <h2>餐廳列表</h2>
    <h5>共 {{ storeCount }} 間餐廳</h5>
    <div class="row">
      <div class="col-md-4 g-0" v-for="store in allStores" :key="store.id">
        <div class="card p-2 m-2">
          <img :src="store.hero_listing_image" class="card-img-top" alt="...">
          <div class="card-body">
            <h5 class="card-title">{{ store.name }}</h5>
            <a :href="store.web_path" class="btn btn-primary">前往餐廳</a>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      allStores: {},
      storeCount: 0,
    };
  },
  methods: {
      searchbtn() {
        axios.post('https://disco.deliveryhero.io/search/api/v1/feed', {
              "q": "",
    "location": {
        "point": {
            "latitude": 24.956594,
            "longitude": 121.4737595
        }
    },
    "config": "Variant21",
    "customer_id": "",
    "vertical_types": [
        "restaurants"
    ],
    "include_component_types": [
        "vendors"
    ],
    "include_fields": [
        "feed"
    ],
    "language_id": "6",
    "opening_type": "delivery",
    "platform": "web",
    "session_id": "",
    "language_code": "zh",
    "customer_type": "regular",
    "limit": 48,
    "offset": 0,
    "dps_session_id": "eyJzZXNzaX2lk......MjM1zOTl9",
    "dynamic_pricing": 0,
    "brand": "foodpanda",
    "country_code": "tw",
    "use_free_delivery_label": false
        })
        .then( res => {
          this.allStores = res.data.feed.items[0].items;
          this.storeCount = res.data.feed.count;
          console.log(this.allStores);
        })
      }
    }
}

</script>