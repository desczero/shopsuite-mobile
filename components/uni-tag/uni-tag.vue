<template>
	<view :class="[
      'uni-tag--' + type,
      disabled === true || disabled === 'true' ? 'uni-tag--disabled' : '',
      inverted === true || inverted === 'true' ? type + '-uni-tag--inverted' : '',
      circle === true || circle === 'true' ? 'uni-tag--circle' : '',
      mark === true || mark === 'true' ? 'uni-tag--mark' : (mark === 'right' ? 'uni-tag--mark-right' : ''),
      'uni-tag--' + size
    ]" @click="onClick()" class="uni-tag" v-if="text">
		<text :class="[type === 'default' ? 'uni-tag--default':'uni-tag-text',inverted === true || inverted === 'true' ? 'uni-tag-text--'+type : '',size === 'small' ? 'uni-tag-text--small':'' ]">{{ text }}</text>
	</view>
</template>

<script>
	/**
	 * Tag 标签
	 * @description 用于展示1个或多个文字标签，可点击切换选中、不选中的状态
	 * @tutorial https://ext.dcloud.net.cn/plugin?id=35
	 * @property {String} text 标签内容
	 * @property {String} size = [normal|small] 大小尺寸
	 * 	@value normal 正常
	 * 	@value small 小尺寸
	 * @property {String} type = [default|primary|success｜warning｜error｜royal]  颜色类型
	 * 	@value default 灰色
	 * 	@value primary 蓝色
	 * 	@value success 绿色
	 * 	@value warning 黄色
	 * 	@value error 红色
	 * 	@value royal 紫色
	 * @property {Boolean} disabled = [true|false] 是否为禁用状态
	 * @property {Boolean} inverted = [true|false] 是否无需背景颜色（空心标签）
	 * @property {Boolean} circle = [true|false] 是否为圆角
	 * @event {Function} click 点击 Tag 触发事件
	 */

	export default {
		name: "UniTag",
		props: {
			type: {
				// 标签类型default、primary、success、warning、error、royal
				type: String,
				default: "default"
			},
			size: {
				// 标签大小 normal, small
				type: String,
				default: "normal"
			},
			// 标签内容
			text: {
				type: String,
				default: ""
			},
			disabled: {
				// 是否为禁用状态
				type: [Boolean, String],
				defalut: false
			},
			inverted: {
				// 是否为空心
				type: [Boolean, String],
				defalut: false
			},
			circle: {
				// 是否为圆角样式
				type: [Boolean, String],
				defalut: false
			},
			mark: {
				// 是否为标记样式
				type: [Boolean, String],
				defalut: false
			}
		},
		methods: {
			onClick() {
				if (this.disabled === true || this.disabled === "true") {
					return;
				}
				this.$emit("click");
			}
		}
	};
</script>

