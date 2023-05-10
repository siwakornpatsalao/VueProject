<template>
    <div>
        <v-row>
            <v-col>
                <img :src="datas[id-1].thumbnail" height="250px">
                <h1>{{ datas[id-1].title}}</h1>
                <h1>Genre: {{ datas[id-1].genre }}</h1>
                <h2>{{ datas[id-1].release_date }}</h2>
                <h2>Description: <br>{{ datas[id-1].short_description }}</h2>
            </v-col>
        </v-row>
    </div>
</template>

<script>
export default {
    data(){
        return{
            datas: [],
            id: '',
        }
    },
    created(){
        this.id = this.$route.params.id
    },
    async asyncData({ $axios }) {
    const { data } = await $axios.get("https://www.mmobomb.com/api1/games");
    console.log(data);
    data.sort((a, b) => a.id - b.id);
    return { datas: data };
  },
    /* async mounted(){
        this.id = this.$route.params.id;
        const {data} = await this.$axios.get("https://www.mmobomb.com/api1/games")
        this.datas = data.filter(key=>
            key.id===Number(this.id));
    } */
}
</script>
