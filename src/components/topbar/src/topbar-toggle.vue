<template>
    <am-button
        @click="tigger"
        customClass="am-topbar-btn am-topbar-toggle am-show-sm-only"
        :color="color"
        size="sm">
        <slot></slot>
        <icon type="bars"></icon>
    </am-button>
</template>

<script>
    import { Button } from '../../button';
    import { Icon } from '../../icon';

    export default {
        name: 'am-topbar-toggle',
        props: {
            color: {
                type: String,
                default: 'primary',
                validator(value) {
                    return [
                        'default',
                        'primary',
                        'secondary',
                        'success',
                        'warning',
                        'danger'
                    ].includes(value);
                }
            }
        },
        methods: {
            tigger() {
                if (this.collapse) {
                    this.collapse.autoFold();
                }
            }
        },
        computed: {
            collapse() {
                let parent = this.$parent;
                while (parent) {
                    parent = parent.$parent;
                    if (parent.$options.name === 'am-topbar') {
                        break;
                    }
                }


                let childNode = parent.$children;

                function findNode(childNodes) {
                    let node = null;

                    childNodes.every((vNode) => {
                        if (vNode.$options.name === 'am-topbar-collapse') {
                            node = vNode;
                            return  false;
                        }

                        if (vNode.$children) {
                            node = findNode(vNode.$children);
                        }

                        return !node;
                    });

                    return node;
                }

                return findNode(parent.$children);
            }
        },
        components: {
            AmButton: Button,
            Icon
        }
    }
</script>
