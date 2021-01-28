<template>
    <div id="item" class="main">
        <div class="table-head-box">
            <table class="table-head">
                <th class="table-head-name">name</th>
                <th class="table-head-type">type</th>
                <th class="table-head-country">country</th>
                <th class="table-head-url">website_url</th>
            </table>
        </div>
        <div class="box" v-for="(item, idx) in items" :key="idx">
            <tr class="table" :class="{ odd: idx % 2 === 0}" @click="item.visible = !item.visible">
                <div class="table-body-header">
                    <td class="header-idx" >{{idx + 1}}</td>
                    <td class="header-name">{{ item.name }}</td>
                    <td class="header-type">{{ item.brewery_type }}</td><br>
                    <td class="header-country">{{ item.country }}</td><br>
                    <td class="header-url"><a :href="item.website_url">{{ item.website_url }}</a></td>
                    <br>
                </div>
            </tr>
            <div class="modal-box" v-if="!item.visible">
                <div class="modal">
                    <div class="modal-top">
                        <div class="modal-top-logo">Modal</div>
                        <div class="modal-top-btn-box"><button class="modal-top-button" @click="item.visible = !item.visible">X</button></div>
                    </div><br>
                    <div class="modal-content">name: {{ item.name }}</div>
                    <div class="modal-content">type: {{item.brewery_type}}</div>
                    <div class="modal-content">state: {{item.state}}</div>
                    <div class="modal-content">city: {{item.city}}</div>
                    <div class="modal-content">phone: {{item.phone}}</div>
                    <div></div>
                </div>
            </div>
        </div>
         <div class="pagination">
            <button class="pagination-button" @click="handlePageDown()">&#60; </button>
            <div class="pagination-btn" v-for="i in 10" :key="{i}" @click="handlePage(i)">
                <button class="pagination-button current-page" v-if="pageNum === i">{{i}}</button>
                <button class="pagination-button" v-else>{{i}}</button>
            </div>
            <button class="pagination-button" @click="handlePageUp()"> &#62;</button>
        </div>
    </div>
</template>

<script>
import Modal from './Modal';

    export default {
        name:"item",
        components:{
            Modal
        },
        props: {
            items: [],
            pageNum: Number,
        },
        created(){
        },
        computed: {},
        methods: {
            handlePage(page){
              this.pageNum = page;
              this.fetchMoreItems();
            },
            handlePageDown(){
                if(this.pageNum === 1){
                    this.pageNum = 11;
                }
                this.pageNum -= 1;
                this.fetchMoreItems();
            },
            handlePageUp(){
                 if(this.pageNum === 10){
                    this.pageNum = 0;
                }
                this.pageNum += 1;
                this.fetchMoreItems();
            },
            fetchMoreItems (){
              this.fetching = true;
              const fetchData = this.$axios.get("https://api.openbrewerydb.org/breweries?page="+this.pageNum+"&per_page=25  ");
              fetchData
                .then(res => res.data)
                .then(data => data.map(res => {
                  return {
                    ...res,
                    visible: true
                  }
                }))
                .then(res => { 
                  this.items = res;
                   })
                .catch(error => console.log(error));
                this.fetching = false;
            },
        }
    };
</script>

<style scoped>
    .main{
        width: 100%;
        display: flex;
        flex-direction: column;
        justify-items:  center;
        align-items: center;
    }
    div {
        flex: 2;
    }
    .odd{
        background-color: #bdc3c7;
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
        padding-left: 25px;
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

    .box .table {
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
    .box .table:hover{
        background-color: #c7ecee;
    }

    .table .table-body-header {
        width: 90%;
        display: flex;
        flex-direction: row;
        justify-content: flex-start;
    }
    .table .table-body-header .header-idx{
        padding-left: 5px;
        margin-right: 3px;
    }
    
    .table .table-body-header .header-name{
        width: 25%;
        padding-left: 10px;
    }
    .table .table-body-header .header-type{
        width: 25%;
    }
    .table .table-body-header .header-country{
        width: 25%;
    }
    .table .table-body-header .header-url{
        width: 25%;
    }
    .modal-box{
        position: fixed;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        z-index: 30;
        background: rgba(0, 0, 0, 0.25);
        /* width: 100%;
        height: 100%; */
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .modal-box .modal {
        padding-top: 15px;
        position: fixed;
        z-index: 10;
        width: 35%;
        height: 25%;
        background-color: white;
        border: 1px solid black;
        border-radius: 2px;
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        align-items: flex-start;
    }
    .modal-box .modal .modal-top{
        padding-left: 10px;
        width: 98%;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        border-bottom: 1px solid  rgba(0, 0, 0, 0.25);
    }
    .modal-box .modal .modal-top-btn-box{
        margin-left: 80%;
        padding-right: 6px;
    }
    .modal-box .modal .modal-top-button{
        height: 20px;
        border: none;
        cursor: pointer;
        background: white;
    }
    .modal-box .modal .modal-top-button:hover{
        background: #c7ecee;
    }
    .modal-top-logo{
        width: 50%; 
    }
    .modal-box .modal .modal-header {
        height: 10%;    
        display: flex;
        flex-direction: row;
        justify-content: space-around;
    }
    .modal-box .modal .modal-content{
        padding-left: 10px; 
    }
    .pagination{
        width: 20%;
        padding-top: 10px;
        padding-bottom: 10px;
        display: flex;
        flex-direction: row;
        align-content: center;
    }

    .pagination .pagination-btn{
        display: flex;
        flex-direction: row;
        justify-items: center;
    }
    .pagination-button{
        font-size: 20px;
        border: none;
        background-color: white;
        cursor: pointer;
    }
    .pagination-button:hover{
        background-color: #c7ecee;
    }
    .current-page{
        color: #e55039;
    }
</style>