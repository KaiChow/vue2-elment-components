<template>
  <div
    :class="[
      type === 'textarea' ? 'el-textarea' : 'el-input',
      {
        'is-disabled': inputDisabled,
        'el-input--prefix': $slots.prefix || prefixIcon,
        'el-input--suffix':
          $slots.suffix || suffixIcon || clearable || showPassword,
      },
    ]"
    @mouseenter="hovering = true"
    @mouseleave="hovering = false"
  >
    <template v-if="type !== 'textarea'">
      <input
        class="el-input__inner"
        ref="input"
        v-bind="$attrs"
        :readonly="readonly"
        :disabled="inputDisabled"
      />
      <!-- 后置部分 -->
      <span class="el-input__suffix" v-if="getSuffixVisible()">
        <span class="el-input__suffix-inner">
          <template v-if="!showClear">
            <slot name="suffix"></slot>
            <i class="el-input__icon" v-if="suffixIcon" :class="suffixIcon">
            </i>
          </template>
          <i
            v-if="showClear"
            class="el-input__icon el-icon-circle-close el-input__clear"
            @mousedown.prevent
            @click="clear"
          ></i>
        </span>
      </span>
    </template>
    <template v-else>
      <textarea
        v-bind="$attrs"
        :readonly="readonly"
        :disabled="inputDisabled"
      />
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
  computed: {
    nativeInputValue() {
      return this.value === null || this.value === undefined
        ? ""
        : String(this.value);
    },
    inputDisabled() {
      return this.disabled || (this.elForm || {}).disabled;
    },
    showClear() {
      return (
        this.clearable &&
        !this.inputDisabled &&
        !this.readonly &&
        (this.focused || this.hovering)
      );
    },
  },
  mounted() {
    this.setNativeInputValue();
  },
  watch: {
    // value(val) {
    //   this.$nextTick(this.resizeTextarea);
    //   if (this.validateEvent) {
    //     this.dispatch("ElFormItem", "el.form.change", [val]);
    //   }
    // },
    // native input value is set explicitly
    // do not use v-model / :value in template
    // see: https://github.com/ElemeFE/element/issues/14521
    nativeInputValue() {
      this.setNativeInputValue();
    },
    // when change between <input> and <textarea>,
    // update DOM dependent value and styles
    // https://github.com/ElemeFE/element/issues/14857
    type() {
      this.$nextTick(() => {
        this.setNativeInputValue();
        this.resizeTextarea();
        this.updateIconOffset();
      });
    },
  },
  methods: {
    getInput() {
      return this.$refs.input || this.$refs.textarea;
    },
    getSuffixVisible() {
      return this.showClear;
    },
    focus() {
      this.getInput().focus();
    },
    blur() {
      this.getInput().blur();
    },
    handleFocus(event) {
      this.focused = true;
      this.$emit("focus", event);
    },
    clear() {
      this.setNativeInputValue();
      this.$emit("input", "");
      this.$emit("change", "");
      this.$emit("clear");
    },
    setNativeInputValue() {
      // eslint-disable-next-line no-debugger
      const input = this.getInput();
      if (!input) return;
      if (input.value === this.nativeInputValue) return;
      input.value = this.nativeInputValue;
    },
    handleInput(event) {
      // should not emit input during composition
      // see: https://github.com/ElemeFE/element/issues/10516
      if (this.isComposing) return;

      // hack for https://github.com/ElemeFE/element/issues/8548
      // should remove the following line when we don't support IE
      if (event.target.value === this.nativeInputValue) return;

      this.$emit("input", event.target.value);

      // ensure native input value is controlled
      // see: https://github.com/ElemeFE/element/issues/12850
      this.$nextTick(this.setNativeInputValue);
    },
    handleChange(event) {
      this.$emit("change", event.target.value);
    },
  },
};
</script>
<style scoped lang="less"></style>
