<template>
  <li class='re-submenu' @click.stop='handleClick' :class="{'is-opened': isActive}">
    <div class="re-menu-title">
      <slot name="title"></slot>
      <i class="re-arrow" :class="{'is-active': isActive}"></i>
    </div>
    <slider-transition>
      <ul class="re-menu" v-show="isActive">
        <slot></slot>
      </ul>
    </slider-transition>
  </li>
</template>

<script>
import sliderTransition from '../transition/slider'

export default {
  name: 'Submenu',
  inject: ['rootMenu'],
  data () {
    return {
      isActive: false
    }
  },
  mounted () {
    this.$on('item-click', (name) => {
      this.isActive = true
    })
  },
  components: {
    sliderTransition
  },
  methods: {
    handleClick () {
      this.$emit('click', this)
      if (this.rootMenu.uniqueOpened) {
        this.$parent.$children.map(item => {
          if (item !== this) {
            item.isActive = false
          }
        })
      }
      this.isActive = !this.isActive
    }
  }
}
</script>
