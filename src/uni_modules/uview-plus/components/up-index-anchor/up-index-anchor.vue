<template>
	<!-- #ifdef APP-NVUE -->
	<header>
	<!-- #endif -->
	<view
	    class="up-index-anchor up-border-bottom"
		:ref="`up-index-anchor-${text}`"
	    :style="{
			height: addUnit(height),
			backgroundColor: bgColor
		}"
	>
		<text
		    class="up-index-anchor__text"
		    :style="{
				fontSize: addUnit(size),
				color: color
			}"
		>{{ text }}</text>
	</view>
	<!-- #ifdef APP-NVUE -->
	</header>
	<!-- #endif -->
</template>

<script>
	import { props } from './props.js';
	import { mpMixin } from '../../libs/mixin/mpMixin.js';
	import { mixin } from '../../libs/mixin/mixin.js';
	import { addUnit, $parent, error } from '../../libs/function/index.js';
	// #ifdef APP-NVUE
	const dom = uni.requireNativePlugin('dom')
	// #endif
	/**
	 * IndexAnchor 列表锚点
	 * @description 
	 * @tutorial https://uview-plus.jiangruyi.com/components/indexList.html
	 * @property {String | Number}	text	列表锚点文本内容
	 * @property {String}			color	列表锚点文字颜色 ( 默认 '#606266' )
	 * @property {String | Number}	size	列表锚点文字大小，单位默认px ( 默认 14 )
	 * @property {String}			bgColor	列表锚点背景颜色 ( 默认 '#dedede' )
	 * @property {String | Number}	height	列表锚点高度，单位默认px ( 默认 32 )
	 * @example <up-index-anchor :text="indexList[index]"></up-index-anchor>
	 */
	export default {
		name: 'up-index-anchor',
		mixins: [mpMixin, mixin, props],
		data() {
			return {
			}
		},
		mounted() {
			this.init()
		},
		methods: {
			addUnit,
			init() {
				// 此处会活动父组件实例，并赋值给实例的parent属性
				const indexList = $parent.call(this, 'up-index-list')
				if (!indexList) { 
					return error('up-index-anchor必须要搭配up-index-list组件使用')
				}
				// 将当前实例放入到up-index-list中
				indexList.anchors.push(this)
				const indexListItem = $parent.call(this, 'up-index-item')
				// #ifndef APP-NVUE
				// 只有在非nvue下，up-index-anchor才是嵌套在up-index-item中的
				if (!indexListItem) {
					return error('up-index-anchor必须要搭配up-index-item组件使用')
				}
				// 设置up-index-item的id为anchor的text标识符，因为非nvue下滚动列表需要依赖scroll-view滚动到元素的特性
				indexListItem.id = this.text.charCodeAt(0)
				// #endif
			}
		},
	}
</script>

<style lang="scss" scoped>
	@import "../../libs/css/components.scss";

	.up-index-anchor {
		position: sticky;
		top: 0;
		@include flex;
		align-items: center;
		padding-left: 15px;
		z-index: 1;

		&__text {
			@include flex;
			align-items: center;
		}
	}
</style>
