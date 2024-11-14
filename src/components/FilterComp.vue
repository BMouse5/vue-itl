<template>
    <div class="filters">
      <div class="filters-title">
        <h3>Фильтровать по:</h3>
        <div class="filters-value">
          <button
            class="value"
            :class="{ active: activeFilter === 'missing' }"
            @click="setActiveFilter('missing')"
          >
            Отсутствующим
          </button>
          <button
            class="value"
            :class="{ active: activeFilter === 'present' }"
            @click="setActiveFilter('present')"
          >
            Присутствующим
          </button>
          <button
            class="value"
            :class="{ active: activeFilter === 'none' }"
            @click="setActiveFilter('none')"
          >
            Без фильтра
          </button>
        </div>
      </div>
    </div>
  </template>
<script>
import { ref } from 'vue'
export default {
  emits: ['filterChanged'],
  setup (_, { emit }) {
    const activeFilter = ref('none')
    const setActiveFilter = (filter) => {
      activeFilter.value = filter
      emit('filterChanged', filter)
    }
    return {
      activeFilter,
      setActiveFilter
    }
  }
}
</script>
