<script>
import { required } from "vuelidate/lib/validators";

export default {
  data: () => ({
    city: "",
    adress: ["Индекс", "Страна", "Область", "Город*", "Улица", "Дом"],
  }),
  validations: {
    city: { required },
  },
};
</script>

<template>
  <section class="person">
    <div v-for="field in adress" :key="field" class="person__block">
      <div class="person__block-info">
        <label>{{ field }}:</label>
        <input v-if="field !== 'Город*'" class="person__input" type="text" />
        <input
          v-else
          @blur="$v.city.$touch"
          :class="{ invalid: $v.city.$dirty && !$v.city.required }"
          class="person__input"
          type="text"
          id="city"
          v-model.trim="city"
        />
        <small v-if="field === 'Город*' && $v.city.$dirty && !$v.city.required"
          >Пожалуйста, заполните поле</small
        >
      </div>
    </div>
    <p class="person__dop-info">Обязательно заполните поля с *</p>
  </section>
</template>

<style lang="scss" scoped>
</style>
