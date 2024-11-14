<template>
  <div>
    <header-comp @click="openPopup" :present="present" :missing="missing" @searchInput="updateSearchTerm"></header-comp>
  </div>
  <main>
    <people-list :people="filteredPeople" @updatePerson="handlePersonUpdate"></people-list>
    <add-people v-if="isPopupOpen" :showPopup="isPopupOpen" @close="closePopup" @addPerson="addPerson"></add-people>
  </main>
  <footer>
    <filter-comp @filterChanged="applyFilter"></filter-comp>
  </footer>
</template>

<script>
import AddPeople from './components/AddPeople.vue'
import HeaderComp from './components/HeaderComp.vue'
import PeopleList from './components/PeopleList.vue'
import FilterComp from './components/FilterComp.vue'
import { ref, computed } from 'vue'

export default {
  setup () {
    const isPopupOpen = ref(false)
    const people = ref([])
    const searchTerm = ref('')

    const loadPeopleFromStorage = () => {
      const storedPeople = localStorage.getItem('people')
      if (storedPeople) {
        people.value = JSON.parse(storedPeople)
      }
    }

    const savePeopleToStorage = () => {
      localStorage.setItem('people', JSON.stringify(people.value))
    }

    const present = computed(() => people.value.filter(person => person.presence).length)

    const missing = computed(() => people.value.length - present.value)

    const filter = ref('none')

    const filteredPeople = computed(() => {
      let filtered = people.value

      if (searchTerm.value) {
        filtered = filtered.filter(person => person.fio.toLowerCase().includes(searchTerm.value.toLowerCase()))
      }

      if (filter.value === 'present') {
        filtered = filtered.filter(person => person.presence)
      } else if (filter.value === 'missing') {
        filtered = filtered.filter(person => !person.presence)
      }

      return filtered
    })

    const openPopup = () => {
      isPopupOpen.value = true
    }

    const closePopup = () => {
      isPopupOpen.value = false
    }

    const addPerson = newPerson => {
      people.value.push(newPerson)
      savePeopleToStorage()
    }

    const updateSearchTerm = term => {
      searchTerm.value = term
    }

    const applyFilter = selectedFilter => {
      filter.value = selectedFilter
    }

    const handlePersonUpdate = ({ index, updatedPerson }) => {
      people.value.splice(index, 1, updatedPerson)
      savePeopleToStorage()
    }
    loadPeopleFromStorage()

    return {
      isPopupOpen,
      openPopup,
      closePopup,
      addPerson,
      people,
      present,
      missing,
      filteredPeople,
      applyFilter,
      updateSearchTerm,
      handlePersonUpdate
    }
  },
  components: {
    HeaderComp,
    PeopleList,
    AddPeople,
    FilterComp
  }
}
</script>
