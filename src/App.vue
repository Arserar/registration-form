<template>
  <form @submit.prevent="submit">
    <!-- First step -->
    <h1 class="first_step_title">Создание клиента</h1>
    <div class="names">
      <input-with-validation
        title="Имя"
        :options="{
          name: 'firstName',
          val: firstName,
        }"
        :validation="v$"
        @input="changeInputValue"
        class="name_label"
      ></input-with-validation>
      <input-with-validation
        title="Фамилия"
        :options="{
          name: 'lastName',
          val: lastName,
        }"
        :validation="v$"
        @input="changeInputValue"
        class="name_label"
      ></input-with-validation>
      <regular-input class="name_label" title="Отчество"></regular-input>
    </div>
    <div class="dateandphone">
      <input-with-validation
        title="Дата рождения"
        :options="{
          name: 'dateOfBirth',
          val: dateOfBirth,
          condition: 'isDate',
          hint: 'Введите, пожалуйста, валидную дату рождения.',
          placeholder: 'ГГГГ-ММ-ДД',
        }"
        :validation="v$"
        @input="changeInputValue"
        class="date_label"
      ></input-with-validation>
      <input-with-validation
        title="Номер телефона"
        :options="{
          name: 'phone',
          val: phone,
          condition: 'isPhoneNumber',
          hint: 'Введите, пожалуйста, валидный номер телефона.',
          placeholder: '7••••••••••',
        }"
        :validation="v$"
        @input="changeInputValue"
        class="phone_label"
      ></input-with-validation>
    </div>
    <div class="gender">
      <div class="gender_label">
        <span>Пол:</span>
        <input type="radio" id="male" value="male" v-model="picked" />
        <label for="male">Мужской</label>
        <input type="radio" id="female" value="female" v-model="picked" />
        <label for="female">Женский</label>
      </div>
    </div>
    <select-with-validation
      title="Группа клиентов"
      :options="{
        name: 'selectGroupOfClients',
        val: selectGroupOfClients,
        defaultValue: 'Выберите группу',
        multiple: true,
        opts: ['VIP', 'Проблемные', 'ОМС'],
      }"
      :validation="v$"
      @select="changeInputValue"
      class="group_of_clients_label"
    ></select-with-validation>
    <div class="doctor_label">
      <span>Лечащий врач:</span>
      <select v-model="selectDoctor">
        <option selected value="Ivanov">Иванов</option>
        <option value="Zaxarov">Захаров</option>
        <option value="Chernisheva">Чернышева</option>
      </select>
    </div>
    <div class="sms_label">
      <input type="checkbox" id="checkbox" v-model="checked" />
      <label for="checkbox">Не отправлять СМС</label>
    </div>

    <!-- Address step -->
    <h1 class="step_title">Адрес</h1>
    <regular-input class="address_label" title="Индекс"></regular-input>
    <regular-input class="address_label" title="Страна"></regular-input>
    <regular-input class="address_label" title="Область"></regular-input>
    <input-with-validation
      title="Город"
      :options="{
        name: 'addressCity',
        val: addressCity,
      }"
      :validation="v$"
      @input="changeInputValue"
      class="address_label"
    ></input-with-validation>
    <regular-input class="address_label" title="Улица"></regular-input>
    <regular-input class="address_label" title="Дом"></regular-input>

    <!-- Passport step -->
    <h1 class="step_title">Паспорт</h1>
    <select-with-validation
      title="Тип документа"
      :options="{
        name: 'selectDocType',
        val: selectDocType,
        defaultValue: 'Выберите тип',
        opts: ['Паспорт', 'Свидетельство о рождении', 'Вод. удостоверение'],
      }"
      :validation="v$"
      @select="changeInputValue"
      class="doctype_label"
    ></select-with-validation>
    <regular-input class="passport_label" title="Серия"></regular-input>
    <regular-input class="passport_label" title="Номер"></regular-input>
    <regular-input class="passport_label" title="Кем выдан"></regular-input>
    <input-with-validation
      title="Дата выдачи"
      :options="{
        name: 'dateOfIssue',
        val: dateOfIssue,
        condition: 'isDate',
        hint: 'Введите, пожалуйста, валидную дату выдачи.',
        placeholder: 'ГГГГ-ММ-ДД',
      }"
      :validation="v$"
      @input="changeInputValue"
      class="date_label date_issue_label"
    ></input-with-validation>
    <div class="divider"></div>
    <button class="button" type="submit" :disabled="submitStatus === 'PENDING'">
      Создать клиента
    </button>
    <p v-if="submitStatus === 'OK'">Клиент создан!!!</p>
    <p v-if="submitStatus === 'ERROR'">
      Заполните, пожалуйста, все поля правильно.
    </p>
    <p v-if="submitStatus === 'PENDING'">Отправка...</p>
  </form>
