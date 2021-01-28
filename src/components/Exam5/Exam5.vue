<template>
    <div id="Ex5Main" class="main">
         <div class="table-head-box">
            <table class="table-head">
                <th class="table-head-name" @click="handleSort(section['name'])" :class="{ up : orderBy === 'up' && sortBy ==='name', down: orderBy ==='down'&& sortBy ==='name' }">name</th>
                <th class="table-head-type" @click="handleSort(section['type'])" :class="{ up: orderBy === 'up' && sortBy ==='brewery_type', down: orderBy ==='down' && sortBy ==='brewery_type'}">type</th>
                <th class="table-head-country" @click="handleSort(section['country'])" :class="{ up: orderBy === 'up'&& sortBy ==='country', down: orderBy ==='down' && sortBy ==='country'}">country</th>
                <th class="table-head-url" @click="handleSort(section['website_url'])" :class="{ up: orderBy === 'up'&& sortBy ==='website_url', down: orderBy ==='down' && sortBy ==='website_url'}">website_url</th>
            </table>
        </div>
        <div class="box" v-for="(item, idx) in sortData" :key="idx">
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
        <Exam5Modal :modalItem="selectedItem" :modalVisible="visible" @modalVisible="handleModalVisible"/>
    </div>
</template>

<script>
import axios from 'axios';
import Exam5Modal from './Exam5Modal';

export default {
    name:"Ex5Main",
    components:{
        Exam5Modal
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
           sortBy: null,
           orderBy: null,
           section:{
               name: 'name',
               type: 'brewery_type',
               country: 'country',
               website_url: 'website_url'
           },
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
        sortData(){
            // const type = this.sortBy;
            // const order = this.orderBy;
            return this.items.slice().sort((a, b) => {
                console.log(this.sortBy);
                let nameA = a[this.sortBy];
                let nameB = b[this.sortBy];
                if(this.orderBy === 'up'){
                    if (nameA < nameB) {
                    return -1;
                    }
                    if (nameA > nameB) {
                      return 1;
                    }            
                    return 0;
                }else if(this.orderBy === 'down'){
                    if (nameA < nameB) {
                    return 1;
                    }
                    if (nameA > nameB) {
                      return -1;
                    }            
                    return 0;
                }else{
                    return 0;
                }
            });
        }
    },
    methods: {
        handleSort(section){
            this.sortBy = section;
            if(this.orderBy === null){
                this.orderBy = 'up';
            }else if(this.orderBy === 'up'){
                this.orderBy = 'down';
            }else{
                this.orderBy = null;
            };
            this.sortData;
        },
        handleModalItem(item){
        this.selectedItem = item;
        this.visible = !this.visible;
        },
        handleModalVisible(visible){
            this.visible = visible;
        },
        handlePage(i){
        this.count = 0;
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