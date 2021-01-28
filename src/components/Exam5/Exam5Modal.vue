<template>
    <div class="modal-box" v-if="!modalVisible">
        <div class="modal">
            <div class="modal-top">
                <div class="modal-top-logo">Modal</div>
                <div class="modal-top-btn-box"><button class="modal-top-button" @click="handleVisible()">X</button></div>
                <!-- handleVisible(newModalVisible) -->
            </div><br>
            <!-- <div class="modal-header-name">{{ item.name }}</div><br> -->
            <div class="modal-content">name: {{ modalItem.name }}</div>
            <div class="modal-content">type: {{modalItem.brewery_type}}</div>
            <div class="modal-content">state: {{modalItem.state}}</div>
            <div class="modal-content">city: {{modalItem.city}}</div>
            <div class="modal-content">phone: {{modalItem.phone}}</div>
            <div></div>
        </div>
    </div>
     <!-- <div class="modal-box" v-if="!visible">
        <div class="modal">
            <div class="modal-top">
                <div class="modal-top-logo">Modal</div>
                <div class="modal-top-btn-box"><button class="modal-top-button" @click="[visible = !visible, handleVisible(visible)]">X</button></div>
            </div><br>
            <div class="modal-content">name: {{ items[index].name }}</div>
            <div class="modal-content">type: {{items[index].brewery_type}}</div>
            <div class="modal-content">state: {{items[index].state}}</div>
            <div class="modal-content">city: {{items[index].city}}</div>
            <div class="modal-content">phone: {{items[index].phone}}</div>
            <div></div>
        </div>
    </div> -->
</template>

<script>
export default {
    props:{
        modalItem: Object,
        modalVisible: Boolean,
    },
    // data(){
    //     return {
    //         newModalItem: {},
            newModalVisible: true,
    //     }
    // },
    created(){
        // this.newModalItem = this.modalItem;
        // this.newModalVisible = this.modalVisible;
    },
    methods:{
        handleVisible(){
            this.newModalVisible = !this.modalVisible;
            this.$emit('modalVisible', this.newModalVisible);
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
</style>