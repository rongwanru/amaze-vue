<template>
    <label :class="computedClass">
        <input
            class="am-radio-origin"
            type="radio"
            v-model="selfVal"
            :disabled="isDisabled"
            :value="label"
            @focus="focusHandle"
            :checked="value"
        />
        <span class="am-ucheck-icons">
            <i class="am-icon-unchecked"></i>
            <i class="am-icon-checked"></i>
        </span>
        <slot></slot>
        <template v-if="!$slots.default">{{ label }}</template>
    </label>
</template>

<script>
    import Emitter from '../../../mixins/emitter';

    export default {
        name: 'am-radio',
        mixins: [Emitter],
        props: {
            value: {},
            customClass: {
                type: String
            },
            label: {
                type: [String, Number, Boolean]
            },
            disabled: {
                type: Boolean,
                default: false
            },
            color: {
                type: String,
                validate(value) {
                    return ['secondary', 'success', 'warning', 'danger'].includes(value);
                }
            },
            inline: {
                type: Boolean,
                default: true
            }
        },
        data() {
            return {
                checked: false
            }
        },
        methods: {
            focusHandle() {
                this.selfVal = this.label;
            }
        },
        watch:{
            selfVal(curVal, oldVal) {
                if (curVal === this.label) {
                    this.checked = true;
                }
                else {
                    this.checked = false;
                }
            }
        },
        computed: {
            computedClass() {
                const classes = [];

                classes.push('am-radio');
                // classes.push('am-needsclick');

                if (this.checked) {
                    classes.push('am-radio-checked');
                }

                if (this.isDisabled) {
                    classes.push('am-radio-disabled');
                }

                if (this.color !== undefined) {
                    classes.push('am-' + this.color);
                }

                if (this.inline) {
                    classes.push('am-radio-inline');
                }

                return classes.join(' ');
            },
            isGroup() {
                let parent = this.$parent;
                while (parent) {
                    if (parent.$options.name !== 'am-radio-group') {
                        parent = parent.$parent;
                    }
                    else {
                        this.radioGroup = parent;
                        return true;
                    }
                }
                return false;
            },
            selfVal: {
                get() {
                    return this.isGroup ? this.radioGroup.value : this.value;
                },
                set(val) {
                    if (this.isGroup) {
                        this.dispatch('am-radio-group', 'input', [val]);
                    } else {
                        this.$emit('input', val);
                    }
                }
            },
            isDisabled() {
                return this.isGroup
                    ? (this.radioGroup.disabled || this.disabled)
                    : this.disabled;
            }
        }
    }
</script>
