<template>
    <up-transition
        :show="loading"
        :custom-style="{
            position: 'fixed',
            top: 0,
            left: 0,
            right: 0,
            bottom: 0,
            backgroundColor: bgColor,
            display: 'flex',
            zIndex: zIndex,
            ...customStyle
        }"
    >
        <view class="up-loading-page">
            <view class="up-loading-page__warpper">
                <view class="up-loading-page__warpper__loading-icon">
                    <image
                        v-if="image"
                        :src="image"
                        class="up-loading-page__warpper__loading-icon__img"
                        mode="widthFit"
						:style="{
							width: addUnit(iconSize),
						    height: addUnit(iconSize)
						}"
                    ></image>
                    <up-loading-icon
                        v-else
                        :mode="loadingMode"
                        :size="addUnit(iconSize)"
                        :color="loadingColor"
                    ></up-loading-icon>
                </view>
                <slot>
                    <text
                        class="up-loading-page__warpper__text"
                        :style="{
                            fontSize: addUnit(fontSize),
                            color: color,
                        }"
                        >{{ loadingText }}</text
                    >
                </slot>
            </view>
        </view>
    </up-transition>
</template>

<script>
import { props } from "./props.js";
import { mpMixin } from '../../libs/mixin/mpMixin.js';
import { mixin } from '../../libs/mixin/mixin.js';
import { addUnit } from '../../libs/function/index.js';
/**
 * loadingPage 加载动画
 * @description 警此组件为一个小动画，目前用在uView的loadmore加载更多和switch开关等组件的正在加载状态场景。
 * @tutorial https://ijry.github.io/uview-plus/components/loading.html
 * @property {String | Number}	loadingText		提示内容  (默认 '正在加载' )
 * @property {String}			image			文字上方用于替换loading动画的图片
 * @property {String}			loadingMode		加载动画的模式，circle-圆形，spinner-花朵形，semicircle-半圆形 （默认 'circle' ）
 * @property {Boolean}			loading			是否加载中 （默认 false ）
 * @property {String}			bgColor			背景色 （默认 '#ffffff' ）
 * @property {String}			color			文字颜色 （默认 '#C8C8C8' ）
 * @property {String | Number}	fontSize		文字大小 （默认 19 ）
 * @property {String | Number}	iconSize		图标大小 （默认 28 ）
 * @property {String}			loadingColor	加载中图标的颜色，只能rgb或者十六进制颜色值 （默认 '#C8C8C8' ）
 * @property {Number}			zIndex	        z-index层级 （默认10 ）
 * @property {Object}			customStyle		自定义样式
 * @example <up-loading mode="circle"></up-loading>
 */
export default {
    name: "up-loading-page",
    mixins: [mpMixin, mixin, props],
    data() {
        return {};
    },
    methods: {
        addUnit
    }
};
</script>

<style lang="scss" scoped>
@import "../../libs/css/components.scss";

$text-color: rgb(200, 200, 200) !default;
$text-size: 19px !default;
$up-loading-icon-margin-bottom: 10px !default;

.up-loading-page {
    @include flex(column);
    flex: 1;
    align-items: center;
    justify-content: center;

    &__warpper {
        margin-top: -150px;
        justify-content: center;
        align-items: center;
        /* #ifndef APP-NVUE */
        color: $text-color;
        font-size: $text-size;
        /* #endif */
        @include flex(column);

        &__loading-icon {
            margin-bottom: $up-loading-icon-margin-bottom;

            &__img {
                width: 40px;
                height: 40px;
            }
        }

        &__text {
            font-size: $text-size;
            color: $text-color;
        }
    }
}
</style>
