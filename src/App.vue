<template>
  <div id="App">
    <Header />
    <div class="container">
      <Exam6 />
    </div>
  </div>
</template>

<script>
  import Header from './components/Header'
  import Exam6 from './components/Exam6/Exam6';

  export default {
  name: 'App',
  components: {
    Header,
    Exam6,
  },

}
</script>

<style>
  h1 {
    color: blue;
  }

  .container {
    height: 100%;
    display: flex;
    justify-content: center;
  }

  .content {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    flex-wrap: wrap;
  }
</style>