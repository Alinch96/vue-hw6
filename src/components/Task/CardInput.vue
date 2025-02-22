<template>
  <div class="card-input-container">
    <template v-if="modelModifiers.divide4digits">
      <label for="cardNumber"> CARD NUMBER </label>
      <input
        class="cardNumber"
        id="cardNumber"
        type="text"
        v-model="cardNumber"
        maxlength="19"
        placeholder="XXXX XXXX XXXX XXXX"
        @blur="onBlur"
      />
    </template>
    <template v-else-if="modelModifiers.expiryDate">
        <div class="date-cvv-container"></div>
      <label for="expiryDate"> EXPIRY DATE </label>
      <input
        class="expiryDate"
        id="expiryDate"
        type="text"
        v-model="expiryDate"
        placeholder="XX/XX"
        maxlength="5"
        @blur="onBlur"
      />
    </template>
    <template v-else-if="modelModifiers.cvv">
      <label for="cvv"> SECURITY CODE </label>
      <input
        class="cvv"
        id="cvv"
        type="text"
        v-model.trim="cvv"
        placeholder="XXX"
        maxlength="3"
        @blur="onBlur"
      />
    </template>
    <span v-if="message" class="message">{{ message }}</span>
  </div>
</template>

<script>
const cardNumberPattern = /^(?:\d{4}[- ]?){3}\d{4}$/
const expiryDatePattern = /^(0[1-9]|1[0-2])\/\d{2}$/
const cvvPattern = /^\d{3}$/

export default {
  name: "CardInput",
  props: {
    modelValue: {
      type: String
    },
    modelModifiers: {
      default: () => ({})
    }
  },
  data() {
    return {
      message: null
    }
  },
  computed: {
    cardNumber: {
      get() {
        return this.modelValue.replaceAll(/(\d{4}(?=\S+))/g, "$1 ")
      },
      set(newValue) {
        this.$emit("update:modelValue", newValue.replaceAll(" ", ""))
      }
    },
    expiryDate: {
      get() {
        return this.modelValue.replace(/(\d{2}(?=\S+))/g, "$1/")
      },
      set(newValue) {
        this.$emit("update:modelValue", newValue.replace("/", ""))
      }
    },
    cvv: {
      get() {
        return this.modelValue
      },
      set(newValue) {
        if (newValue.length > 3) return
        this.$emit("update:modelValue", newValue.replace(" ", ""))
      }
    }
  },
  methods: {
    onBlur() {
      if (
        this.modelModifiers.divide4digits &&
        this.cardNumber &&
        !cardNumberPattern.test(this.cardNumber)
      )
        this.message = "Invalid card number format"
      else if (
        this.modelModifiers.expiryDate &&
        this.expiryDate &&
        !expiryDatePattern.test(this.expiryDate)
      )
        this.message = "Invalid expiry date format"
      else if (
        this.modelModifiers.cvv &&
        this.cvv &&
        !cvvPattern.test(this.cvv)
      )
        this.message = "Invalid cvv format"
      else this.message = null
    }
  }
}
</script>

<style lang="scss" scoped>
.card-input-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 1rem 0;
  
  label {
    font-weight: bold;
    margin-bottom: 0.5rem;
  }

  input {
    padding: 0.5rem;
    border: 1px solid #ddd;
    border-radius: 4px;
    margin-bottom: 0.5rem;
    transition: border-color 0.3s ease;
   
    &:focus {
      border-color: #2c3e50;
      outline: none;
    }
  }

  .cardNumber {
    width: 26ch;
  }
  .expiryDate {
    width: 10ch;
  }
  .cvv {
    width: 8ch;
  }
}

.message {
  color: red;
  font-size: 0.9rem;
}
</style>
