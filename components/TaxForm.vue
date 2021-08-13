<template>
  <div class="deduction">
    <h3 class="deduction__header">Налоговый вычет</h3>
    <p class="deduction__description">
      Используйте налоговый вычет чтобы погасить ипотеку досрочно. Размер
      налогового вычета составляет не более 13% от своего официального годового
      дохода.
    </p>
    <h3 class="deduction__input-title">Ваша зарплата в месяц</h3>
    <p>
      <input
        class="input-element"
        name="salary_name"
        type="text"
        v-model="salary"
        placeholder="Введите данные"
        @blur="$v.salary.$touch()"
        v-bind:class="{ active: $v.$error}"
      />
    </p>
    <div class="input__error" v-if="$v.$error">
      <span v-if="!$v.salary.required"> Поле обязательно для заполнения </span>
    </div>
    <p>
      <button class="calculate-btn" @click="calculateTaxDeduction">
        Рассчитать
      </button>
    </p>

    <section class="calculate" v-if="payments.length !== 0">
      <h3 class="calculate__title">
        Итого можете внести в качестве досрочных:
      </h3>
      <div v-for="payment in payments" v-bind:key="payment.year">
        <input type="checkbox" class="calculate__checkbox" :id="payment.year" />
        <label :for="payment.year" class="calculate__checkbox-label"
          >{{ payment.value }} рублей <span class="calculate__interval">&nbsp;{{ formatNumeral(payment.year) }} год</span> </label
        >
      </div>
    </section>

    <section class="options">
      <h4 class="options__title">Что уменьшаем?</h4>
      <label class="checkbox-btn">
        <input type="checkbox" checked />
        <span>Платеж</span>
      </label>

      <label class="checkbox-btn">
        <input type="checkbox" />
        <span>Срок</span>
      </label>
    </section>
    <div class="footer-btn">
      <button class="deduction__add-btn" :disabled="$v.$invalid">
        Добавить
      </button>
    </div>
  </div>
</template>

<script>
import { required } from "vuelidate/lib/validators";

