<template>
  <h3>Add new transaction</h3>
  <form id="form" @submit.prevent="onSubmit">
    <div class="form-control">
      <label for="text">Text</label>
      <input type="text" id="text" placeholder="Enter text..." v-model="text" />
    </div>
    <div class="form-control">
      <label for="amount">Amount <br /> </label>
      <input type="number" id="amount" placeholder="Enter amount..." v-model="value" />
    </div>
    <button class="btn">Add transaction</button>
  </form>
</template>

<script setup>
import { ref } from 'vue'
import { useToast } from 'vue-toastification'
const toast = useToast()
const submitTransaction = defineEmits(['submitTransaction'])
const text = ref('')
const value = ref('')
const onSubmit = () => {
  if (text.value !== '' && value.value !== '') {
    const obj = {
      id: Math.random(),
      text: text.value,
      amount: value.value
    }
    submitTransaction('submitTransaction', obj)
    toast.success('Sucessfully')
    text.value = ''
    value.value = ''
  } else {
    toast.error('Empty')
  }
}
</script>
