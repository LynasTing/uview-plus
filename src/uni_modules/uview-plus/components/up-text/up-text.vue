<template>
    <view
        class="up-text"
        :class="[customClass]"
        v-if="show"
        :style="{
            margin: margin,
			justifyContent: align === 'left' ? 'flex-start' : align === 'center' ? 'center' : 'flex-end'
        }"
        @tap="clickHandler"
    >
        <text
            :class="['up-text__price', type && `up-text__value--${type}`]"
            v-if="mode === 'price'"
            :style="[valueStyle]"
            >￥</text
        >
        <view class="up-text__prefix-icon" v-if="prefixIcon">
            <up-icon
                :name="prefixIcon"
                :customStyle="addStyle(iconStyle)"
            ></up-icon>
        </view>
        <up-link
            v-if="mode === 'link'"			class="up-text__value"
            :style="{fontWeight: valueStyle.fontWeight, wordWrap: valueStyle.wordWrap, fontSize: valueStyle.fontSize}"			:class="[			    type && `up-text__value--${type}`,			    lines && `up-line-${lines}`			]"			:text="value"
            :href="href"
            underLine
        ></up-link>
        <template v-else-if="openType && isMp">
            <button
                class="up-reset-button up-text__value"
                :style="[valueStyle]"
                :data-index="index"
                :openType="openType"
                @getuserinfo="onGetUserInfo"
                @contact="onContact"
                @getphonenumber="onGetPhoneNumber"
                @error="onError"
                @launchapp="onLaunchApp"
                @opensetting="onOpenSetting"
                :lang="lang"
                :session-from="sessionFrom"
                :send-message-title="sendMessageTitle"
                :send-message-path="sendMessagePath"
                :send-message-img="sendMessageImg"
                :show-message-card="showMessageCard"
                :app-parameter="appParameter"
            >
                {{ value }}
            </button>
        </template>
        <text
            v-else
            class="up-text__value"
            :style="[valueStyle]"
            :class="[
                type && `up-text__value--${type}`,
                lines && `up-line-${lines}`
            ]"
            >{{ value }}</text
        >
        <view class="up-text__suffix-icon" v-if="suffixIcon">
            <up-icon
                :name="suffixIcon"
                :customStyle="addStyle(iconStyle)"
            ></up-icon>
        </view>
    </view>
</template>

<script>
import { propsText } from './props.js'
import value from './value.js'
import { mpMixin } from '../../libs/mixin/mpMixin.js';
import { mixin } from '../../libs/mixin/mixin.js';
import { buttonMixin } from '../../libs/mixin/button.js';
import { openType } from '../../libs/mixin/openType.js';
import { addStyle, addUnit, deepMerge } from '../../libs/function/index.js';
/**
 * Text 文本
 * @description 此组件集成了文本类在项目中的常用功能，包括状态，拨打电话，格式化日期，*替换，超链接...等功能。 您大可不必在使用特殊文本时自己定义，text组件几乎涵盖您能使用的大部分场景。
 * @tutorial https://ijry.github.io/uview-plus/components/loading.html
 * @property {String} 					type		主题颜色
 * @property {Boolean} 					show		是否显示（默认 true ）
 * @property {String | Number}			text		显示的值
 * @property {String}					prefixIcon	前置图标
 * @property {String} 					suffixIcon	后置图标
 * @property {String} 					mode		文本处理的匹配模式 text-普通文本，price-价格，phone-手机号，name-姓名，date-日期，link-超链接
 * @property {String} 					href		mode=link下，配置的链接
 * @property {String | Function} 		format		格式化规则
 * @property {Boolean} 					call		mode=phone时，点击文本是否拨打电话（默认 false ）
 * @property {String} 					openType	小程序的打开方式
 * @property {Boolean} 					bold		是否粗体，默认normal（默认 false ）
 * @property {Boolean} 					block		是否块状（默认 false ）
 * @property {String | Number} 			lines		文本显示的行数，如果设置，超出此行数，将会显示省略号
 * @property {String} 					color		文本颜色（默认 '#303133' ）
 * @property {String | Number} 			size		字体大小（默认 15 ）
 * @property {Object | String} 			iconStyle	图标的样式 （默认 {fontSize: '15px'} ）
 * @property {String} 					decoration	文字装饰，下划线，中划线等，可选值 none|underline|line-through（默认 'none' ）
 * @property {Object | String | Number}	margin		外边距，对象、字符串，数值形式均可（默认 0 ）
 * @property {String | Number} 			lineHeight	文本行高
 * @property {String} 					align		文本对齐方式，可选值left|center|right（默认 'left' ）
 * @property {String} 					wordWrap	文字换行，可选值break-word|normal|anywhere（默认 'normal' ）
 * @event {Function} click  点击触发事件
 * @example <up-text text="我用十年青春,赴你最后之约"></up-text>
 */
export default {
    name: 'up-text',
    // #ifdef MP
    mixins: [mpMixin, mixin, value, buttonMixin, openType, propsText],
    // #endif
    // #ifndef MP
    mixins: [mpMixin, mixin, value, propsText],
    // #endif
	emits: ['click'],
    computed: {
        valueStyle() {
            const style = {
                textDecoration: this.decoration,
                fontWeight: this.bold ? 'bold' : 'normal',
                wordWrap: this.wordWrap,
                fontSize: addUnit(this.size)
            }
            !this.type && (style.color = this.color)
            this.isNvue && this.lines && (style.lines = this.lines)
            this.lineHeight &&
                (style.lineHeight = addUnit(this.lineHeight))
            !this.isNvue && this.block && (style.display = 'block')
            return deepMerge(style, addStyle(this.customStyle))
        },
        isNvue() {
            let nvue = false
            // #ifdef APP-NVUE
            nvue = true
            // #endif
            return nvue
        },
        isMp() {
            let mp = false
            // #ifdef MP
            mp = true
            // #endif
            return mp
        }
    },
    data() {
        return {}
    },
    methods: {
        addStyle,
        clickHandler() {
            // 如果为手机号模式，拨打电话
            if (this.call && this.mode === 'phone') {
                uni.makePhoneCall({
                    phoneNumber: this.text
                })
            }
            this.$emit('click')
        }
    }
}
</script>

<style lang="scss" scoped>
@import '../../libs/css/components.scss';

.up-text {
    @include flex(row);
    align-items: center;
    flex-wrap: nowrap;
    flex: 1;
	/* #ifndef APP-NVUE */
	width: 100%;
	/* #endif */

    &__price {
        font-size: 14px;
        color: $up-content-color;
    }

    &__value {
        font-size: 14px;
        @include flex;
        color: $up-content-color;
        flex-wrap: wrap;
        // flex: 1;
        text-overflow: ellipsis;
        align-items: center;

        &--primary {
            color: $up-primary;
        }

        &--warning {
            color: $up-warning;
        }

        &--success {
            color: $up-success;
        }

        &--info {
            color: $up-info;
        }

        &--error {
            color: $up-error;
        }

        &--main {
            color: $up-main-color;
        }

        &--content {
            color: $up-content-color;
        }

        &--tips {
            color: $up-tips-color;
        }

        &--light {
            color: $up-light-color;
        }
    }
}
</style>
