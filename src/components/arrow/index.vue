<template functional>
  <component :is="props.tag"
    class="vx-arrow"
    :style="Object.assign({
      height: props.size,
      width: props.size,
      borderColor: {
        up: `${props.color} transparent transparent ${props.color}`,
        right: `${props.color} ${props.color} transparent transparent`,
        down: `transparent ${props.color} ${props.color} transparent`,
        left: `transparent transparent ${props.color} ${props.color}`
      }[props.direction],
    }, props.styles)"
    >
  </component>
</template>
<script>
export default {
  props: {
    direction: {
      type: String,
      default: 'right'
    },
    styles: {
      type: Object
    },
    size: {
      type: String,
      default: '0.2rem'
    },
    color: {
      type: String,
      default: '#999'
    },
    tag: {
      type: String,
      default: 'i'
    }
  }
}
</script>
