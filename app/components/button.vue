<template>
  <component
    :is="tag"
    class="s-lk-mobile-app-button"
    :class="classes"
    :to="to"
    :href="href || to"
    v-bind="$attrs"
  >
    <slot />
  </component>
</template>
  
<script>
  export default {
    name: 'SLkMobileAppButton',
    props: {
      to: {
        type: [String, Object],
        default: null
      },
      href: {
        type: String,
        default: null
      },
      secondary: {
        type: Boolean,
        default: false
      },
      disabled: {
        type: Boolean,
        default: false
      },
      outlined: {
        type: Boolean,
        default: false
      },
      noBorder: {
        type: Boolean,
        default: false
      },
      noUppercase: {
        type: Boolean,
        default: false
      }
    },
    computed: {
      tag() {
        return (this.to || this.href) && !this.disabled
          ? (typeof this.to !== 'object' && this.to.match('^https?:\\/\\/.+$')) ||
            this.href
            ? 'a'
            : 'nuxt-link'
          : 'div';
      },
      classes() {
        return {
          secondary: this.secondary,
          disabled: this.disabled,
          outlined: this.outlined,
          'no-border': this.noBorder,
          'no-uppercase': this.noUppercase
        };
      }
    }
  };
</script>
  
<style lang="scss">
  .s-lk-mobile-app-button {
    cursor: pointer;
    display: inline-flex;
    align-items: center;
    justify-content: center;
    color: #fff;
    font-size: 16px;
    font-weight: 600;
    line-height: 19px;
    text-transform: uppercase;
    background: #fe6702;
    padding: 15px 40px;
    border-radius: 100px;
    width: 100%;
  
    &.secondary {
      color: #fe6702;
      background: transparent;
      padding: 14px 39px;
      border: 1px solid #fe6702;
    }
  
    &.no-border {
      color: #fe6702;
      background: transparent;
      padding: 0;
      border: none;
  
      &.secondary {
        color: #9696a0;
      }
    }
  
    &.no-uppercase {
      text-transform: none;
    }
  
    &.disabled {
      cursor: auto;
      color: #69686f;
      background: #e6e5ea;
      padding: 14px 39px;
  
      &.secondary {
        color: #bebebe;
        background: transparent;
      }
  
      &.no-border {
        padding: 0;
        border: none;
      }
    }
  }
</style>
  