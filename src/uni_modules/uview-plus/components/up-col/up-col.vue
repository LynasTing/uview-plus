<template>
	<view
	    class="up-col"
		ref="up-col"
	    :class="[
			'up-col-' + span
		]"
	    :style="[colStyle]"
	    @tap="clickHandler"
	>
		<slot></slot>
	</view>
</template>

<script>
	import { propsCol } from './props.js';
	import { mpMixin } from '../../libs/mixin/mpMixin.js';
	import { mixin } from '../../libs/mixin/mixin.js';
	import { addStyle, addUnit, deepMerge, getPx } from '../../libs/function/index.js';
	/**
	 * CodeInput 栅格系统的列 
	 * @description 该组件一般用于Layout 布局 通过基础的 12 分栏，迅速简便地创建布局
	 * @tutorial https://ijry.github.io/uview-plus/components/Layout.html
	 * @property {String | Number}	span		栅格占据的列数，总12等份 (默认 12 ) 
	 * @property {String | Number}	offset		分栏左边偏移，计算方式与span相同 (默认 0 ) 
	 * @property {String}			justify		水平排列方式，可选值为`start`(或`flex-start`)、`end`(或`flex-end`)、`center`、`around`(或`space-around`)、`between`(或`space-between`)  (默认 'start' ) 
	 * @property {String}			align		垂直对齐方式，可选值为top、center、bottom、stretch (默认 'stretch' ) 
	 * @property {String}			textAlign	文字水平对齐方式 (默认 'left' ) 
	 * @property {Object}			customStyle	定义需要用到的外部样式
	 * @event {Function}	click	col被点击，会阻止事件冒泡到row
	 * @example	 <up-col  span="3" offset="3" > <view class="demo-layout bg-purple"></view> </up-col>
	 */
	export default {
		name: 'up-col',
		mixins: [mpMixin, mixin, propsCol],
		data() {
			return {
				width: 0,
				parentData: {
					gutter: 0
				},
				gridNum: 12
			}
		},
		//  微信小程序中 options 选项
		// #ifdef MP-WEIXIN
		options: {
			virtualHost: true // 将自定义节点设置成虚拟的，更加接近Vue组件的表现。我们不希望自定义组件的这个节点本身可以设置样式、响应 flex 布局等
		},
		// #endif
		computed: {
			uJustify() {
				if (this.justify == 'end' || this.justify == 'start') return 'flex-' + this.justify
				else if (this.justify == 'around' || this.justify == 'between') return 'space-' + this.justify
				else return this.justify
			},
			uAlignItem() {
				if (this.align == 'top') return 'flex-start'
				if (this.align == 'bottom') return 'flex-end'
				else return this.align
			},
			colStyle() {
				const style = {
					// 这里写成"padding: 0 10px"的形式是因为nvue的需要
					paddingLeft: addUnit(getPx(this.parentData.gutter)/2),
					paddingRight: addUnit(getPx(this.parentData.gutter)/2),
					alignItems: this.uAlignItem,
					justifyContent: this.uJustify,
					textAlign: this.textAlign,
					// #ifndef APP-NVUE
					// 在非nvue上，使用百分比形式
					flex: `0 0 ${100 / this.gridNum * this.span}%`,
					marginLeft: 100 / 12 * this.offset + '%',
					// #endif
					// #ifdef APP-NVUE
					// 在nvue上，由于无法使用百分比单位，这里需要获取父组件的宽度，再计算得出该有对应的百分比尺寸
					width: addUnit(Math.floor(this.width / this.gridNum * Number(this.span))),
					marginLeft: addUnit(Math.floor(this.width / this.gridNum * Number(this.offset))),
					// #endif
				}
				return deepMerge(style, addStyle(this.customStyle))
			}
		},
		mounted() {
			this.init()
		},
		emits: ["click"],
		methods: {
			async init() {
				// 支付宝小程序不支持provide/inject，所以使用这个方法获取整个父组件，在created定义，避免循环引用
				this.updateParentData()
				this.width = await this.parent.getComponentWidth()
			},
			updateParentData() {
				this.getParentData('up-row')
			},
			clickHandler(e) {
				this.$emit('click');
			}
		},
	}
</script>

<style lang="scss" scoped>
	@import "../../libs/css/components.scss";

	.up-col {
		padding: 0;
		/* #ifndef APP-NVUE */
		box-sizing:border-box;
		/* #endif */
		/* #ifdef MP */
		display: block;
		/* #endif */
	}

	// nvue下百分比无效
	/* #ifndef APP-NVUE */
	.up-col-0 {
		width: 0;
	}

	.up-col-1 {
		width: calc(100%/12);
	}

	.up-col-2 {
		width: calc(100%/12 * 2);
	}

	.up-col-3 {
		width: calc(100%/12 * 3);
	}

	.up-col-4 {
		width: calc(100%/12 * 4);
	}

	.up-col-5 {
		width: calc(100%/12 * 5);
	}

	.up-col-6 {
		width: calc(100%/12 * 6);
	}

	.up-col-7 {
		width: calc(100%/12 * 7);
	}

	.up-col-8 {
		width: calc(100%/12 * 8);
	}

	.up-col-9 {
		width: calc(100%/12 * 9);
	}

	.up-col-10 {
		width: calc(100%/12 * 10);
	}

	.up-col-11 {
		width: calc(100%/12 * 11);
	}

	.up-col-12 {
		width: calc(100%/12 * 12);
	}

	/* #endif */
</style>
