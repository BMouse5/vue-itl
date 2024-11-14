<template>
  <div class="navbar" @click="$emit('open')">
    <div class="navbar-main">
      <div class="navbar-logo">
        <img src="../assets/logo.png" alt="logo" />
      </div>
      <input type="text" class="navbar-input" placeholder="Поиск по имени" @input="onInput" @click.stop>
      <button-comp color="btn-green">Добавить</button-comp>
    </div>
    <div class="navbar-peoples">
      <h3 class="title">Посетители</h3>
      <h3 class="title">
        <span :class="{'present': present >= 0}">{{ present }}</span> /
        <span :class="{'missing': missing >= 0}">{{ missing }}</span>
      </h3>
    </div>
  </div>
</template>

<script>
import ButtonComp from './ButtonComp.vue'

export default {
  props: {
    present: {
      type: Number,
      required: true
    },
    missing: {
      type: Number,
      required: true
    }
  },
  methods: {
    onInput (event) {
      this.$emit('searchInput', event.target.value)
    }
  },
  components: {
    ButtonComp
  }
}
</script>
