<template>
  <div class="deduction">
    <h3 class="deduction__header">Налоговый вычет</h3>
    <p class="deduction__description">Используйте налоговый вычет чтобы погасить ипотеку досрочно. 
      Размер налогового вычета составляет не более 13% от своего 
      официального годового дохода.
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
          >{{ payment.value }} рублей {{ formatNumeral(payment.year) }} </label
        ><span class="calculate__interval"></span>
      </div>
    </section>

    <section class="options">
      <h4 class="options__title">Что уменьшаем?</h4>
      <label class="checkbox-btn">
        <input type="checkbox" checked>
        <span>Платеж</span>
      </label>
	
    <label class="checkbox-btn">
      <input type="checkbox">
      <span>Срок</span>
    </label>
    </section>
    <button class="deduction__add-btn" :disabled="$v.$invalid">Добавить</button>
  </div>
</template>

<script>
import {required} from 'vuelidate/lib/validators'

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
          required
        }
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
            payments.push({ value: tax, year: currentYear });
          } else {
            payments.push({ value: currentDeduction, year: currentYear });
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
        return `в ${numeral}-ый`;
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

      if ((numeral > 8) && (numeral < 20)) {
        return `в ${numeral}-ый`;
      }
    },
  },
};
</script>

<style lang="scss" scoped>
.deduction {
  position: relative;
  &__header {
    font-family: $font-stack;
    font-size: 28px;
    font-weight: 500;
    line-height: 40px;
  }
  &__description {
    color: #808080;
    font-family: $font-stack;
    font-size: 14px;
    font-weight: normal;
    line-height: 24px;
    white-space: pre-line;
    margin: 16px 0px 24px;
  }

  &__input-title {
    font-family: $font-stack;
    font-size: 14px;
    font-weight: 500;
    line-height: 24px;
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
  &__add-btn:active{
    cursor: url("@/assets/img/pointer.svg"), pointer;
    background: #EA0029;
  }
}
button:disabled {
  background:#BEC5CC;
  box-shadow:none;
}
.input-element {
  margin: 8px 0px;
  padding: 8px;
  width: 100%;
  font-size: 14px;
  font-family: $font-stack;
  border: 1px solid #dfe3e6;
  border-radius: 3px;
}

.input__error {
  color: red;
  text-align: left;
  font-size: 10px;
  margin-top: 3px;
} 

.calculate-btn {
  color: #ea0029;
  border: none;
  background-color: inherit;
  cursor: pointer;
  font-family: $font-stack;
  font-size: 14px;
  font-weight: 500;
  line-height: 24px;
}

.calculate-btn:active {
    color: #ea0029;
  cursor: url("@/assets/img/pointer.svg"), pointer;
}

.calculate-btn:hover {
  color: #F53A31;
  cursor: url("@/assets/img/pointer.svg"), pointer;
}

.calculate {
  border-bottom: 2px solid #dfe3e6;
  &__title {
    font-family: $font-stack;
    font-size: 14px;
    font-weight: 500;
    line-height: 24px;
    padding: 16px 0px;
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
  }
  &__checkbox + label::before {
    content: "";
    display: inline-block;
    width: 1em;
    height: 1em;
    flex-shrink: 0;
    flex-grow: 0;
    border: 1px solid #dfe3e6;
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
    border:1px solid black;
    cursor: url("@/assets/img/pointer.svg"), pointer;
  }

  &__checkbox-label {
    font-family: $font-stack;
    font-size: 14px;
    font-weight: 500;
    line-height: 24px;
  }

  &__interval {
    font-family: $font-stack;
    font-size: 14px;
    font-weight: 500;
    line-height: 24px;
    color: gray;
  }
}

.options {
  margin-bottom: 46px;
  margin-top: 24px;
  &__title {
    display: inline-flex;
    font-family: $font-stack;
    font-size: 14px;
    font-weight: 500;
    line-height: 24px;
    margin-right:32px;
  }

.checkbox-btn {
	display: inline-block;
	margin: 0 16px 0 0;
	user-select: none;
	position: relative;  
}
.checkbox-btn input[type=checkbox] {
	z-index: -1;
	opacity: 0;
	display: block;
	width: 0;
	height: 0;
}
.checkbox-btn span {
	display: inline-block;
	cursor: pointer;
  background: #eef0f2;
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
  padding-top:6px;
}
 
.checkbox-btn input[type=checkbox]:checked + span {
 background: $primary-background;
    width: 73px;
    height: 36px;
    border-radius: 50px;
    border: none;
    color: white;
    font-family: $font-stack;
    font-size: 14px;
    font-weight: normal;
    line-height: 24px;}
 
 
/* Hover */
.checkbox-btn input[type=checkbox]:hover +span {
	background: #DFE3E6;
      border-radius: 50px;
      	color: black;
}
 
/* Disabled */
.checkbox-btn input[type=checkbox]:disabled + span {
	background: #EEF0F2;
	color: black;
	cursor: default;
}
.checkbox-btn input[type=checkbox]:checked:disabled + span {
	background: #EEF0F2;
}
}
</style>
