<template>
    <div :class="[`${prefixCls}`]" @mouseenter="handleShowPopper" @mouseleave="handleClosePopper">
        <div :class="[`${prefixCls}-rel`]" v-el:reference>
            <slot></slot>
        </div>
        <div :class="[`${prefixCls}-popper`]" transition="fade" v-el:popper v-show="!disabled && showPopper">
            <div :class="[`${prefixCls}-content`]">
                <div :class="[`${prefixCls}-arrow`]"></div>
                <div :class="[`${prefixCls}-inner`]"><slot name="content">{{ content }}</slot></div>
            </div>
        </div>
    </div>
</template>
<script>
    import Popper from '../base/popper';
    import { oneOf } from '../../utils/assist';

    const prefixCls = 'ivu-tooltip';

    export default {
        mixins: [Popper],
        props: {
            placement: {
                validator (value) {
                    return oneOf(value, ['top', 'top-start', 'top-end', 'bottom', 'bottom-start', 'bottom-end', 'left', 'left-start', 'left-end', 'right', 'right-start', 'right-end']);
                },
                default: 'bottom'
            },
            content: {
                type: [String, Number],
                default: ''
            },
            delay: {
                type: Number,
                default: 0
            },
            disabled: {
                type: Boolean,
                default: false
            }
        },
        data () {
            return {
                prefixCls: prefixCls
            }
        },
        methods: {
            handleShowPopper() {
                this.timeout = setTimeout(() => {
                    this.showPopper = true;
                }, this.delay);
            },
            handleClosePopper() {
                clearTimeout(this.timeout);
                this.showPopper = false;
            }
        }
    }
</script>