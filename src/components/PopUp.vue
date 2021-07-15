<template>
  <div class="popup-wrapper"> 
    <div :class="showCheckbox ? 'popup show-checkbox' : 'popup'">
        <div class="popup-inner">
            <img class="close" src="../assets/close.svg" @click="hidePopUp">
            <h3>Налоговый вычет</h3>
            <p class="desc">Используйте налоговый вычет чтобы погасить ипотеку досрочно. Размер налогового вычета составляет не более 13% от своего официального годового дохода.</p>
            <span class="form-header">Ваша зарплата в месяц</span>
            <div class="form">
                <input v-on:input="inputChange" type="text" placeholder="Введите данные" v-model.number="salary"
                    @keydown="inputHandler">
                <button @click="calculate">Рассчитать</button>
            </div>
            <div v-if="showCheckbox" class="checkboxes">
                <div v-if="recoupment[0]">
                    <input class="checkbox-orange" type="checkbox" id="1" value="yes">
                    <label for="1">{{recoupment[0]}} рублей в 1-ый год</label>
                </div>
                <div v-if="recoupment[1]">
                    <input class="checkbox-orange" type="checkbox" id="2" value="yes">
                    <label for="2">{{recoupment[1]}} рублей в 2-ой год</label>
                </div>
                <div v-if="recoupment[2]">
                    <input class="checkbox-orange" type="checkbox" id="3" value="yes">
                    <label for="3">{{recoupment[2]}} рублей в 3-ий год</label>
                </div>
                <div v-if="recoupment[3]">
                    <input class="checkbox-orange" type="checkbox" id="4" value="yes">
                    <label for="4">{{recoupment[3]}} рублей в 4-ый год</label>
                </div>
            </div>
            <div class="switcher">
                <span>Что уменьшаем?</span>
                <button :class="payment ? 'active' : 'disabled'" @click="paymentActive">Платеж</button>
                <button :class="!payment ? 'active' : 'disabled'" @click="timeActive">Срок</button>
            </div>
            <button class="add">Добавить</button>
        </div>
    </div>
  </div>
</template>

<script>
export default {
    name: 'PopUp',
    data(){
        return{
            showCheckbox: false,
            payment: true,
            salary: '',
            recoupment: [],
            maxRecoupment: 260000 
        }
    },
    methods:{
        hidePopUp(){
            this.$emit('hidePopUp')
        },
        paymentActive(){
            this.payment = true
        },
        timeActive(){
            this.payment = false
        },
        inputHandler(e){
        if ( e.keyCode == 46 || e.keyCode == 8 || e.keyCode == 9 || e.keyCode == 27 ||
            (e.keyCode == 65 && e.ctrlKey === true) || (e.keyCode >= 35 && e.keyCode <= 39)) {
                return;
        } else {
            if ((e.keyCode < 48 || e.keyCode > 57) && (e.keyCode < 96 || e.keyCode > 105 )) {
                    e.preventDefault();
                }
            }   
        },
        inputChange(){
            this.recoupment = []
        },
        calculate(){
            if(this.salary!=0){
                this.recoupment = []
                this.showCheckbox = true
                let rec = (this.salary*12)*0.13
                rec = rec > this.maxRecoupment ? this.maxRecoupment : rec
                let y = Math.floor(this.maxRecoupment/rec)
                let lyRec = this.maxRecoupment%rec
                for(let i=0; i<y;i++){
                    this.recoupment.push(rec)
                }
                this.recoupment.push(lyRec) 
            }
        }
    }
}
</script>


