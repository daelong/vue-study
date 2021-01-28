<template>
    <div class="modal-box" v-if="!modalVisible">
        <div class="item-modal" v-if="type === 'item'">
            <div class="modal-top">
                <div class="modal-top-logo">Modal</div>
                <div class="modal-top-btn-box"><button class="modal-top-button" @click="handleVisible()">X</button></div>
            </div><br>
            <div class="modal-content">name: {{ modalItem.name }}</div>
            <div class="modal-content">type: {{modalItem.brewery_type}}</div>
            <div class="modal-content">state: {{modalItem.state}}</div>
            <div class="modal-content">city: {{modalItem.city}}</div>
            <div class="modal-content">phone: {{modalItem.phone}}</div>
        </div>
        <div class="add-modal" v-else>
            <div class="modal-top">
                <div class="modal-top-logo">Modal</div>
                <div class="modal-top-btn-box"><button class="modal-top-button" @click="handleVisible()">X</button></div>
            </div>
            <div class="content-box" action="http://localhost:8080/">
                <div class="modal-content">
                    <div class="modal-content-tags">name:</div>
                        <input class="modal-content-tag" type="text" name="addName" id="name" placeholder="name"><br>
                    </div>
                    <span class="errorMsg">{{ error.name }}</span>
                <div class="modal-content">
                    <div class="modal-content-tags">brewery_type:</div>
                    <select class="modal-content-tag selector" name="addType" id="type">
                        <option value="">--choose an options--</option>
                        <option value="micro">micro</option>
                        <option value="contract">contract</option>
                        <option value="brewpub">brewpub</option>
                        <option value="regional">regional</option>
                        <option value="planning">planning</option>
                        <option value="proprietor">proprietor</option>
                    </select><br>
                </div>
                <span class="errorMsg">{{ error.type }}</span>
                <div class="modal-content">
                    <div class="modal-content-tags">country:</div>
                      <select class="modal-content-tag selector" name="addCountry" id="country">
                        <option value="">--choose an options--</option>
                        <option :value="country" v-for="(country, idx) in countryItem" :key="idx">{{country}}</option>
                    </select>
                </div>
                <div class="modal-content"><div class="modal-content-tags">state:</div><input class="modal-content-tag" type="text" name="addState" id="state" placeholder="state"/></div>
                <div class="modal-content"><div class="modal-content-tags">city:</div><input class="modal-content-tag" type="text" name="addCity" id="city" placeholder="city"/></div>
                <div class="modal-content"><div class="modal-content-tags">phone:</div>
                <input class="modal-content-tag" type="tel" name="addPhone" id="phone" placeholder="phone"/>
                </div>
                <span class="errorMsg">{{ error.phone }}</span>
                <div class="modal-content"><div class="modal-content-tags">website_url:</div><input class="modal-content-tag" type="url" name="addUrl" id="url" placeholder="website_url"/></div>
                <div class="modal-content-tag-btn">
                    <button @click="sendAddItem()">제출</button>
                </div>  
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios';


export default {
    props:{
        modalItem: {},
        modalVisible: Boolean,
        type: "",
        inputData: {},
        countryItem: {},
    },
    data() {
       return{
           getAddItem: {},
           error:{
               name: '',
               type: '',
               phone: '',
            },
       }
    },
    created(){
    },
    computed:{
        getCountry(){
            return axios.get("https://gist.githubusercontent.com/keeguon/2310008/raw/bdc2ce1c1e3f28f9cab5b4393c7549f38361be4e/countries.json")
            .then(res => eval(res.data))
            .then(res => res.map(data => data.name));
        },
    },
    methods:{
        handleVisible(){
            this.newModalItem = {};
            this.newModalVisible = !this.modalVisible;
            this.$emit('modalVisible', this.newModalVisible, this.newModalItem);
        },
       sendAddItem(){
            const name = document.getElementById('name').value;
            const brewery_type = document.getElementById('type').value;
            if(name && brewery_type){
                this.error.name = '';
                this.error.type = '';
                const country = document.getElementById('country').value;
                const state = document.getElementById('state').value;
                const city = document.getElementById('city').value;
                const phone = document.getElementById('phone').value;
                if(phone.length !== 11){
                    if((isNaN(parseInt(phone)))){
                        this.error.phone = '* 핸드폰번호를 제대로 입력해주세요';
                        return 0;
                    }else{
                        this.error.phone = '* 핸드폰번호를 제대로 입력해주세요';
                        return 0;
                    }
                }else {
                    if((isNaN(parseInt(phone)))){
                        this.error.phone = '* 핸드폰번호를 제대로 입력해주세요';
                        return 0;
                    }else{
                        this.error.phone = '';
                    }
                }
                const website_url = document.getElementById('url').value;
                this.getAddItem = {
                    name: name,
                    brewery_type: brewery_type,
                    country: country,
                    state: state,
                    city: city,
                    phone: phone,
                    website_url: website_url
                }
                this.$emit('addItem', this.getAddItem);
                this.handleVisible();
            }else if(name){
                this.error.type = '* brewery_type을 입력해주세요';
                this.error.name = '';
            }else if(type){
                this.error.name = '* 이름을 입력해주세요';
                this.error.type = '';
            }else{
                this.error.name = '* 이름을 입력해주세요';
                this.error.type = '* brewery_type을 입력해주세요';
            }
            
        }
    }
}
</script>

<style scoped>
.modal-box{
        position: fixed;
        top: 0;
        bottom: 0;
        left: 0;
        right: 0;
        z-index: 30;
        background: rgba(0, 0, 0, 0.25);
        display: flex;
        justify-content: center;
        align-items: center;
    }
    .modal-box .item-modal
    {
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
    .modal-box .modal-top{
        padding-left: 10px;
        width: 98%;
        display: flex;
        flex-direction: row;
        justify-content: space-between;
        border-bottom: 1px solid  rgba(0, 0, 0, 0.25);
    }
    .modal-box .modal-top-btn-box{
        margin-left: 80%;
        padding-right: 6px;
    }
    .modal-box .modal-top-button{
        height: 20px;
        border: none;
        cursor: pointer;
        background: white;
    }
    .modal-box .modal-top-button:hover{
        background: #c7ecee;
    }
    .modal-top-logo{
        width: 50%; 
    }
    .modal-box .modal-header {
        height: 10%;    
        display: flex;
        flex-direction: row;
        justify-content: space-around;
    }
    .modal-box  .content-box{
        width: 100%;
        padding-left: 10px; 
        display: flex;
        flex-direction: row;
        justify-content: flex-start;
        align-items: space-around;
    }
    .add-modal{
        /* padding-top: 3px; */
        padding-top: 15px;
        position: fixed;
        z-index: 10;
        width: 45%;
        height: 40%;
        background-color: white;
        border: 1px solid black;
        border-radius: 2px;
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        align-items: flex-start;
    }
    .add-modal .content-box{
        padding-top : 20px;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-content: center;
    }
    .modal-content{
        padding-left: 10px;
        padding-top: 7px;
        display: flex;
        flex-direction: row;
    }
    .modal-content-tags{
        width: 11%;
        padding-right: 60px;
    }
    .modal-content-tag{
      width: 50%;
    }
     .selector{
        width: 52%;
    }
    .modal-content-tag-btn{
        padding-top: 3px;
        width: 95%;
        display: flex;
        justify-content: flex-end;
    }
    .errorMsg{
        color: red;
        font-size: 12px;
    }
</style>