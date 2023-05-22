<template>
  <div
    :class="[type === 'textarea' ? 'el-textarea' : 'el-input']"
    @mouseenter="hovering = true"
    @mouseleave="hovering = false"
  >
    <template v-if="type !== 'textarea'">
      <input
        class="el-input__inner"
        ref="input"
        v-bind="$attrs"
        :readonly="readonly"
      />
    </template>
    <template v-else>
      <textarea v-bind="$attrs" :readonly="readonly" />
    </template>
  </div>
</template>

<script>
export default {
  name: "ElInput",
  componentName: "ElInput",
  inheritAttrs: false,
  props: {
    value: [String, Number],
    size: String,
    resize: String,
    form: String,
    disabled: Boolean,
    readonly: Boolean,
    type: {
      type: String,
      default: "text",
    },
    autosize: {
      type: [Boolean, Object],
      default: false,
    },
    autocomplete: {
      type: String,
      default: "off",
    },
    /** @Deprecated in next major version */
    autoComplete: {
      type: String,
      validator() {
        process.env.NODE_ENV !== "production" &&
          console.warn(
            "[Element Warn][Input]'auto-complete' property will be deprecated in next major version. please use 'autocomplete' instead."
          );
        return true;
      },
    },
    validateEvent: {
      type: Boolean,
      default: true,
    },
    suffixIcon: String,
    prefixIcon: String,
    label: String,
    clearable: {
      type: Boolean,
      default: false,
    },
    showPassword: {
      type: Boolean,
      default: false,
    },
    showWordLimit: {
      type: Boolean,
      default: false,
    },
    tabindex: String,
  },
  data() {
    return {
      textareaCalcStyle: {},
      hovering: false,
      focused: false,
      isComposing: false,
      passwordVisible: false,
    };
  },

  components: {},
  mounted() {},
  methods: {
    getInput() {
      return this.$refs.input || this.$refs.textarea;
    },
    focus() {
      this.getInput().focus();
    },
    blur() {
      this.getInput().blur();
    },
  },
};
</script>
<style scoped lang="less"></style>
