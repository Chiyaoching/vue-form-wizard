<template>
  <div v-show="active" class="wizard-tab-container"
       role="tabpanel"
       :id="title"
       :aria-hidden="!active"
       :aria-labelledby="title">
    <slot :active="active">
    </slot>
  </div>
</template>
<script>
  export default{
    name: 'tab-content',
    props: {
      title: {
        type: String,
        default: ''
      },
      /***
       * Icon name for the upper circle corresponding to the tab
       */
      icon: {
        type: String,
        default: ''
      },
      /***
       * Function to execute before tab switch. Return value must be boolean
       * If the return result is false, tab switch is restricted
       */
      beforeChange: {
        type: Function
      },
      route: {
        type: [String, Object]
      },
      additionalInfo: {
        type: Object,
        default: () => {}
      }
    },
    inject: ['addTab', 'removeTab'],
    data () {
      return {
        active: false,
        validationError: null,
        checked: false
      }
    },
    computed: {
      shape () {
        return this.$parent.shape
      },
      color () {
        return this.$parent.color
      },
      errorColor () {
        return this.$parent.errorColor
      }
    },
    mounted () {
      this.addTab(this)
    },
    destroyed () {
      if (this.$el && this.$el.parentNode) {
        this.$el.parentNode.removeChild(this.$el)
      }
      this.removeTab(this)
    }
  }
</script>
