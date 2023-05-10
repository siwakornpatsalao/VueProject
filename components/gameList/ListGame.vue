<template>
    <div>
      <div style="display: flex; flex-wrap: wrap; justify-content: center">
        <v-icon>mdi-search-web</v-icon>
        <v-text-field v-model="search" label="Search Game Title"></v-text-field>
      </div>
  
      <div>
        Sort By
        <v-btn @click="sortByAZ">Title (A-Z)</v-btn>
        <v-btn @click="sortByDate">Release Date</v-btn>
      </div>
  
      <div class="container">
        <v-row>
          <!--<pre>{{datas}}</pre>-->
            <v-col v-for="data in filterList" :key="data.id">
              <v-card @click="goTo(data)">
                <img :src="data.thumbnail" height="200" />
                <v-col><h3>{{ data.id }}</h3></v-col>
                <v-col><h3>{{ data.title }}</h3></v-col>
                <v-col><h3>Genre: {{ data.genre }}</h3></v-col>
                <v-col><h3>Release Date: {{ data.release_date }}</h3></v-col>
              </v-card>
            </v-col>
        </v-row>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        datas: [],
        search: "",
      };
    },
    /* async asyncData({ $axios }) {
      const { data } = await $axios.get("https://www.mmobomb.com/api1/games");
      console.log(data);
      data.sort((a, b) => a.id - b.id);
      return { datas: data };
    }, */
    computed: {
      filterList() {
        if (this.search == "") {
          return this.datas;
        } else {
          return this.datas.filter((data) => {
            return data.title
              .toLowerCase()
              .includes(this.search.toLocaleLowerCase());
          });
        }
      },
    },
    methods: {
      sortByAZ() {
        return this.datas.sort((a, b) => {
          if (a.title < b.title) return -1;
          if (a.title > b.title) return 1;
          return 0;
        });
      },
      sortByDate() {
        return this.datas.sort((a, b) => {
          return new Date(a.release_date) - new Date(b.release_date);
        });
      },
      goTo(data){
        this.$router.push(`/${data.id}`);
      }
    },
    head() {
      return {
        title: "Game List",
      };
    },
    async fetch() {
      const { data } = await this.$axios.get("https://www.mmobomb.com/api1/games",{ mode: "no-cors" })
      //data.sort((a, b) => a.id - b.id);
      console.log(data)
      this.datas = data
    },
  };
  </script>
  