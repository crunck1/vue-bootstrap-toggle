<template>
   <input type="checkbox"/>
</template>

<script>
import merge from 'merge'

if (!jQuery().bootstrapToggle) {
  require('bootstrap-toggle')
}

const defaults = {}

export default {
  defaults,
  props: {
    value: Boolean,
    options: {
      type: Object,
      default: () => ({})
    },
    disabled: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return { updating: false, };
  },
  computed: {
    $$el() {
      return jQuery(this.$el)
    }
  },
  watch: {
    value(newValue) {
      if(this.updating) {
        return;
      }

      this.$$el.bootstrapToggle(newValue ? 'on' : 'off');
    },
    disabled(newValue) {
      this.$$el.bootstrapToggle(newValue ? 'disable' : 'enable');
    }
  },
  mounted() {
    if (this.value) {
      this.$el.checked = true
    }
    this.$$el.bootstrapToggle(merge.recursive(true, defaults, this.options))
    if (this.disabled) { this.$$el.bootstrapToggle('disable') }
    this.$$el.change(() => {
      this.updating = true;
      this.$emit('input', this.$$el);
      this.$nextTick( () => this.updating = false );
    })
  },
  beforeDestroy() {
    this.$$el.bootstrapToggle('destroy')
    this.$$el.off('change')
  }
}
</script>

<style src="bootstrap-toggle/css/bootstrap-toggle.css"/>
