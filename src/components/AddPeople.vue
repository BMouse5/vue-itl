<template>
    <div class="popup-overlay">
    <div class="popup">
      <div class="close-wrapp">
        <button class="close-btn" @click="closePopup">×</button>
      </div>
      <form @submit.prevent="submitForm">
        <div class="form-control">
          <label for="fio">ФИО:</label>
          <input type="text" v-model="fio" id="fio" name="fio" required />
        </div>
        <div class="form-control">
          <label for="company">Компания:</label>
          <input type="text" v-model="company" id="company" name="company" required />
        </div>
        <div class="form-control">
          <label for="group">Группа:</label>
          <select v-model="group" id="group" name="group" required>
            <option value="Партнер">Партнер</option>
            <option value="Прохожий">Прохожий</option>
          </select>
        </div>
        <div class="form-checkbox">
          <label for="presence">Присутствие:</label>
          <input type="checkbox" v-model="presence" id="presence" name="presence" />
        </div>
        <div class="buttons">
          <button-comp class="btn-green" type="submit">Добавить</button-comp>
          <button-comp class="btn-gray" @click="closePopup">Закрыть</button-comp>
        </div>
      </form>
    </div>
  </div>
  </template>

<script>
import { ref } from 'vue'
import ButtonComp from './ButtonComp.vue'
export default {
  props: {
    showPopup: {
      type: Boolean,
      default: false
    }
  },
  emits: ['close', 'addPerson'],
  setup (_, { emit }) {
    const fio = ref('')
    const company = ref('')
    const group = ref('')
    const presence = ref(false)

    const closePopup = () => {
      emit('close')
    }

    const submitForm = () => {
      const newPerson = {
        fio: fio.value,
        company: company.value,
        group: group.value,
        presence: presence.value
      }
      emit('addPerson', newPerson)
      closePopup()
    }

    return {
      fio,
      company,
      group,
      presence,
      closePopup,
      submitForm
    }
  },
  components: {
    ButtonComp
  }
}
</script>