</template>

<script>
import useVuelidate from "@vuelidate/core";
import { required } from "@vuelidate/validators";
import RegularInput from "./components/RegularInput";
import InputWithValidation from "./components/InputWithValidation";
import SelectWithValidation from "./components/SelectWithValidation";
import moment from "moment";

const isDate = (value) => moment(value, "YYYY-MM-DD", true).isValid();

const isPhoneNumber = (value) => value[0] === "7" && value.length === 11;

export default {
  setup() {
    return { v$: useVuelidate() };
  },
  components: {
    "regular-input": RegularInput,
    "input-with-validation": InputWithValidation,
    "select-with-validation": SelectWithValidation,
  },
  data() {
    return {
      firstName: "",
      lastName: "",
      dateOfBirth: "",
      phone: "",
      picked: "",
      selectGroupOfClients: [],
      selectDoctor: "Ivanov",
      checked: false,
      addressCity: "",
      selectDocType: "",
      dateOfIssue: "",
      submitStatus: null,
    };
  },
  validations() {
    return {
      firstName: { required },
      lastName: { required },
      dateOfBirth: {
        required,
        isDate(value) {
          return isDate(value);
        },
      },
      dateOfIssue: {
        required,
        isDate(value) {
          return isDate(value);
        },
      },
      phone: {
        required,
        isPhoneNumber(value) {
          return isPhoneNumber(value);
        },
      },
      selectGroupOfClients: {
        required,
      },
      addressCity: {
        required,
      },
      selectDocType: {
        required,
      },
    };
  },
  methods: {
    submit() {
      console.log("submit!");
      this.v$.$touch();
      if (this.v$.$invalid) {
        this.submitStatus = "ERROR";
      } else {
        this.submitStatus = "PENDING";
        setTimeout(() => {
          this.submitStatus = "OK";
        }, 500);
      }
    },
    changeInputValue(val, name) {
      if (this.v$[name]) {
        this[name] = val;
        this.v$[name].$touch();
      }
    },
  },
};
</script>

<style lang="scss">
body {
  background: #2c3e50;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  border-radius: 5px;
  background: #fff;
  padding-bottom: 25px;
}

form {
  display: flex;
  flex-direction: column;
  align-items: center;
}

input {
  border: 1px solid silver;
  border-radius: 4px;
  background: white;
  padding: 5px 10px;
}

.error {
  border-color: red;
  background: #fdd;
}

.error:focus {
  outline-color: #f99;
}

.valid {
  border-color: #5a5;
  background: #efe;
}

.valid:focus {
  outline-color: #8e8;
}

.errors {
  color: red;
}

.names {
  display: flex;
  justify-content: space-between;
  padding: 10px;

  @media (max-width: 649px) {
    flex-direction: column;
    justify-content: center;
    align-items: center;

    & > label {
      margin: 0;
      margin-bottom: 10px;
    }
  }
}

.dateandphone,
.gender {
  display: flex;
  padding: 10px;
}

.dateandphone {
  @media (max-width: 460px) {
    flex-direction: column;
    justify-content: center;
    align-items: center;

    & > label {
      margin: 0;
      margin-bottom: 10px;
    }
  }
}

.name_label,
.date_label,
.phone_label,
.group_of_clients_label,
.doctor_label,
.address_label,
.doctype_label,
.passport_label {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: start;
}

.name_label,
.date_label {
  margin-right: 10px;
}

.address_label,
.doctype_label {
  margin-bottom: 10px;
}

.name_label,
.phone_label {
  &:nth-last-of-type(1) {
    margin: 0;
  }
}

.address_label,
.doctype_label,
.date_issue_label,
.name_label,
.passport_label,
.phone_label {
  max-width: 190px;
}

.date_issue_label {
  margin-top: 10px;
}

.group_of_clients_label,
.doctor_label {
  align-items: center;
}

.doctor_label,
.sms_label {
  margin-top: 20px;
}

.date_label {
  flex-grow: 1;

  & > span {
    align-self: center;
  }

  & > input {
    align-self: stretch;
  }
}

.step_title {
  border-top: 3px solid #000;
  border-bottom: 3px solid #000;
  align-self: stretch;
  text-align: center;
}

.divider {
  width: 230px;
  height: 5px;
  background: #000;
  margin-top: 10px;
}

button {
  background: #fff;
  font-weight: bold;
  height: 40px;
  border-color: #000;
  border-style: solid;
  border-radius: 5px;
  font-size: 1.2rem;
  margin-top: 10px;
  transition: background, color 1s ease;

  &:hover {
    background: #000;
    color: #fff;
    cursor: pointer;
  }
}
</style>
