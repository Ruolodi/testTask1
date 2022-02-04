<template>
  <div class="container">
 <form @submit.prevent="submit">
      <div class="card form-control">
        <h1>Форма регистрации</h1>
      <input  type="text" placeholder="Фамилия*" v-model.trim="lastName">
      <input  type="text" :style="v$.firstName.$error? styleObject : '' " v-model.trim="firstName" placeholder="Имя*">
      {{v$.firstName.$error}}
      <input  type="text" placeholder="Отчество">
      <input  type="date" placeholder="Дата рождения*">
      <input  type="text" placeholder="Номер телефона*">
       <hr/>
      <select v-model="gender">
      <option disabled="disabled" value="">Выберите пол</option>
      <option>Мужской</option>
      <option>Женский</option>
      </select>
      <select multiple  v-model="group">
      <option disabled value="">Группа клиентов*</option>
      <option ></option>
      </select>
       <select  v-model="doctor">
      <option disabled value="">Лечащий врач</option>
      <option>Иванов</option>
      <option>Захаров</option>
      <option>Чернышева</option>
      </select>
      <hr/>
      <div class="cardFlex">
        <p><input class="checkBox" type="checkbox" name="a" value="sms">Не отправлять СМС</p>
        </div>
      <hr/>
        <h1>Адрес:</h1>
        <input  type="text" placeholder="Индекс">
      <input  type="text" placeholder="Страна">
      <input  type="text" placeholder="Область">
      <input  type="text" placeholder="Город*">
      <input  type="text" placeholder="Улица">
      <input  type="text" placeholder="Дом">
      <hr/>
        <h1>Документ удостоверяющий личность:</h1>
           <select  v-model="document">
      <option disabled value="">Тип документа</option>
      <option :value="'birthCertificate'">Свидетельство о рождении</option>
      <option :value="'passport'">Паспорт</option>
      <option :value="'driverCertificate'">Водительское удостоверение</option>
      </select>
      <div class="passport" v-if="document==='passport'">
      <input  type="text" placeholder="Серия">
      <input  type="text" placeholder="Номер">
      <input  type="text" placeholder="Кем выдан">
      <input  type="date" placeholder="Дата выдачи*">
      </div>
      <div v-else-if="document==='birthCertificate'">
        <input  type="number" v-model="number" placeholder="Номер">
      </div>
      <div v-else-if="document==='driverCertificate'">
        <input  type="text" placeholder="Номер">
      </div>
        <div v-if="v$.firstName.$error"  style="width:100px; height:100px; border:2px solid green">

        </div>
      </div>
      <button class="btn" type="submit">Отправить</button>
 </form>
  </div>
</template>

<script>
import useVuelidate from '@vuelidate/core'
import { required, minLength, } from '@vuelidate/validators'

export default {
    setup () {
    return { v$: useVuelidate() }
    },
  data(){
    return{
      firstName: '',
      lastName: '',
      number:'',
      selected: '',
      document: '',
      styleCheck:'',
      doctor: ['Иванов','Захаров','Чернышева'],
      group:['VIP','Проблемные','ОМС'],
      gender:['Мужской','Женский'],
     styleObject: {
    borderColor: 'red'
  }

    }
  },
   validations () {
    return { 
      firstName: { required }, // Matches this.firstName
      lastName: { required }, // Matches this.lastName
      number: { required, minLength: minLength(11) },
    }
  },
  
    
  methods:{
   async submit(){
     const result = await this.v$.$validate()
      if(result===false){
        this.v$.firstName.$touch()
        console.log('gggg',this.v$.firstName.required)
        return
      }
    }
  },
}
</script>

<style>

</style>