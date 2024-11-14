<template>
  <button :class="['btn', buttonClass]" @click="$emit('click')">
    <slot/>
  </button>
</template>

<script>
import { computed } from 'vue'

export default {
  props: {
    color: {
      type: String,
      default: 'btn'
    }
  },
  setup (props) {
    const buttonClass = computed(() => {
      return props.color
    })

    return {
      buttonClass
    }
  }
}
</script>
