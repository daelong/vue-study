인피니티 스크롤, 데이터 받아오는것
<template>
    <div id='content'>
        <Item :items="items" v-on:scroll="handleScroll()"/>
    </div>
</template>
    
<script>
import Item from './Item';

export default {
    name: 'content',
    components: {
        Item
    },
    created() {
      window.addEventListener('scroll', this.handleScroll);
      const fetchData = this.$axios.get("https://api.openbrewerydb.org/breweries?page="+this.pageNum+"&per_page=50");
      fetchData
        .then(res => res.data)
        .then(res => res.map(res => {
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
  data() {
    return {
      items: [],
      fetching: false,
      pageNum: 1,
    }
  },
  computed: {
  },
  methods: {
    handleScroll() {
      const scrollHeight = document.documentElement.scrollHeight;
      const scrollTop = document.documentElement.scrollTop;
      const clientHeight = document.documentElement.clientHeight;
      if (scrollTop + clientHeight >= scrollHeight && this.fetching === false) {
        // 페이지 끝에 도달하면 추가 데이터를 받아온다
        this.pageNum += 1;
        console.log(this.pageNum);
        this.fetchMoreItems();
      }
    },
    fetchMoreItems (){
      this.fetching = true;
      const fetchData = this.$axios.get("https://api.openbrewerydb.org/breweries?page="+this.pageNum+"&per_page=15  ");
      fetchData
        .then(res => res.data)
        .then(data => data.map(res => {
          return {
            ...res,
            visible: true
          }
        }))
        .then(res => { 
          this.items = this.items.concat(res);
           })
        .catch(error => console.log(error));
        this.fetching = false;
    }
  }
};
</script>

<style scoped>
div{
    flex:2; 
}

</style>