<template>
	<view :class="['uni-collapse-cell', { 'uni-collapse-cell--disabled': disabled, 'uni-collapse-cell--open': isOpen }]" :hover-class="disabled ? '' : 'uni-collapse-cell--hover'">
		<view class="uni-collapse-cell__title header" @click="onClick">
			<view v-if="thumb" class="uni-collapse-cell__title-extra">
				<image :src="thumb" class="uni-collapse-cell__title-img" />
			</view>
			<view class="uni-collapse-cell__title-inner">
				<view class="uni-collapse-cell__title-text">{{ title }}</view>
			</view>
			<view :class="{ 'uni-active': isOpen, 'uni-collapse-cell--animation': showAnimation === true }" class="uni-collapse-cell__title-arrow">
				<uni-icons color="#000000" size="20" type="arrowdown" style="font-weight:bold" />
			</view>
		</view>
		<view :style="{ height: isOpen ? 'auto' : '0px' }" class="uni-collapse-cell__content">
			<view :class="{ 'uni-collapse-cell--animation': showAnimation === true }" :style="{ transform: isOpen ? 'translateY(0px)' : 'translateY(-50%)','-webkit-transform' : isOpen ? 'translateY(0px)' : 'translateY(-50%)' }">
				<slot />
			</view>
		</view>
	</view>
</template>

<script>
	export default {
		name: 'UniCollapseItem',
		components: {
			
		},
		props: {
			title: {
				// 列表标题
				type: String,
				default: ''
			},
			name: {
				// 唯一标识符
				type: [Number, String],
				default: 0
			},
			disabled: {
				// 是否禁用
				type: [Boolean, String],
				default: false
			},
			showAnimation: {
				// 是否显示动画
				type: Boolean,
				default: false
			},
			open: {
				// 是否展开
				type: [Boolean, String],
				default: false
			},
			thumb: {
				// 缩略图
				type: String,
				default: ''
			}
		},
		data() {
			return {
				isOpen: false
			}
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
		methods: {
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
				this.$forceUpdate()
			}
		}
	}
</script>

<style lang="scss">
	@charset "UTF-8";

	.uni-collapse-cell {
		position: relative
	}

	.uni-collapse-cell--hover {
		/* background-color: rgba(255,255,255,1); */
	}

	.uni-collapse-cell--open {
		/* background-color: rgba(255,255,255,1); */
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
		/* height: 1px; */
		content: '';
		-webkit-transform: scaleY(.5);
		transform: scaleY(.5);
		background-color: #e5e5e5
	}

	.uni-collapse-cell__title {
		padding: 24rpx 30rpx;
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
		margin-right: 18rpx;
		display: flex;
		flex-direction: row;
		justify-content: center;
		align-items: center
	}

	.uni-collapse-cell__title-img {
		height: 52rpx;
		width: 52rpx
	}

	.uni-collapse-cell__title-arrow {
		width: 20px;
		height: 20px;
		transform: rotate(0);
		transform-origin: center center
	}

	.uni-collapse-cell__title-arrow.uni-active {
		transform: rotate(180deg)
	}

	.uni-collapse-cell__title-inner {
		flex: 1;
		overflow: hidden;
		display: flex;
		flex-direction: column
	}

	.uni-collapse-cell__title-text {
		text-overflow: ellipsis;
		white-space: nowrap;
		line-height: 1.5;
		overflow: hidden;
		font-size:28rpx;
		font-family:PingFangHK-Regular,PingFangHK;
		font-weight:bold;
		color:rgba(51,51,51,1);
	}

	.uni-collapse-cell__content {
		position: relative;
		width: 100%;
		overflow: hidden
	}

	.uni-collapse-cell__content .view {
		font-size: 28rpx
	}
</style>