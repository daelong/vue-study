<template>
    <div>Menu component</div>
</template>

<script>
export default {
    
}
</script>

<style scoped>
div{
    flex:1
}
</style>

<template>
    <div>
        <ul>
            <li v-for="(item, idx) in items" :key="idx">
                <div class="card" v-if="item.visible">
                    <div class="card-header">
                        <br><div class="header-name">{{ item.name }}</div><br>
                        <button class="button" @click="item.visible = !item.visible">닫기</button>
                    </div>
                    <span>{{ item.brewery_type }}</span><br>
                    <span>{{ item.country }}</span><br>
                    <a :href="item.website_url">{{ item.website_url }}</a><br>
                </div>
                <div class="folded-card" v-if="!item.visible">
                    <div class="card-header">
                        <br><div class="header-name">{{ item.name }}</div><br>
                        <button class="button" @click="item.visible = !item.visible">펼치기</button>
                    </div><br>
                </div>
            </li>
        </ul>
    </div>
</template>
    
<script>
export default {
    props:{
        items: [],
    },
    computed: {
        getfilter() {
            return this.items.filter(items => items.visible)
        }
    },
    methods: {
    }
};
</script>

<style scoped>
div{
    flex:2; 
}
ul{ 
    width: 100%;
    padding-top: 20px;
    list-style: none;
}
.card{
    margin: 0 0 20px 20px;
    padding-top: 20px;
    width: 45%;
    border: 1px solid black;
    border-radius: 8px;
    display: flex;
    flex-direction: column;
    align-items: center;
}
.card .card-header{
    width: 90%;
    display: flex;
    flex-direction: row;
    justify-content: space-around;
    }
.folded-card{
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
.folded-card .card-header{
    width: 90%;
    display: flex;
    flex-direction: row;
    justify-content: space-around;
}
.card .card-header .header-name{
}
.card .card-header .button{
    width: 10%;
    height: 100%;
}
</style>