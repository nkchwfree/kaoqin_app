<template>
	<view :class="['uni-collapse-cell',{'uni-collapse-cell--disabled':disabled,'uni-collapse-cell--open':isOpen}]" :hover-class="disabled ? '' : 'uni-collapse-cell--hover'">
		<view class="uni-collapse-cell__title" @click="onClick">
			<view class="uni-collapse-cell__title-extra" v-if="thumb">
				<image class="uni-collapse-cell__title-img" :src="thumb"></image>
			</view>
			<view class="uni-collapse-cell__title-inner">
				<view class="uni-collapse-cell__title-text">{{title}}</view>
			</view>
			<view class="uni-collapse-cell__title-arrow" :class="{'uni-active':isOpen,'uni-collapse-cell--animation':showAnimation===true}">
				<!-- <uni-icon color="#bbb" size="20" type="arrowdown"></uni-icon> -->
			</view>
		</view>
		<view class="uni-collapse-cell__content" :class="{'uni-collapse-cell--animation':showAnimation===true}" :style="{height:isOpen ? height : '0px'}">
			<view :id="elId">
				<slot />
			</view>
		</view>
	</view>
</template>

<script>
	import uniIcon from '../uni-icon/uni-icon.vue'
	export default {
		name: 'uni-collapse-item',
		components: {
			uniIcon
		},
		props: {
			title: { //列表标题
				type: String,
				default: ''
			},
			name: { //唯一标识符
				type: [Number, String],
				default: 0
			},
			disabled: { //是否禁用
				type: [Boolean, String],
				default: false
			},
			showAnimation: { //是否显示动画
				type: Boolean,
				default: false
			},
			open: { //是否展开
				type: [Boolean, String],
				default: false
			},
			thumb: { //缩略图
				type: String,
				default: ''
			}
		},
		data() {
			const elId = `Uni_${Math.ceil(Math.random() * 10e5).toString(36)}`
			return {
				isOpen: false,
				height: 'auto',
				elId: elId
			};
		},
		watch: {
			open(val) {
				this.isOpen = val
			}
		},
		inject: ['collapse'],
		created() {
			this.isOpen = this.open
			this.nameSync = this.name ? this.name : this.collapse.childrens.length
			this.collapse.childrens.push(this)
			if (String(this.collapse.accordion) === 'true') {
				if (this.isOpen) {
					let lastEl = this.collapse.childrens[this.collapse.childrens.length - 2]
					if (lastEl) {
						this.collapse.childrens[this.collapse.childrens.length - 2].isOpen = false
					}
				}
			}
		},
		// #ifdef H5
		mounted() {
			this.getSize()
		},
		// #endif
		// #ifndef H5
		onReady() {
			this.getSize()
		},
		// #endif
		methods: {
			getSize() {
				if (this.showAnimation) {
					uni.createSelectorQuery().in(this).select(`#${this.elId}`).boundingClientRect().exec((ret) => {
						this.height = ret[0].height + 'px'
					});
				}
			},
			onClick() {
				if (this.disabled) {
					return
				}
				if (String(this.collapse.accordion) === 'true') {
					this.collapse.childrens.forEach(vm => {
						if (vm === this) {
							return
						}
						vm.isOpen = false
					})
				}
				this.isOpen = !this.isOpen
				this.collapse.onChange && this.collapse.onChange()
			}
		}
	}
</script>

<style>
	@charset "UTF-8";

	.uni-collapse-cell {
		position: relative
	}

	.uni-collapse-cell--hover {
		background-color: #f1f1f1
	}

	.uni-collapse-cell--open {
		background-color: #f1f1f1
	}

	.uni-collapse-cell--disabled {
		opacity: .3
	}

	.uni-collapse-cell--animation {
		transition: all .3s
	}

	.uni-collapse-cell:after {
		position: absolute;
		z-index: 3;
		right: 0;
		bottom: 0;
		left: 0;
		height: 1px;
		content: '';
		-webkit-transform: scaleY(.5);
		transform: scaleY(.5);
		background-color: #c8c7cc
	}

	.uni-collapse-cell__title {
		padding: 24upx 30upx;
		width: 100%;
		box-sizing: border-box;
		flex: 1;
		position: relative;
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center
	}

	.uni-collapse-cell__title-extra {
		margin-right: 18upx;
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center
	}

	.uni-collapse-cell__title-img {
		height: 52upx;
		width: 52upx
	}

	.uni-collapse-cell__title-arrow {
		width: 20px;
		height: 20px;
		transform: rotate(0);
		transform-origin: center center
	}

	.uni-collapse-cell__title-arrow.uni-active {
		transform: rotate(-180deg)
	}

	.uni-collapse-cell__title-inner {
		flex: 1;
		overflow: hidden;
		display: flex;
		flex-direction: column
	}

	.uni-collapse-cell__title-text {
		font-size: 32upx;
		text-overflow: ellipsis;
		white-space: nowrap;
		color: inherit;
		line-height: 1.5;
		overflow: hidden
	}

	.uni-collapse-cell__content {
		position: relative;
		width: 100%;
		overflow: hidden;
		background: #fff
	}

	.uni-collapse-cell__content view {
		font-size: 28upx
	}
</style>