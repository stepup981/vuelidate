<script>
import {
  required,
  numeric,
  maxLength,
  minLength,
} from "vuelidate/lib/validators";

export default {
  data: () => ({
    firstName: "",
    surName: "",
    middleName: "",
    birthDay: "",
    minBirthDate: "1930-01-01",
    maxBirthDate: new Date().toISOString().split("T")[0],
    phoneNumber: "",
    gender: "",
    clientGroup: [],
    attendingDoctor: "",
    isBirthDateValid: true,
  }),
  validations: {
    firstName: { required },
    surName: { required },
    birthDay: { required },

    phoneNumber: {
      required,
      numeric,
      maxLength: maxLength(11),
      minLength: minLength(11),
      startsWithSeven: (value) => value && value.toString().startsWith("7"),
    },
    clientGroup: { required },
  },
  methods: {
    updateBirthDate(newDate) {
      this.$v.birthDay.$touch();

      const selectedDate = new Date(newDate);
      const minDate = new Date(this.minBirthDate);
      const maxDate = new Date(this.maxBirthDate);

      if (selectedDate < minDate || selectedDate > maxDate) {
        this.isBirthDateValid = false;
      } else {
        this.isBirthDateValid = true;
      }
    },
  },
};
</script>

<template>
  <section class="person">
    <div class="person__block">
      <div class="person__block-info">
        <label>Фамилия*:</label>
        <input
          v-model.trim="surName"
          type="text"
          id="surName"
          placeholder="Введите вашу фамилию"
          @blur="$v.surName.$touch"
          class="person__input"
          :class="{ invalid: $v.surName.$dirty && !$v.surName.required }"
        />
      </div>

      <small v-if="$v.surName.$dirty && !$v.surName.required"
        >Пожалуйста, заполните поле</small
      >
    </div>

    <div class="person__block">
      <div class="person__block-info">
        <label>Имя*:</label>
        <input
          v-model.trim="firstName"
          type="text"
          id="firstName"
          placeholder="Введите ваше имя"
          @blur="$v.firstName.$touch"
          :class="{ invalid: $v.firstName.$dirty && !$v.firstName.required }"
          class="person__input"
        />
      </div>

      <small v-if="$v.firstName.$dirty && !$v.firstName.required"
        >Пожалуйста, заполните поле</small
      >
    </div>

    <div class="person__block">
      <div class="person__block-info">
        <label>Отчество</label>
        <input
          v-model.trim="middleName"
          type="text"
          id="middleName"
          placeholder="Введите ваше отчество"
          class="person__input"
        />
      </div>
    </div>

    <div class="person__block">
      <div class="person__block-info">
        <label>Дата рождения*:</label>
        <input
          v-model="birthDay"
          type="date"
          :min="minBirthDate"
          :max="maxBirthDate"
          id="birthDay"
          @blur="updateBirthDate($event.target.value)"
          class="person__input"
          :class="{
            invalid:
              $v.birthDay.$dirty &&
              (!$v.birthDay.required || !isBirthDateValid),
          }"
        />
      </div>

      <small v-if="$v.birthDay.$dirty && !$v.birthDay.required"
        >Пожалуйста, заполните поле</small
      >
      <small v-if="!isBirthDateValid"
        >Дата должна быть корректна. Посмотрите внимательно</small
      >
    </div>

    <div class="person__block">
      <div class="person__block-info">
        <label>Номер телефона*:</label>
        <input
          v-model.trim="phoneNumber"
          type="tel"
          id="phoneNumber"
          placeholder="78005553535"
          @blur="$v.phoneNumber.$touch"
          class="person__input"
          :class="{
            invalid:
              $v.phoneNumber.$dirty &&
              (!$v.phoneNumber.required ||
                !$v.phoneNumber.numeric ||
                !$v.phoneNumber.maxLength ||
                !$v.phoneNumber.minLength ||
                !$v.phoneNumber.startsWithSeven),
          }"
        />
      </div>

      <small v-if="$v.phoneNumber.$dirty && !$v.phoneNumber.required"
        >Пожалуйста, заполните поле</small
      >
      <small v-else-if="$v.phoneNumber.$dirty && !$v.phoneNumber.numeric"
        >Введите цифры</small
      >
      <small v-else-if="$v.phoneNumber.$dirty && !$v.phoneNumber.maxLength"
        >Введите телефон (11 цифр)</small
      >
      <small v-else-if="$v.phoneNumber.$dirty && !$v.phoneNumber.minLength"
        >Введите телефон (11 цифр)</small
      >
      <small
        v-else-if="$v.phoneNumber.$dirty && !$v.phoneNumber.startsWithSeven"
        >Телефон должен начинаться с 7</small
      >
    </div>

    <div class="person__block">
      <div class="person__block-info">
        <label>Пол:</label>
        <div>
          <input type="radio" id="male" value="male" v-model="gender" />
          <label for="male">Мужской</label>
          <input type="radio" id="female" value="female" v-model="gender" />
          <label for="female">Женский</label>
        </div>
      </div>
    </div>

    <div class="person__block">
      <div class="person__block-info">
        <label>Группа клиентов*:</label>
        <select
          @blur="$v.clientGroup.$touch"
          @change="$v.clientGroup.$touch"
          class="person__input"
          :class="{
            invalid: clientGroup.length === 0 && $v.clientGroup.$dirty,
          }"
          multiple
          v-model="clientGroup"
        >
          <option value="VIP">VIP</option>
          <option value="Проблемные">Проблемные</option>
          <option value="ОМС">ОМС</option>
        </select>
      </div>

      <small v-if="clientGroup.length === 0 && $v.clientGroup.$dirty"
        >Выберите хотя бы одну группу клиентов</small
      >
    </div>

    <div class="person__block">
      <div class="person__block-info">
        <label>Лечащий врач:</label>
        <select class="person__input" v-model="attendingDoctor">
          <option value="Иванов">Иванов</option>
          <option value="Захаров">Захаров</option>
          <option value="Чернышева">Чернышева</option>
        </select>
      </div>
    </div>

    <div class="person__block">
      <div class="person__block-info-sms">
        <label for="sms">Не отправлять СМС</label>
        <input type="checkbox" name="sms" id="sms" />
      </div>
    </div>

    <p class="person__dop-info">Обязательно заполните поля с *</p>
  </section>
</template>

<style lang="scss" scoped></style>
