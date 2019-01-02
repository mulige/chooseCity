<template>
    <div class="chooseCity">
      <Select v-model="model1" style="min-width:150px;max-width: 160px;" @on-change="deleteModel(1)">
        <Option v-for="item in firstFloorDatas" :value="item.id" :key="item.id">{{ item.name }}</Option>
      </Select>
      <Select v-model="model2" style="min-width:150px;max-width: 160px;" v-if="showSecond" @on-change="deleteModel(2)">
        <Option v-for="item in secondFloorDatas" :value="item.id" :key="item.id">{{ item.name }}</Option>
      </Select>
      <Select v-model="model3" style="min-width:150px;max-width: 160px;" v-if="showThird" >
        <Option v-for="item in thirdFloorDatas" :value="item.id" :key="item.id">{{ item.name }}</Option>
      </Select>
    </div>
</template>

<script>
 
  var cityDatas = require('./cityDatas')
    export default {
        name: "chooseCity",
        data() {
          return {
            model1: '',
            model2: '',
            model3: '',
            firstFloorDatas: [],
            secondFloorDatas: [],
            thirdFloorDatas: [],
            showSecond: false,
            showThird: false,
          }
        },
      props: {
          cityData: {
              default: ''
          }
      },
      methods: {
          handleFirstDatas() {
            console.log(cityDatas)
            this.firstFloorDatas = cityDatas.default.filter((item,index) => {
              return item.level == "1";
            })

          },
        handleSecondDatas() {
            let str = this.model1.toString()
            let strPreThree = str.substring(0,3);

            let re = new RegExp("^(" + strPreThree + ")\\d{2}(00)$")

          this.secondFloorDatas = cityDatas.default.filter((item,index) => {
            let str1 = item.id.toString()

            return (re.test(str1) && str1 !== str)
          })
          if(this.secondFloorDatas.length != 0) {
              this.showSecond = true;
          }

        },
        handleThridDatas() {
            if(this.model2) {
              let str = this.model2.toString();
              let strPreFive = str.substring(0,5)
              let re = new RegExp("^(" + strPreFive + ")\\d{2}$");
              this.thirdFloorDatas = cityDatas.default.filter((item,index) => {
                let str1 = item.id.toString()
                return (re.test(str1) && str1 !== str)
            })
              if(this.thirdFloorDatas.length != 0 && this.model2) {
                this.showThird = true;
              }
            }

        },
        //根据数据进行选择的城市的显示
        showData() {
          let items = cityDatas.default.filter((item,index) => {
            return item.id == this.cityData
          })
          if(items.length!=0) {
            let level = items[0].level;
            let id = items[0].id.toString();
            switch(level) {
              case 1:
                this.model1 = parseInt(this.cityData);
                break;
              case 2:
                this.model1 = parseInt(id.substring(0,3) + "0000");
                this.model2 = parseInt(this.cityData);
                break;
              case 3:
                this.model1 = parseInt(id.substring(0,3) + "0000");
                this.model2 = parseInt(id.substring(0,5) + "00");
                this.model3 = parseInt(this.cityData);
            }
          }

          // alert(this.model1,this.model2.this.model3)
        },
        getChoosedCity() {
            if(this.model3) {
              return this.model3
            }else if(this.model2) {
              return this.model2
            }else if(this.model1) {
              return this.model1
            }else {
              return ''
            }
        },
        deleteModel(id) {
            if(id===1) {
              this.model2 = '';
              this.model3 = '';
              this.showThird = false;
            }else if(id ===2) {
              this.model3 = '';
            }
        },
      },
      created() {
          this.handleFirstDatas()
          if(this.cityData) {
            this.showData();
          }
      },
      watch: {
        model1: {
          handler: function(newVal) {
            // alert(newVal)
            this.handleSecondDatas()
          }
        },
        model2: {
          handler: function(newVal) {

            this.handleThridDatas()
          }
        },
        cityData: {
          handler: function(newVal) {
            this.model2 = '';
            this.model3 = '';
            this.model1 = '';
            this.showSecond = false;
            this.showThird = false;
            this.showData();
            console.log(newVal)
          }
        }
      }
    }
</script>

<style >
  .chooseCity {
    width: 100%;
    height: 40px;
  }
</style>
