<template>
	<view>
		<!-- 按钮 -->
		<button :class="['buttonBorder',!_rotate?'dlbutton':'dlbutton_loading']" :style="{'background':bgColor, 'color': fontColor}">
			<view :class="_rotate?'rotate_loop':''">
				<text v-if="_rotate" class="cuIcon cuIcon-loading1 "></text>
				<text v-if="!_rotate">{{ text }}</text>
			</view>
		</button>
	</view>
</template>

<script>
	export default{
		props:{
			text: String, //显示文本
			rotate:{
				//是否启动加载
				type: [Boolean,String],
				default: false,
			}, 
			bgColor:{
				//按钮背景颜色
				type: String,
				default: "linear-gradient(to right, rgba(0,0,0,0.7), rgba(0,0,0,0.6))",
			},
			fontColor:{
				//按钮字体颜色
				type: String,
				default: "#FFFFFF",
			},
		},
		computed:{
			_rotate() {
				//处理值
				return String(this.rotate) !== 'false'
			},
		}
	}
</script>

<style lang="scss">
	.dlbutton {
		display: flex;
		justify-content: center;
		align-items: center;
		color: #FFFFFF;
		font-size: 30rpx;
		width:601rpx;
		height:100rpx;
		background:linear-gradient(to right, rgba(0,0,0,0.7), rgba(0,0,0,0.6));
		box-shadow:0rpx 0rpx 13rpx 0rpx rgba(164,217,228,0.4);
		border-radius:2.5rem;
		margin-top: 0rpx;
	}
	.dlbutton_loading {
		display: flex;
		justify-content: center;
		align-items: center;
		color: #FFFFFF;
		font-size: 30rpx;
		width:100rpx;
		height:100rpx;
		background:linear-gradient(to right, rgba(0,0,0,0.7), rgba(0,0,0,0.6));
		box-shadow:0rpx 0rpx 13rpx 0rpx rgba(164,217,228,0.4);
		border-radius:2.5rem;
		margin-top: 0rpx;
	}
	.buttonBorder{
	    border: none ;
	    border-radius: 2.5rem ;
	    -webkit-box-shadow: 0 0 60rpx 0 rgba(0,0,0,.2) ;
	    box-shadow: 0 0 60rpx 0 rgba(0,0,0,.2) ;
	    -webkit-transition: all 0.4s cubic-bezier(.57,.19,.51,.95);
	    -moz-transition: all 0.4s cubic-bezier(.57,.19,.51,.95);
	    -ms-transition: all 0.4s cubic-bezier(.57,.19,.51,.95);
	    -o-transition: all 0.4s cubic-bezier(.57,.19,.51,.95);
	    transition: all 0.4s cubic-bezier(.57,.19,.51,.95);
	}
	
	/* 旋转动画 */
	.rotate_loop{
		-webkit-transition-property: -webkit-transform;
	    -webkit-transition-duration: 1s;
	    -moz-transition-property: -moz-transform;
	    -moz-transition-duration: 1s;
	    -webkit-animation: rotate 1s linear infinite;
	    -moz-animation: rotate 1s linear infinite;
	    -o-animation: rotate 1s linear infinite;
	    animation: rotate 1s linear infinite;
	}
	@-webkit-keyframes rotate{from{-webkit-transform: rotate(0deg)}
	    to{-webkit-transform: rotate(360deg)}
	}
	@-moz-keyframes rotate{from{-moz-transform: rotate(0deg)}
	    to{-moz-transform: rotate(359deg)}
	}
	@-o-keyframes rotate{from{-o-transform: rotate(0deg)}
	    to{-o-transform: rotate(359deg)}
	}
	@keyframes rotate{from{transform: rotate(0deg)}
	    to{transform: rotate(359deg)}
	}
	/* #ifdef MP-ALIPAY */
	
	button
	{
		position: relative;
		/* display: block; */
		margin-left: auto;
		margin-right: auto;
		padding-left: 14px;
		padding-right: 14px;
		box-sizing: border-box;
		font-size: 18px;
		text-align: center;
		text-decoration: none;
		line-height: 2.55555556;
		/* border-radius: 5px; */
		-webkit-tap-highlight-color: transparent;
		overflow: hidden;
		/* color: #000; */
		background-color: #f8f8f8;
		cursor: pointer;
		margin-top: 70rpx !important;
	}
	/* #endif */
</style>
