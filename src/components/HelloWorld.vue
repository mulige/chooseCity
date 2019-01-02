<template>
  
  <div class="chooseCityBox"> 
    <div class="box">
      <span>选择城市</span>
      <choose-city></choose-city> 
    </div>     
  </div>
</template>

<script>
import chooseCity from './chooseCity'
export default {
  name: 'HelloWorld',
  data () {
    return {
     
    }
  },
  components: {chooseCity}
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style >
  .chooseCityBox {
    width: 100%;
    height: 100%;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .box {
    width: 500px;
    height: 600px;
    box-sizing: border-box;
    padding-top: 50px;
    box-shadow: #ccc 0px 0px 50px 2px; 
  }
</style>
