<template>
  <div :class="classes" :disabled="disabled">
    <slot></slot>
  </div>
</template>

<script>
import { input } from 'utils/mixins.js'
import Radio from './Radio'

export default {
  componentName: 'RadioGroup',
  mixins: [input],
  components: {
    Radio
  },
  props: {
    divider: {
      type: Boolean,
      default: true
    }
  },
  computed: {
    classes () {
      return ['vx-radio-group', this.divider ? 'vx-radio-group-divider' : '']
    }
  },
  methods: {
    handleChange (value) {
      this.$emit('input', value).$emit('change', value)
    }
  }
}
</script>