<style scoped>
    .popup-wrapper{
        background: rgba(0, 0, 0, 0.3);
        width: 100%;
        height: 100%;
        display: flex;
        justify-content: center;
        font-family: "LabGrotesque-Regular";
    }
    button{
        font-family: "LabGrotesque-Regular";
    }
    .popup{
        display: flex;
        background: #FFF;
        width: 35%;
        height: auto;
        border-radius: 30px;
        margin: 12vh 0 12vh 0;
        position: relative;
        transition: 0.2s ease;
    }
    .show-checkbox{
        margin: 12vh 0 5vh 0;
    }
    .popup-inner{
        display: flex;
        flex-direction: column;
        justify-content: flex-start;
        width: 90%;
        margin: 0 auto;
    }
    .close{
        position: absolute;
        right: 27px;
        top: 27px;
        cursor: pointer;
        transition: 0.3s ease;
    }
    .close:hover{
        transform: rotate(-90deg);
    }
    h3{
        font-weight: 500;
        font-size: 28px;
        line-height: 40px;
    }
    .desc{
        font-style: normal;
        font-weight: normal;
        font-size: 14px;
        line-height: 24px;
        color: #808080;
    }
    .form{
        display: flex;
        flex-direction: column;
    }
    .form button{
        width: 30px;
        border: none;
        background: none;
        cursor: pointer;
        font-style: normal;
        font-weight: 500;
        font-size: 14px;
        line-height: 24px;
        color: #EA0029;
        padding: 0;
        transition: 0.3s ease;
    }
    .form button:hover{
        color: #F53A31;  
    }
    .form-header{
        margin: 10px 0;
        font-family: "LabGrotesque-Regular";
        font-weight: 700;
        font-size: 14px;
        line-height: 24px;
    }
    input{
        border: 1px solid #DFE3E6;
        box-sizing: border-box;
        border-radius: 3px;
        padding: 10px;
    }
    input:hover{
        border: 1px solid #000000;
    }
    input:focus{
        border: 1px solid #DFE3E6;
    }
    input:focus-visible{
        outline: none;
    }
    .checkboxes input,label{
        cursor: pointer;
    }
    .checkbox-orange{
        position: absolute;
        z-index: -1;
        opacity: 0;
    }
    .checkbox-orange+label {
        display: inline-flex;
        align-items: center;
        user-select: none;
    }
    .checkbox-orange+label::before{
        content: '';
        display: inline-block;
        width: 1em;
        height: 1em;
        flex-shrink: 0;
        flex-grow: 0;
        border: 1px solid #adb5bd;
        border-radius: 0.25em;
        margin-right: 0.5em;
        background-repeat: no-repeat;
        background-position: center center;
        background-size: 50% 50%;
    }
    .checkbox-orange+label:hover::before{
        border: 1px solid #000000;
    }
    .checkbox-orange:checked+label::before{
        border-color: #FF5E56;
        background: linear-gradient(255.35deg, #DC3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #FF5E56;
        background-image: url("../assets/checkbox.svg");
        background-repeat: no-repeat;
        background-position: center;
    }
    .switcher{
        margin: 25px 0;
    }
    .switcher span{
        font-style: normal;
        font-weight: 700;
        font-size: 14px;
        line-height: 24px;
    }
    .switcher button{
        border: none;
        background: none;
        cursor: pointer;
        padding: 10px;
        margin-left: 15px;
    }
    .switcher .active{
        background: linear-gradient(255.35deg, #DC3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #FF5E56;
        border-radius: 50px;
        color: #FFF;
    }
    .switcher .disabled{
        background: #EEF0F2;
        border-radius: 50px;
    }
    .add{
        background: linear-gradient(255.35deg, #DC3131 0.83%, rgba(255, 79, 79, 0) 108.93%), #FF5E56;
        box-shadow: 0px 0px 24px rgba(234, 0, 41, 0.33);
        border-radius: 6px;
        color: #FFF;
        font-size: 16px;
        line-height: 24px;
        width: 100%;
        height: 56px;
        border: none;
        cursor: pointer;
        margin-bottom: 20px;
        margin-top: auto;
    }
    .add:hover{
        background: #EA0029;
        box-shadow: 0px 0px 24px rgba(234, 0, 41, 0.33);
    }
    @media screen and (max-width: 1024px) {
        .popup{
            width: 60%;
            margin: 10vh 0 55vh 0;
        }
        .show-checkbox{
            margin: 10vh 0 50vh 0;
        }
    }
    @media screen and (max-width: 769px) {
        .popup{
            width: 60%;
            margin: 10vh 0 40vh 0;
        }
        .show-checkbox{
            margin: 10vh 0 35vh 0;
        }
    }
    @media screen and (max-width: 425px) {
        .popup{
            width: 100%;
            height: 100%;
            margin: 0;
            border-radius: 0;
        }
        .show-checkbox{
            margin: 0;
        }
    }
</style>
