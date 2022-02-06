<template>
  <div class="container">
    <form @submit.prevent="submit">
      <div class="card form-control">
        <h1>Форма регистрации</h1>
        <small v-if="v$.lastName.$error">Введите фамилию</small>
        <input
          type="text"
          placeholder="Фамилия*"
          :style="
            v$.lastName.$error || v$.lastName.mustBeLetter.$invalid
              ? styleObject
              : ''
          "
          v-model.trim="lastName"
        />
        <small v-if="v$.firstName.$error">Введите имя</small>
        <input
          type="text"
          placeholder="Имя*"
          :style="
            v$.firstName.$error || v$.firstName.mustBeLetter.$invalid
              ? styleObject
              : ''
          "
          v-model.trim="firstName"
        />
        <input type="text" placeholder="Отчество" />
        <input type="date" placeholder="Дата рождения*" />
        <small
          v-if="
            v$.number.minLength.$params.min != number.length && v$.number.$dirty
          "
          >Неправильный формат номера</small
        >
        <input
          type="number"
          placeholder="Номер телефона*"
          :style="
            v$.number.minLength.$params.min != number.length && v$.number.$dirty
              ? styleObject
              : ''
          "
          v-model.trim="number"
        />
        <hr />
        <select v-model="selectGender">
          <option disabled="disabled" value="">Выберите пол</option>
          <option v-for="items in gender" :key="items">{{ items }}</option>
        </select>
        <small v-if="v$.selectGroup.$error">Выберите группу</small>
        <select
          v-model="selectGroup"
          :style="v$.selectGroup.$error ? styleObject : ''"
          multiple
        >
          <option disabled value="">Группа клиентов*</option>
          <option v-for="items in group" :key="items">{{ items }}</option>
        </select>
        <select v-model="selectDoctor">
          <option disabled value="">Лечащий врач</option>
          <option v-for="items in doctor" :key="items">{{ items }}</option>
        </select>
        <hr />
        <div class="cardFlex">
          <p>
            <input class="checkBox" type="checkbox" name="a" value="sms" />Не
            отправлять СМС
          </p>
        </div>
        <hr />
        <h1>Адрес:</h1>
        <input type="text" v-model="index" placeholder="Индекс" />
        <input type="text" v-model="country" placeholder="Страна" />
        <input type="text" v-model="region" placeholder="Область" />
        <small v-if="v$.city.$error">Укажите ваш город</small>
        <input
          type="text"
          :style="v$.city.$error ? styleObject : ''"
          placeholder="Город*"
          v-model="city"
        />
        <input type="text" v-model="street" placeholder="Улица" />
        <input type="text" v-model="home" placeholder="Дом" />

        <hr />
        <h1>Документ удостоверяющий личность:</h1>
        <select v-model="document">
          <option disabled value="">Тип документа</option>
          <option :value="'birthCertificate'">Свидетельство о рождении</option>
          <option :value="'passport'">Паспорт</option>
          <option :value="'driverCertificate'">
            Водительское удостоверение
          </option>
        </select>
        <div class="passport" v-if="document === 'passport'">
          <input type="text" v-model="passportSeries" placeholder="Серия" />
          <input type="text" v-model="passportNumber" placeholder="Номер" />
          <input type="text" v-model="passportIssued" placeholder="Кем выдан" />
          <input
            type="date"
            :style="v$.passportDate.$error ? styleObject : ''"
            v-model="passportDate"
            placeholder="Дата выдачи*"
          />
        </div>
        <div v-else-if="document === 'birthCertificate'">
          <input type="number" v-model="birthNumber" placeholder="Номер" />
        </div>
        <div v-else-if="document === 'driverCertificate'">
          <input type="text" v-model="driverNumber" placeholder="Номер" />
        </div>
      </div>
      <button class="btn" type="submit">Отправить</button>
      <div class="validate" v-if="sent">Ваши данные отправлены!</div>
      <div class="errorValidate" v-if="errorSwitch">
        Пожалуйста проверьте правильность заполнения полей!
      </div>
    </form>
  </div>
</template>

<script>
import useVuelidate from "@vuelidate/core"
import { required, minLength } from "@vuelidate/validators"

const mustBeLetter = (value) => {
  return !/[^a-zA-Z]+[^а-яА-Я]/.test(value) //проверка на наличие цифр и спец символов в строке
}

export default {
  setup() {
    return { v$: useVuelidate() }
  },
  data() {
    return {
      firstName: "",
      lastName: "",
      number: "",
      city: "",
      selected: "",
      document: "",
      passportSeries: "",
      passportNumber: "",
      passportIssued: "",
      passportDate: "",
      styleCheck: "",
      selectDoctor: "",
      selectGender: "",
      selectGroup: "",
      index: "",
      country: "",
      region: "",
      street: "",
      home: "",
      birthNumber: "",
      driverNumber: "",
      errorSwitch: false,
      sent: false,
      doctor: ["Иванов", "Захаров", "Чернышева"],
      group: ["VIP", "Проблемные", "ОМС"],
      gender: ["Мужской", "Женский"],
      styleObject: {
        borderColor: "red",
      },
    }
  },
  validations() {
    return {
      firstName: { required, mustBeLetter },
      lastName: { required, mustBeLetter },
      city: { required },
      number: { required, minLength: minLength(11) },
      selectGroup: { required },
      passportDate: { required },
    }
  },

  methods: {
    async submit() {
      const result = await this.v$.$validate()
      if (result === false) {
        this.v$.firstName.$touch()
        this.v$.lastName.$touch()
        this.v$.city.$touch()
        this.v$.number.$touch()
        this.v$.selectGroup.$touch()
        console.log(this.passportDate)
        this.errorSwitch = true
        return
      } else {
        this.errorSwitch = false
        this.sent = true
      }
    },
  },
  watch: {
    number() {
      if (this.number.length === 1 && this.number !== "7") {
        this.number = +7 + this.number
      }
    },
  },
}
</script>

<style></style>
