<script>
import { required } from "vuelidate/lib/validators";

export default {
  data: () => ({
    typeDocument: "",
    datePassport: '',
    minBirthDate: "1930-01-01",
    maxBirthDate: new Date().toISOString().split("T")[0],
    isBirthDateValid: true
  }),
  validations: {
    typeDocument: { required },
    datePassport: { required },
  },
  methods: {
    updateBirthDate(newDate) {
      this.$v.datePassport.$touch();

      const selectedDate = new Date(newDate);
      const minDate = new Date(this.minBirthDate);
      const maxDate = new Date(this.maxBirthDate);

      if (selectedDate < minDate || selectedDate > maxDate) {
        this.isBirthDateValid = false;
      } else {
        this.isBirthDateValid = true;
      }
    },
  }
};
</script>

<template>
  <section class="person">
    <div class="person__block">
      <div class="person__block-info">
        <label>Тип документа*:</label>
        <select @blur="$v.typeDocument.$touch" @change="$v.typeDocument.$touch" v-model="typeDocument" class="person__input" :class="{invalid:$v.typeDocument.$dirty &&  typeDocument.length === 0}" id="typeDocument">
          <option value="Пасспорт">Пасспорт</option>
          <option value="Свидетельство о рождении">Свидетельство о рождении</option>
          <option value="Вод. удовстоверение">Вод. удовстоверение</option>
        </select>
      </div>
      <small v-if="$v.typeDocument.$dirty && typeDocument.length === 0">Выберите один из документов</small>
    </div>
    <div class="person__block">
      <div class="person__block-info">
        <label>Серия</label>
        <input class="person__input" type="text">
      </div>
    </div>
    <div class="person__block">
      <div class="person__block-info">
        <label>Номер</label>
        <input class="person__input" type="text">
      </div>
    </div>
    <div class="person__block">
      <div class="person__block-info">
        <label>Кем выдан</label>
        <input class="person__input" type="text">
      </div>
    </div>
    <div class="person__block">
      <div class="person__block-info">
        <label>Дата выдачи*:</label>
        <input
          v-model="datePassport"
          type="date"
          id="datePassport"
          @blur="updateBirthDate($event.target.value)"
          class="person__input"
          :class="{ invalid: $v.datePassport.$dirty && (!$v.datePassport.required || !isBirthDateValid)}"
        />
      </div>

      <small v-if="$v.datePassport.$dirty && !$v.datePassport.required"
        >Пожалуйста, заполните поле</small
      >
      <small v-if="!isBirthDateValid"
        >Дата должна быть корректна. Посмотрите внимательно</small
      >
    </div>
    <p class="person__dop-info">Обязательно заполните поля с *</p>
  </section>
</template>

<style lang="scss" scoped></style>