export default {
  data() {
    return {
      payments: [],
      salary: "",
      tax: 0,
      maxTaxDeduction: 260000,
    };
  },

  validations: {
    salary: {
      required,
    },
  },

  methods: {
    calculateTaxDeduction() {
      let numericSalary = Number(this.salary);
      if (isNaN(numericSalary) === false && numericSalary !== 0) {
        let tax = numericSalary * 12 * 0.13;
        let currentDeduction = this.maxTaxDeduction;
        let currentYear = 1;
        let payments = [];
        while (true) {
          if (Math.floor(currentDeduction / tax) !== 0) {
            payments.push({ value: Math.round(tax,2).toLocaleString(), year: currentYear });
          } else {
            payments.push({
              value: Math.round(currentDeduction,2).toLocaleString(),
              year: currentYear,
            });
            break;
          }
          currentDeduction -= tax;
          currentYear += 1;
        }
        this.payments = payments;
      }
    },

    formatNumeral(numeral) {
      if ([1, 4, 5].includes(numeral)) {
        return ` в ${numeral}-ый`;
      }

      if (numeral == 2) {
        return `во ${numeral}-ой`;
      }

      if ([6, 7, 8].includes(numeral)) {
        return `в ${numeral}-ой`;
      }

      if (numeral == 3) {
        return `в ${numeral}-ий`;
      }

      if (numeral > 8 && numeral < 20) {
        return `в ${numeral}-ый`;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
%font-shared {
  font-family: $font-stack;
  font-size: 14px;
  line-height: 24px;
}

.deduction {
  position: relative;
  &__header {
    font-family: $font-stack;
    font-size: 28px;
    font-weight: 500;
    line-height: 40px;
  }
  &__description {
    @extend %font-shared;
    font-weight: normal;
    color: #808080;
    margin: 16px 0px 24px;
  }

  &__input-title {
    @extend %font-shared;
    font-weight: 500;
  }

  &__add-btn {
    width: 100%;
    background: $primary-background;
    min-height: 56px;
    color: white;
    box-shadow: 0px 0px 24px rgba(234, 0, 41, 0.33);
    border-radius: 6px;
    border: none;
    font-family: $font-stack;
    font-size: 16px;
    font-weight: 500;
    line-height: 24px;
  }

  &__add-btn:hover,
  &__add-btn:active {
    cursor: $custom-cursor;
    background: #ea0029;
  }
}

button:disabled {
  background: #bec5cc;
  box-shadow: none;
}

.input-element {
  margin: 8px 0px;
  padding: 8px;
  width: 100%;
  @extend %font-shared;
  border: 1px solid $label-color;
  border-radius: 3px;
}

.active{
  border: 1px solid #EA0029;
  box-sizing: border-box;
}

.input__error {
  color: red;
  text-align: left;
  font-size: 10px;
  margin-top: 3px;
}

.calculate-btn {
  color: $button-color;
  border: none;
  background-color: inherit;
  cursor: $custom-cursor;
  @extend %font-shared;
  font-weight: 500;
}

.calculate-btn:active {
  color: $button-color;
  cursor: $custom-cursor;
}

.calculate-btn:hover {
  color: #f53a31;
  cursor: $custom-cursor;
}

.calculate {
  &__title {
    @extend %font-shared;
    font-weight: 500;
    margin-top: 16px;
  }
  &__interval{
    color:gray;
  }
  &__checkbox {
    position: absolute;
    z-index: -1;
    opacity: 0;
  }
  &__checkbox + label {
    display: inline-flex;
    align-items: center;
    user-select: none;
    margin-top: 16px;
    padding-bottom: 16px;
    width: 100%;
    border-bottom: 1px solid $label-color;
  }
  &__checkbox + label::before {
    content: "";
    display: inline-block;
    width: 1em;
    height: 1em;
    flex-shrink: 0;
    flex-grow: 0;
    border: 1px solid $label-color;
    border-radius: 6px;
    margin-right: 0.5em;
    background-repeat: no-repeat;
    background-position: center center;
    background-size: 50% 50%;
  }

  &__checkbox:checked + label::before {
    background: $primary-background;
    background-image: url("@/assets/img/checkbox.svg");
  }
  &__checkbox:hover + label::before {
    border: 1px solid black;
    cursor: $custom-cursor;
  }

  &__checkbox-label {
    @extend %font-shared;
    font-weight: 500;
  }

  &__interval {
    @extend %font-shared;
    font-weight: 500;
    color: gray;
  }
}

.options {
  margin-bottom: 46px;
  margin-top: 24px;
  &__title {
    display: inline-flex;
    @extend %font-shared;
    font-weight: 500;
    margin-right: 32px;
  }

  .checkbox-btn {
    display: inline-block;
    margin: 0 16px 0 0;
    user-select: none;
    position: relative;
  }
  .checkbox-btn input[type="checkbox"] {
    z-index: -1;
    opacity: 0;
    display: block;
    width: 0;
    height: 0;
  }
  .checkbox-btn span {
    display: inline-block;
    cursor: $custom-cursor;
    background: $checkbox-background;
    width: 57px;
    height: 36px;
    border-radius: 50px;
    border: none;
    color: black;
    font-family: $font-stack;
    text-align: center;
    font-size: 14px;
    font-weight: normal;
    line-height: 24px;
    padding-top: 6px;
  }

  .checkbox-btn input[type="checkbox"]:checked + span {
    background: $primary-background;
    width: 73px;
    height: 36px;
    border-radius: 50px;
    border: none;
    color: $checkbox-background;
    @extend %font-shared;
    font-weight: normal;
  }

  .checkbox-btn input[type="checkbox"]:hover + span {
    background: $label-color;
    border-radius: 50px;
    color: black;
  }

  .checkbox-btn input[type="checkbox"]:disabled + span {
    background: $checkbox-background;
    color: black;
    cursor: default;
  }
  .checkbox-btn input[type="checkbox"]:checked:disabled + span {
    background: $checkbox-background;
  }
}

@media (max-width: 600px) {
  .options .checkbox-btn {
    margin: 0;
  }
}
</style>
