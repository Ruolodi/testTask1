<template>
  <div class="container">
    <div class="card form-control ">
      <h1>Форма регистрации</h1>
    <input  type="text" placeholder="Фамилия*">
    <input  type="text" placeholder="Имя*">
    <input  type="text" placeholder="Отчество">
    <input  type="text" placeholder="Дата рождения*">
    <input  type="text" placeholder="Номер телефона*">
     <hr/>
    <select  v-model="selected">
    <option disabled="disabled" value="">Выберите пол</option>
    <option>Мужской</option>
    <option>Женский</option>
    </select>
    <select multiple  v-model="selected">
    <option disabled value="">Группа клиентов*</option>
    <option>VIP</option>
    <option>Проблемные</option>
    <option>ОМС</option>
    </select>
     <select  v-model="selected">
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
    <input  type="text" placeholder="Дата выдачи*">
    </div>
    <div v-else-if="document==='birthCertificate'">
      <input  type="text" placeholder="Номер">
    </div>
    <div v-else-if="document==='driverCertificate'">
      <input  type="text" placeholder="Номер">
    </div>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return{
      selected: '',
      document: ''
    }
  }
}
</script>

<style>

</style>