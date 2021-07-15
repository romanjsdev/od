<template>
  <div class="main"> 
    <button @click="showPopUp">Налоговый вычет</button>
  </div>
</template>

<script>
export default {
  name: 'Main',
  methods:{
    showPopUp(){
      this.$emit('showPopUp')
    }
  }
}
</script>


<style scoped>
  .main{
    display: flex;
    justify-content: center;
    width: 100%;
    height: 100%;
    background: linear-gradient(255.35deg, #DC3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #FF5E56;
    box-shadow: 0px -0.11px 16.9495px rgba(183, 187, 225, 0.33);
  }
  button{
    font-family: "LabGrotesque-Regular";
    font-size: 16px;
    line-height: 24px;
    cursor: pointer;
    align-self: center;
    width: 198px;
    height: 56px;
    border: 1px solid #FFFFFF;
    color: #FFF;
    background-color: rgba(0, 0, 0, 0);
    box-sizing: border-box;
    border-radius: 6px;
    transition: 0.3s ease;
  }
  button:hover{
    background: #FFF;
    color: #000;
  }
</style>
