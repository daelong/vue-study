<template>
    <div>
     <div class="box" v-for="(item, idx) in items" :key="idx">
          <div class="card" v-if="item.visible">
                 <div class="card-header">
                     <br>
                     <div class="header-name">{{ item.name }}</div><br>
                     <button class="button" @click="item.visible = !item.visible">닫기</button>
                 </div>
                 <span>{{ item.brewery_type }}</span><br>
                 <span>{{ item.country }}</span><br>
                 <a :href="item.website_url">{{ item.website_url }}</a><br>
             </div>
             <div class="folded-card" v-if="!item.visible">
                 <div class="card-header">
                     <br>
                     <div class="header-name">{{ item.name }}</div><br>
                     <button class="button" @click="item.visible = !item.visible">펼치기</button>
                 </div><br>
             </div>
        </div>
    </div>
</template>
// name, brewery_type, country, website_url //  state, city, phone
<script>
export default {
    props: {
        items: [],
    },
    created(){
    },
    computed: {},
    methods: {
        }
}
</script>

<style scoped>
    div {
        flex: 2;
    }
    .box {
        width: 100%;
    }

    .box .card {
        margin: 0 0 20px 20px;
        padding-top: 20px;
        padding-bottom: 20px;
        width: 45%;
        border: 1px solid black;
        border-radius: 8px;
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        align-items: center;
    }
    .card .card-header {
        width: 90%;
        display: flex;
        flex-direction: row;
    }
    .card .card-header .header-button{
        width: 5%;
        height: 10%;
        margin-right: 10px;
    }
    .folded-card {
        margin: 0 0 20px 20px;
        padding-top: 20px;
        width: 45%;
        border: 1px solid black;
        border-radius: 8px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
    }

    .folded-card .card-header {
        width: 90%;
        display: flex;
        flex-direction: row;
        justify-content: space-around;
    }
</style>