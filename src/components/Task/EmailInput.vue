<template>
  <div class="email-input-container">
    <label>
      <input
        type="text"
        v-model.trim="userEmail"
        :class="inputClass"
        placeholder="Enter your email"
        @blur="onBlur"
        @focus="inputClass = null"
      />
      <span v-if="modelModifiers.auto">{{ domain }}</span>
    </label>
  </div>
</template>

<script>
const pattern = /^[a-zA-Z0-9._%+-]+$/

export default {
  name: 'EmailInput',
  props: {
    domain: {
      type: String,
      default: ''
    },
    modelValue: {
      type: String
    },
    modelModifiers: {
      default: () => ({})
    }
  },
  data() {
    return {
      inputClass: null
    }
  },
  methods: {
    onBlur() {
      if (this.userEmail && this.modelModifiers.check && !pattern.test(this.userEmail)) {
        this.inputClass = 'red'
          }
      else {
        this.inputClass = null
      }
    }
  },
  computed: {
    userEmail: {
      get() {
        if (this.modelModifiers.auto) return this.modelValue.split('@')[0]
        return this.modelValue
      },
      set(newValue) {
        if (this.modelModifiers.auto && this.domain)
          newValue = newValue + this.domain
        this.$emit('update:modelValue', newValue)
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.email-input-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin: 1rem 0;
}

label {
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

input {
  padding: 0.5rem;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 1rem;
  transition: border-color 0.3s ease;

  &:focus {
    border-color: #2c3e50;
    outline: none;
  }
}

.red {
  background-color: red;
}
</style>
