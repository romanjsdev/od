<template>
  <Main v-if="!popUpIsActive" v-on:showPopUp='showPopUp' />
  <PopUp v-else v-on:hidePopUp='hidePopUp' />
</template>

<script>
import Main from './components/Main.vue'
import PopUp from './components/PopUp.vue'

export default {
  name: 'App',
  components: {
    Main,
    PopUp
  },
  data(){
    return{
      popUpIsActive: false
    }
  },
  methods:{
    showPopUp(){
      this.popUpIsActive = true
    },
    hidePopUp(){
      this.popUpIsActive = false
    }
  }
}
</script>

<style>
@font-face {
 font-family: "LabGrotesque-Light";
 src: url("./font/LabGrotesque-Light.ttf") format("ttf"),

}
@font-face {
  font-family: "LabGrotesque-Regular";
  src: url("./font/LabGrotesque-Regular.ttf") format("ttf");
}
body{
  margin: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  width: 100vw;
  height: 100vh;
}
</style>
