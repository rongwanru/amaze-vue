<template>
    <form :class="computedClass">
        <slot></slot>
    </form>
</template>

<script>
    export default {
        name: 'am-form',
        props: {
            customClass: String
        },
        computed: {
            computedClass() {
                const classes = [];

                classes.push('am-form');

                if (this.customClass !== undefined) {
                    classes.push(this.customClass);
                }

                return classes.join(' ');
            }
        }
    }
</script>
