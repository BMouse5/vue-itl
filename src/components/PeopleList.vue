<template>
  <div class="list">
    <table>
      <thead>
        <tr>
          <th>Номер</th>
          <th>ФИО</th>
          <th>Компания</th>
          <th>Группа</th>
          <th>Присутствие</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(person, index) in people" :key="index" @click="openEditModal(person, index)">
          <td>{{ index + 1 }}</td>
          <td>{{ person.fio }}</td>
          <td>{{ person.company }}</td>
          <td>{{ person.group }}</td>
          <td>
            <span :class="['circle', person.presence ? 'green' : 'red']"></span>
          </td>
        </tr>
      </tbody>
    </table>
    <edit-person
      v-if="isEditModalOpen"
      :showPopup="isEditModalOpen"
      :person="selectedPerson"
      @close="closeEditModal"
      @updatePerson="updatePerson"
    />
  </div>
</template>

<script>
import EditPerson from './EditPerson.vue'

export default {
  props: {
    people: {
      type: Array,
      required: true
    }
  },
  components: {
    EditPerson
  },
  data () {
    return {
      isEditModalOpen: false,
      selectedPerson: null
    }
  },
  methods: {
    openEditModal (person, index) {
      this.selectedPerson = { ...person }
      this.selectedPersonIndex = index
      this.isEditModalOpen = true
    },
    closeEditModal () {
      this.isEditModalOpen = false
    },
    updatePerson (updatedPerson) {
      if (this.selectedPersonIndex !== null) {
        this.$emit('updatePerson', { index: this.selectedPersonIndex, updatedPerson })
      }
      this.isEditModalOpen = false
    }
  }
}
</script>
