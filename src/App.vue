<template>
  <div class="container text-center pt-3">
    <h1>Foodpanda 餐廳查詢</h1>
    <button type="button" class="btn btn-primary" @click.prevent="searchbtn">查詢餐廳</button>
    <hr>
  </div>
  <div class="container" v-if="storeCount != 0">
    <h2>餐廳列表</h2>
    <h5>共 {{ storeCount }} 間餐廳</h5>
    <p class="text-end">資料更新時間：{{ dateTime }}</p>
    <div class="row">
      <div class="col-md-4 g-0" v-for="(store, index) in allStores" :key="store.id">
        <div class="card p-2 m-2">
          <img :src="store.hero_listing_image" class="card-img-top" alt="...">
          <div class="card-body">
            <h5 class="card-title">{{ index+1 }} {{ store.name }}</h5>
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
      aipUrl: 'https://disco.deliveryhero.io/listing/api/v1/pandora/vendors?latitude=24.956594&longitude=121.4737595&language_id=6&country=tw&vertical=restaurants&configuration=Original&limit=48&offset=0&customer_type=regular',
      dateTime: '',
    };
  },
  methods: {
      searchbtn() {
        this.updateDataTime();

        axios.get(this.aipUrl, {
          headers: {
            'x-disco-client-id' : 'web',
          }
        })
        .then( res => {
          this.allStores = res.data.data.items;
          this.storeCount = res.data.data.available_count;
        })
        .catch( err => {
          console.log(err);
        })
      },
      updateDataTime() {
        const now = new Date();
        const year = now.getFullYear();
        const month = String(now.getMonth() + 1).padStart(2, '0');
        const day = String(now.getDate()).padStart(2, '0');
        const hour = String(now.getHours()).padStart(2, '0');
        const minute = String(now.getMinutes()).padStart(2, '0');
        const second = String(now.getSeconds()).padStart(2, '0');

        this.dateTime = `${year}-${month}-${day} ${hour}:${minute}:${second}`;
      }
    }
}

</script>