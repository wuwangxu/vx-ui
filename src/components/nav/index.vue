<template>
  <div :class="classes">
    <flexbox class="vx-nav-inner" align="center">
      <button :class="['btn-pull','vx-nav-back']" @click="handleBack" v-if="back!==false">
        <arrow direction="left" :color="arrow.color" :size="arrow.size"/>
      </button>
      <flexbox-item :class="['vx-nav-title', back===false ? 'vx-nav-title-center' : '']">
        <slot name="title"></slot>
      </flexbox-item>
      <slot name="pull"></slot>
    </flexbox>
    <slot></slot>
  </div>
</template>

<script>
import {Flexbox, FlexboxItem} from '../flexbox'
import Arrow from '../arrow'
export default {
  componentName: 'XNav',
  components: {
    Flexbox,
    FlexboxItem,
    Arrow
  },
  props: {
    back: {
      type: [String, Boolean, Function, Object],
      default: true
    },
    backText: {
      type: String,
      default: '返回'
    },
    arrow: {
      type: Object,
      default () {
        return {
          size: '0.24rem',
          color: '#fff'
        }
      }
    }
  },
  computed: {
    classes () {
      return ['vx-nav']
    }
  },
  methods: {
    handleBack () {
      if (this.back === true) {
        history.back()
      } else if (typeof this.back === 'function') {
        this.back()
      } else {
        this.$router.push(this.back)
      }
    }
  }
}
</script>
