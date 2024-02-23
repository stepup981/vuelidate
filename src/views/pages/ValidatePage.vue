<script>
import AdressForm from "@/components/AdressForm.vue";
import AgeForm from "@/components/AgeForm.vue";
import PassportForm from "@/components/PassportForm.vue";

export default {
  components: {
    AdressForm,
    AgeForm,
    PassportForm,
  },

  methods: {
    submitForm(e) {
      e.preventDefault();
      if (
        this.$refs.addressForm.$v.$invalid ||
        this.$refs.ageForm.$v.$invalid ||
        this.$refs.passportForm.$v.$invalid
      ) {
        console.log("test");
        this.$refs.addressForm.$v.$touch();
        this.$refs.ageForm.$v.$touch();
        this.$refs.passportForm.$v.$touch();
        return;
      } else {
        alert("Пользователь был создан");
      }
    },
  },
};
</script>

<template>
  <form class="validation">
    <AgeForm ref="ageForm" />
    <AdressForm ref="addressForm" />
    <PassportForm ref="passportForm" />
    <button class="validation__submit" @click="submitForm">
      Создать пользователя
    </button>
  </form>
</template>

<style lang="scss">
.validation {
  margin: 20px auto;
  width: max-content;

  &__submit {
    background-color: rgba(0, 212, 142, 0.733);
    border: none;
    padding: 20px 20px;
    cursor: pointer;
    transition: 0.3s ease;

    &:hover {
      transform: scale(1.1);
    }
  }
}
</style>
