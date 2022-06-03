<template>
  <div>
    <!-- <q-list bordered>
      <q-item clickable v-for="news in newsList" :key="news.id">
        <q-item-section class="text-h6">{{ news.title }}</q-item-section>
        <q-item-section side class="text-caption">{{
          fromNow(news.time)
        }}</q-item-section>
      </q-item>
    </q-list> -->
    <q-table
      :rows="newsList"
      row-key="name"
      :filter="filter"
      @row-click="rowClick"
      v-model:pagination="pagination"
    >
      <template v-slot:top-right>
        <q-input
          borderless
          dense
          debounce="300"
          v-model="filter"
          placehoder="Search"
        >
          <template v-slot:append>
            <q-icon name="search"></q-icon>
          </template>
        </q-input>
      </template>
    </q-table>
  </div>
</template>

<script>
import axios from "axios";
const moment = require("moment");
moment.locale("ko-KR");

export default {
  data() {
    return {
      newsList: [],
      filter: "",
      pagination: {
        rowsPerPage: 10,
      },
    };
  },
  async created() {
    const news = await axios.get("https://api.hnpwa.com/v0/news/1.json");
    this.newsList = news.data;
    this.$q.notify({ message: "뉴스 로드 완료" });
  },
  methods: {
    fromNow(timestamp) {
      return moment(timestamp).fromNow();
    },
    rowClick(event, row, index) {
      window.open(this.newsList[index].url);
    },
  },
};
</script>
