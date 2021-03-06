<template>
  <popup :class="classes" :open="open" :history="history" :fast-close="fastClose" :direction="myDirection" @close="handleClosePopup" @close-after="handleCloseAfter" @enter="handleEnter">
    <div :class="['vx-actionsheet-inner']" onselectstart="return false;">
      <div v-if="title" :class="['vx-actionsheet-title']">
        {{title}}
      </div>
      <div :class="['vx-actionsheet-items']">
        <slot></slot>
      </div>
      <div v-if="cancel" :class="'vx-actionsheet-cancel'" @click="handleClose">
        {{cancelText}}
      </div>
    </div>
  </popup>
</template>

<script>
import Popup from '../popup'
export default {
  componentName: 'Actionsheet',
  components: {
    Popup
  },
  props: {
    type: {
      type: String,
      default: 'default'
    },
    open: {
      type: Boolean,
      default: false
    },
    history: {
      type: Boolean,
      default: true
    },
    value: {
      type: [String, Number]
    },
    cancel: {
      type: Boolean,
      default: false
    },
    title: {
      type: String
    },
    cancelText: {
      type: String,
      default: '取消'
    },
    fastClose: {
      type: Boolean,
      default: true
    }
  },
  computed: {
    classes () {
      return ['vx-actionsheet', this.type === 'menu' ? 'vx-actionsheet-menu' : '']
    },
    myDirection () {
      if (this.type === 'default') {
        return 'bottom'
      } else if (this.type === 'menu') {
        return 'center'
      }
    }
  },
  mounted () {
    if (this.open) {
      requestAnimationFrame(() => {
        this.$el.style.display = 'block'
      })
    }
  },
  watch: {
    open (value) {
      if (value) {
        requestAnimationFrame(() => {
          this.$el.style.display = 'block'
          this.$emit('open')
        })
      } else {
        setTimeout(() => {
          requestAnimationFrame(() => {
            this.$el.style.display = 'none'
          })
        }, 300)
      }
    }
  },
  methods: {
    handleAction (value) {
      this.$emit('update:open', false).$emit('action', value)
    },
    handleClose () {
      this.$emit('update:open', false).$emit('close')
    },
    handleCloseAfter () {
      this.$emit('close-after')
    },
    handleClosePopup () {
      this.$emit('update:open', false).$emit('close')
    },
    handleEnter () {
      if (!this.$children) return
      requestAnimationFrame(() => {
        this.$children[0].$children.forEach((item) => {
          if (item.$el.className.indexOf('overlay') === -1) {
            item.value === this.value && (item.checked = true)
            item.$off('action-item').$on('action-item', this.handleAction)
          }
        })
      })
    }
  }
}
</script>
