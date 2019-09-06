<template>
  <div class="va-list-group">
    <div class="va-list-group__header" @click="toggle">
      <slot name="anchor"/>
      <va-icon
        class="va-list-group__header-icon"
        :name="show ? 'fa fa-angle-up' : 'fa fa-angle-down'"
      />
    </div>
    <transition
      @beforeEnter="beforeEnter"
      @afterEnter="afterEnter"
      @beforeLeave="beforeLeave"
      @afterLeave="afterLeave"
    >
      <div class="va-list-group__body" v-show="show">
        <slot/>
      </div>
    </transition>
  </div>
</template>

<script>

import VaIcon from '../va-icon/VaIcon'
export default {
  name: 'VaListGroup',
  components: { VaIcon },
  props: {
    sub: Boolean,
  },
  data () {
    return {
      show: false,
    }
  },
  methods: {
    toggle () {
      this.show = !this.show
      this.$emit('input', this.show)
    },
    beforeEnter (el) {
      el.classList.remove('collapse')
      el.style.display = 'block'
      el.classList.add('collapsing')
      el.style.height = `${el.scrollHeight}px`
    },
    afterEnter (el) {
      el.classList.remove('collapsing')
      el.classList.add('collapse', 'in')
    },
    beforeLeave (el) {
      el.classList.add('collapsing')
      el.classList.remove('collapse', 'in')
      el.style.height = 0
    },
    afterLeave (el) {
      el.classList.remove('collapsing')
      el.classList.add('collapse')
      el.style.display = 'none'
    },
    countHeight (el) {
    },
  },
}
</script>

<style lang="scss">
.va-list-group {
  &__header {
    display: flex;
    cursor: pointer;
    &-icon {
      margin-left: auto;
      display: flex;
      align-items: center;
      margin-right: 1rem;
    }
  }
  &__body {
    padding: 0.5rem 1rem 0.5rem 1.5rem;
    .va-item-section--main {
      padding-left: .5rem;
    }
  }
  .collapse {
    display: none;

    &.in {
      display: block;
    }
  }

  .collapsing {
    position: relative;
    height: 0;
    overflow: hidden;
    transition: height .3s ease;
  }

}
</style>
