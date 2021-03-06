<template>
    <label class="m-checkbox">
        <template v-if="group">
            <input type="checkbox" v-model="model" :value="val" @change="changeHandler" :disabled="disabled"/>
        </template>
        <template v-else>
            <input type="checkbox" v-model="checked" :disabled="disabled"/>
        </template>

        <span class="checkbox-icon" :style="iconStyles()"><i :style="checkIconStyles()"></i></span>

        <template v-if="!$slots.default">
            <span class="checkbox-text">{{val}}</span>
        </template>
        <template v-else>
            <span class="checkbox-text"><slot></slot></span>
        </template>
    </label>
</template>

<script type="text/babel">
    import {isColor} from '../../../utils/assist';

    export default {
        name: 'yd-checkbox',
        data() {
            return {
                model: [],
                group: false,
                checked: this.value
            }
        },
        props: {
            value: {
                type: Boolean,
                default: false
            },
            val: {
                type: [Boolean, String, Number]
            },
            disabled: {
                type: Boolean,
                default: false
            },
            color: {
                validator(value) {
                    if (!value) return true;
                    return isColor(value);
                },
                default: '#4CD864'
            },
            size: {
                validator(val) {
                    return /^([1-9]\d*)$/.test(val);
                },
                default: 20
            }
        },
        methods: {
            changeHandler () {
                if (this.disabled) return;
                this.$parent.change(this.model);
            },
            iconStyles() {
                const size = (this.group ? this.$parent.size : this.size) + 'px';
                const color = this.group ? this.$parent.color : this.color;

                return {
                    width: size,
                    height: size,
                    color: color
                };
            },
            checkIconStyles() {
                const size = this.group ? this.$parent.size : this.size;

                return {
                    width: Math.round(size / 2.8) + 'px',
                    height: Math.round(size / 1.5) + 'px'
                }
            }
        },
        watch: {
            checked(val) {
                this.$emit('input', val);
            },
            value(val) {
                this.checked = val;
            }
        },
        created() {
            if (this.$parent.color) {
                this.group = true;
            }
        }
    }
</script>

<style lang="less">
    @import '../../../styles/components/checkbox.less';
</style>
