<template>
    <div class="popup-overlay" v-if="showPopup">
      <div class="popup">
        <div class="close-wrapp">
          <button class="close-btn" @click="closePopup">×</button>
        </div>
        <form @submit.prevent="submitForm">
          <div class="form-control">
            <label for="fio">ФИО:</label>
            <input type="text" v-model="editedPerson.fio" id="fio" name="fio" required />
          </div>
          <div class="form-control">
            <label for="company">Компания:</label>
            <input type="text" v-model="editedPerson.company" id="company" name="company" required />
          </div>
          <div class="form-control">
            <label for="group">Группа:</label>
            <select v-model="editedPerson.group" id="group" name="group" required>
              <option value="Партнер">Партнер</option>
              <option value="Прохожий">Прохожий</option>
            </select>
          </div>
          <div class="form-checkbox">
            <label for="presence">Присутствие:</label>
            <input type="checkbox" v-model="editedPerson.presence" id="presence" name="presence" />
          </div>
          <div class="buttons">
            <button-comp class="btn-green" type="submit">Сохранить</button-comp>
            <button-comp class="btn-gray" @click="closePopup">Закрыть</button-comp>
          </div>
        </form>
      </div>
    </div>
  </template>
<script>
import { ref, watch } from 'vue'
import ButtonComp from './ButtonComp.vue'
export default {
  props: {
    showPopup: {
      type: Boolean,
      default: false
    },
    person: {
      type: Object,
      required: true
    }
  },
  emits: ['close', 'updatePerson'],
  setup (props, { emit }) {
    const editedPerson = ref({ ...props.person })
    watch(props, (newProps) => {
      editedPerson.value = { ...newProps.person }
    })
    const closePopup = () => {
      emit('close')
    }
    const submitForm = () => {
      emit('updatePerson', editedPerson.value)
      closePopup()
    }
    return {
      editedPerson,
      closePopup,
      submitForm
    }
  },
  components: {
    ButtonComp
  }
}
</script>
