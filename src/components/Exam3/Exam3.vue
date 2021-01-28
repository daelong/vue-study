<template>
    <div>
        <div class="table-head-box">
            <table class="table-head">
                <th class="table-head-name">name</th>
                <th class="table-head-type">type</th>
                <th class="table-head-country">country</th>
                <th class="table-head-url">website_url</th>
            </table>
        </div>
        <div class="box" v-for="(item, idx) in items" :key="idx">
            <table class="card" v-if="item.visible"  @click="handleAlert(item)">
                <div class="card-header">
                    <td class="header-name">{{ item.name }}</td>
                    <td class="header-type">{{ item.brewery_type }}</td><br>
                    <td class="header-country">{{ item.country }}</td><br>
                    <td class="header-url"><a :href="item.website_url">{{ item.website_url }}</a></td>
                    <br>
                </div>
            </table>
        </div>
    </div>
</template>

<script>
    export default {
        props: {
            items: [],
        },
        created(){
        },
        computed: {},
        methods: {
            handleAlert(item){
                alert(
                    " name: " + item.name + "\n" +
                 " brewery_type: " + item.brewery_type + "\n" +
                  " country: " + item.country + " state: " + "\n" +
                   item.state + " city: " + item.city + "\n" +
                    " phone: " + item.phone + "\n" +
                     " website_url: " + item.website_url
                     );
            }
        }
    };
</script>

<style scoped>
    div {
        flex: 2;
    }
    .table-head-box{
        width: 100%;
        margin-left: 20px;
    }
    .table-head-box .table-head{
        padding: 10px 0 10px 0;
        width: 90%;
        margin-right: 0;
        display: flex;
        flex-direction: row;
        justify-content: flex-start;
        border-bottom: 1px solid gray;
    }

    .table-head-box .table-head .table-head-name{
        width: 25%;
        padding-left: 10px;
        text-align: left;
    }
    .table-head-box .table-head .table-head-type{
        width: 25%;
        text-align: left;
    }
    .table-head-box .table-head .table-head-country{
        width: 25%;
        text-align: left;
    }
    .table-head-box .table-head .table-head-url{
        width: 25%;
        text-align: left;
    }
    .box {
        width: 100%;
    }

    .box .card {
        margin: 0 0 0 20px;
        padding-top: 20px;
        padding-bottom: 20px;
        width: 90%;
        border-bottom: 1px solid black;
        display: flex;
        flex-direction: row;
        justify-content: flex-start;
        align-items: center;
    }
    .box .card:hover{
        background-color: #c7ecee;
    }

    .card .card-header {
        width: 90%;
        display: flex;
        flex-direction: row;
        justify-content: flex-start;
    }
    .card .card-header .header-name{
        width: 25%;
        padding-left: 10px;
    }
    .card .card-header .header-type{
        width: 25%;
    }
    .card .card-header .header-country{
        width: 25%;
    }
    .card .card-header .header-url{
        width: 25%;
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