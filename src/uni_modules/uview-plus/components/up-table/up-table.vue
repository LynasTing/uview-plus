<template>
	<view class="up-table">
		
	</view>
</template>

<script>
	import { props } from './props.js';
	import { mpMixin } from '../../libs/mixin/mpMixin.js';
	import { mixin } from '../../libs/mixin/mixin.js';
	/**
	 * Table 表格 
	 * @description 表格组件一般用于展示大量结构化数据的场景 本组件标签类似HTML的table表格，由table、tr、th、td四个组件组成
	 * @tutorial https://ijry.github.io/uview-plus/components/table.html
	 * @example <up-table><up-tr><up-th>学校</up-th </up-tr> <up-tr><up-td>浙江大学</up-td> </up-tr> <up-tr><up-td>清华大学</up-td> </up-tr></up-table>
	 */
	export default {
		name: 'up-table',
		mixins: [mpMixin, mixin, props],
		data() {
			return {
				
			}
		}
	}
</script>

<style lang="scss" scoped>
	@import "../../libs/css/components.scss";
	
</style>
