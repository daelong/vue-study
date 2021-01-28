<template>
    <div id="Ex4Main" class="main">
         <div class="table-head-box">
            <table class="table-head">
                <th class="table-head-name">name</th>
                <th class="table-head-type">type</th>
                <th class="table-head-country">country</th>
                <th class="table-head-url">website_url</th>
            </table>
        </div>
         <div class="box" v-for="(item, idx) in items" :key="idx">
            <tr class="table" :class="{ odd: idx % 2 === 0}" @click="handleModalItem(item)">
                <div class="table-body-header">
                    <td class="header-idx" >{{idx + 1}}</td>
                    <td class="header-name">{{ item.name }}</td>
                    <td class="header-type">{{ item.brewery_type }}</td><br>
                    <td class="header-country">{{ item.country }}</td><br>
                    <td class="header-url"><a :href="item.website_url">{{ item.website_url }}</a></td>
                    <br>
                </div>
            </tr>
        </div>
         <div class="pagination">
            <button class="pagination-button" @click="handlePageDown()">&#60; </button>
            <div class="pagination-btn" v-for="i in 10" :key="i" @click="handlePage(i)">
                <button class="pagination-button currentPage" v-if="pageNum === i">{{i}}</button>
                <button class="pagination-button" v-else>{{i}}</button>
            </div>
            <button class="pagination-button" @click="handlePageUp()"> &#62;</button>
        </div>
        <Exam4Modal :modalItem="selectedItem" :modalVisible="visible" @modalVisible="handleModalVisible"/>
    </div>
</template>

<script>
import axios from 'axios';
import Exam4Modal from './Exam4Modal';

    export default {
        name:"Ex4Main",
        components:{    
            Exam4Modal
        },
        props: {
        },
        created(){
        },
        data() {
        return {
           items: [],
           selectedItem: {},
           visible: true,
           pageNum: 1,
           count: 0,
        }
        },
        mounted() {
          axios.get("https://api.openbrewerydb.org/breweries?page=" + this.pageNum + "&per_page=15")
          .then(res => res.data)
          .then(data => data.map( data => {
            return {
              ...data, 
              visible: true
              }
            }))
            .then(data => this.items = data)
            .catch(error => console.log(error));
        },
        computed: {
        },
        methods: {
            handleModalItem(item){
            this.selectedItem = item;
            this.visible = !this.visible;
        },
            handleModalVisible(visible){
                this.visible = visible;
            },
            handlePage(i){
              this.pageNum = i;
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
                axios.get("https://api.openbrewerydb.org/breweries?page="+this.pageNum+"&per_page=15")
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
    .main .box{
        margin-left: 0;
        width: 100%;
    }
    .up:after{
        content:" \2191";
    }
    .down:after{
        content:" \2193";
    }
    .odd{
        background-color: #bdc3c7;
    }
    .box .table {
        margin: 0;
        padding-top: 20px;
        padding-bottom: 20px;
        width: 100%;
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
        width: 100%;
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
    .table .table-body-header .header-type, .header-country, .header-url
    /* .table .table-body-header .header-country,
    .table .table-body-header .header-url */
    {
        width: 25%;
    }
    .table-head-box{
        width: 100%;
    }
    .table-head-box .table-head{
        padding: 10px 0 10px 0;
        width: 100%;
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
    .table-head-box .table-head .table-head-type, .table-head-country, .table-head-url{
        width: 25%;
        text-align: left;
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
    .currentPage{
        color: #e55039;
    }
</style>