<style lang="scss" scoped>
  @import '../../styles/_variables.scss';

	$tag-pd: 0px 16px;
	$tag-small-pd: 0px 8px;

	.uni-tag {
		/* #ifndef APP-NVUE */
		display: flex;
		/* #endif */
		padding: $tag-pd;
		height: 30px;
		line-height: 30px;
		justify-content: center;
		color: $uni-text-color;
		border-radius: $uni-border-radius-base;
		background-color: $uni-bg-color-grey;
		border-width: 1rpx;
		border-style: solid;
		border-color: $uni-bg-color-grey;
	}

	.uni-tag--circle {
		border-radius: 15px;
	}

	.uni-tag--mark {
		border-top-left-radius: 0;
		border-bottom-left-radius: 0;
		border-top-right-radius: 15px;
		border-bottom-right-radius: 15px;
	}



	.uni-tag--mark-right{
		border-top-left-radius: 15px;
		border-bottom-left-radius: 15px;
		border-top-right-radius: 0;
		border-bottom-right-radius: 0;

	}

	.uni-tag--disabled {
		opacity: 0.5;
	}

	.uni-tag--small {
		height: 20px;
		padding: $tag-small-pd;
		line-height: 20px;
		font-size: $uni-font-size-sm;
		text-align: center;
		vert-align: middle;
	}

	.uni-tag--default {
		color: $uni-text-color;
		font-size: $uni-font-size-base;
	}

	.uni-tag-text--small {
		font-size: 12px !important;
	}

	.uni-tag-text {
		color: $uni-text-color-inverse;
		font-size: $uni-font-size-base;
	}

	.uni-tag--default {
		color: $uni-text-color;
		font-size: $uni-font-size-base;
	}

	.uni-tag-text--primary {
		color: $uni-color-primary !important;
	}

	.uni-tag-text--success {
		color: $uni-color-success !important;
	}

	.uni-tag-text--warning {
		color: $uni-color-warning !important;
	}

	.uni-tag-text--error {
		color: $uni-color-error !important;
	}

	.uni-tag--primary {
		color: $uni-text-color-inverse;
		background-color: $uni-color-primary;
		border-width: 1rpx;
		border-style: solid;
		border-color: $uni-color-primary;
	}

	.primary-uni-tag--inverted {
		color: $uni-color-primary;
		background-color: $uni-bg-color;
		border-width: 1rpx;
		border-style: solid;
		border-color: $uni-color-primary;
	}

	.uni-tag--success {
		color: $uni-text-color-inverse;
		background-color: $uni-color-success;
		border-width: 1rpx;
		border-style: solid;
		border-color: $uni-color-success;
	}

	.success-uni-tag--inverted {
		color: $uni-color-success;
		background-color: $uni-bg-color;
		border-width: 1rpx;
		border-style: solid;
		border-color: $uni-color-success;
	}

	.uni-tag--warning {
		color: $uni-text-color-inverse;
		background-color: $uni-color-warning;
		border-width: 1rpx;
		border-style: solid;
		border-color: $uni-color-warning;
	}


    .uni-tag--royal {
        color: #fff;
        background-color: #8a6de9;
        border: 1rpx solid #8a6de9;
    }

    .royal-uni-tag--inverted {
        color: #8a6de9;
        background-color: #FFFFFF;
        border: 1rpx solid #8a6de9;
    }



    .uni-tag--shopsuite {
        color: #fff;
        background-color: $default-skin-bg;
		border-width: 1rpx;
		border-style: solid;
		border-color: $default-skin-bg;
    }

    .shopsuite-uni-tag--inverted {
        color: $default-skin-bg;
        background-color: #FFFFFF;
		border-width: 1rpx;
		border-style: solid;
		border-color: $default-skin-bg;
    }

	.uni-tag-text--shopsuite {
		color: $default-skin-bg !important;
	}


  .uni-tag--ss {
    color: #222222;
    background-color: #f5f5f5;
    border-width: 1rpx;
    border-style: solid;
    border-color: #f5f5f5;
    .uni-tag-text{
      color: $default-skin-bg;
    }
  }

  .ss-uni-tag--inverted {
    color: $default-skin-bg;
    background-color: #FFFFFF;
    border-width: 1rpx;
    border-style: solid;
    border-color: $default-skin-bg;

    .uni-tag-text--ss {
      color: #222222 !important;
    }
  }


    .uni-tag--gray {
        color: #fff;
        background-color: #999999;
        border: 1rpx solid #999999;
    }

    .gray-uni-tag--inverted {
        color: #999999;
        background-color: #FFFFFF;
        border: 1rpx solid #999999;
    }
	.uni-tag-text--gray {
		color: #999999 !important;
	}



	.warning-uni-tag--inverted {
		color: $uni-color-warning;
		background-color: $uni-bg-color;
		border-width: 1rpx;
		border-style: solid;
		border-color: $uni-color-warning;
	}

	.uni-tag--error {
		color: $uni-text-color-inverse;
		background-color: $uni-color-error;
		border-width: 1rpx;
		border-style: solid;
		border-color: $uni-color-error;
	}

	.error-uni-tag--inverted {
		color: $uni-color-error;
		background-color: $uni-bg-color;
		border-width: 1rpx;
		border-style: solid;
		border-color: $uni-color-error;
	}

	.uni-tag--inverted {
		color: $uni-text-color;
		background-color: $uni-bg-color;
		border-width: 1rpx;
		border-style: solid;
		border-color: $uni-bg-color-grey;
	}


  .uni-tag--ss {
    width: 110rpx;
    height: 70rpx;
    margin-right: 16rpx;
    margin-bottom: 4rpx;
    background: #f5f5f5;
    border-radius: 18rpx;
    border: 2rpx solid #f5f5f5;
    font-size: 24rpx;
    font-family: PingFangSC-Regular, PingFang SC;
    font-weight: 400;
    color: #222222 !important;
    line-height: 70rpx;
    text-align: center;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;

    border: 2rpx solid $default-skin-bg;
    background-color: #f7e9e9;
  }

  .ss-uni-tag--inverted {
    background-color: #f5f5f5;
    border: 2rpx solid #f5f5f5;
  }
</style>
