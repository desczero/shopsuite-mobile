<template>
	<view class="main-list oBorder">
		<!-- 国家区号 -->
		<picker v-if="_isShowIntl" mode="selector" @change="onIntlChange" :value="mode_index" :range="_lang" range-key="name">
			<view class="country-int-code" style="color:#000000">+ {{country_code}}</view>
		</picker>
		<!-- 文本框 -->
		<input
			class="main-input"
			:value="value"
			:type="_type"
			:maxlength="maxlength"
			:placeholder="placeholder"
			:password="type==='password'&&!showPassword"
			@input="onInput"
		/>
		<!-- 是否可见密码 -->
		<image
			v-if="_isShowPass&&type==='password'&&!_isShowCode"
			class="img cuIcon"
			:class="showPassword?'cuIcon-attention':'cuIcon-attentionforbid'"
			@tap="showPass"
		></image>
		<!-- 倒计时 -->
		<view
			v-if="_isShowCode&&!_isShowPass&&!_isShowCodeImg"
			:class="['vercode',{'vercode-run': second>0}]"
			@click="setCode"
		>{{ getVerCodeSecond }}</view>


		<view
			v-if="_isShowCode&&!_isShowPass&&_isShowCodeImg"
			:class="['vercode',{'vercode-run': second>0}]"
			@click="refresh"
		>
			<view style="line-height: 0px;">
				<imgcode ref="imgcode"></imgcode>
			</view>
		</view>
	</view>
</template>

<script>
	import imgcode from '../../components/verify-code/imgcode.vue';
	
	var _this, countDown;
	export default{
		data(){
			return{
				showPassword: false, //是否显示明文
				second: 0, //倒计时
				isRunCode: false, //是否开始倒计时

				mode_index:0,
				country_code:86,
			}
		},
		components: {
            imgcode
		},
		props:{
			type: String, //类型
			value: String, //值
			placeholder: String, //框内提示
			maxlength: {
				//最大长度
				type: [Number,String],
				default: 50,
			},
			isShowIntl:{
				//是否显示国家区号（二选一）
				type: [Boolean,String],
				default: false,
			},
			isShowPass:{
				//是否显示密码图标（二选一）
				type: [Boolean,String],
				default: false,
			},
			isShowCodeImg:{
				//是否显示获取验证码（二选一）
				type: [Boolean,String],
				default: false,
			},
			isShowCode:{
				//是否显示获取验证码（二选一）
				type: [Boolean,String],
				default: false,
			},
			codeText:{
				type: String,
				default: "获取验证码",
			},
			setTime:{
				//倒计时时间设置
				type: [Number,String],
				default: 60,
			}
		},
		model: {
			prop: 'value',
			event: 'input'
		},
		mounted() {
			_this=this
			//准备触发
			this.$on('runCode',(val)=>{
                this.runCode(val);
            });
			clearInterval(countDown);//先清理一次循环，避免缓存
			
			if (this._isShowCodeImg)
			{
				this.$refs.imgcode.refresh();
			}

		},
		methods:{
			showPass(){
				//是否显示密码
				this.showPassword = !this.showPassword
			},
			onInput(e) {
				//传出值
				this.$emit('input', e.target.value)
			},
			/* start 图形验证码 */
			refresh:function(){
			    this.$refs.imgcode.refresh();
			},
			setCode(){
				//设置获取验证码的事件
				if(this.isRunCode){
					//判断是否开始倒计时，避免重复点击
					return false;
				}
				this.$emit('setCode')
				
			},
			runCode(val){
				//开始倒计时
				if(String(val)=="0"){

					//判断是否需要终止循环
					this.second = 0; //初始倒计时
					clearInterval(countDown);//清理循环
					this.isRunCode= false; //关闭循环状态
					return false;
				}
				if(this.isRunCode){
					//判断是否开始倒计时，避免重复点击
					return false;
				}
				this.isRunCode= true
				this.second = this._setTime //倒数秒数

				let _this=this;
				countDown = setInterval(function(){
					_this.second--
					if(_this.second==0){
						_this.isRunCode= false
						clearInterval(countDown)
					}
				},1000)
			},
			onIntlChange: function(e){
				var that = this;
				var index = e.detail.value
				this.setData({
					mode_index:e.detail.value,
					country_code:this._lang[index].id,
				})
				
				//传出值
				this.$emit('intl', '+' + this._lang[index].id)
			}
		},
		computed:{
			_type(){
				//处理值
				const type = this.type
				return type == 'password' ? 'text' : type
			},
			_isShowIntl() {
				//处理值
				return String(this.isShowIntl) !== 'false'
			},
			_lang(){
				let lists = [];
				
				for(let idx in this.Lang.data.items)
				{
					lists.push({
							id:this.Lang.data.items[idx].currency_id,
							name:this.Lang.data.items[idx].label + '(+' + this.Lang.data.items[idx].currency_id + ')'
					})
					
					if (idx == this.mode_index)
					{
						this.country_code = this.Lang.data.items[idx].currency_id;
						this.$emit('intl', '+' + this.country_code)
					}
				}
				
				return lists;
			},
			_isShowPass() {
				//处理值
				return String(this.isShowPass) !== 'false'
			},
			
			_isShowCode() {
				//处理值
				return String(this.isShowCode) !== 'false'
			},
			
			_isShowCodeImg() {
				//处理值
				return String(this.isShowCodeImg) !== 'false'
			},
			
			_setTime() {
				//处理值
				const setTime = Number(this.setTime)
				return setTime>0 ? setTime : 60
			},
			getVerCodeSecond(){
				//验证码倒计时计算
				if(this.second<=0){
					return this.codeText;
				}else{
					if(this.second<10){
						return '0'+this.second;
					}else{
						return this.second;
					}
				}

			}
		}
	}
</script>

<style lang="scss">
	.main-list{
		display: flex;
		flex-direction: row;
		justify-content: space-between;
		align-items: center;
		height: 36rpx;   /* Input 高度 */
		color: #333333;
		padding: 32rpx;
		margin-top:24rpx;
		margin-bottom: 24rpx;
	}
	.img{
		width: 32rpx;
		height: 32rpx;
		font-size: 32rpx;
		line-height: 32rpx;
	}
	.main-input{
		flex: 1;
		text-align: left;
		font-size: 28rpx;
		/* line-height: 100rpx; */
		padding-right: 10rpx;
		margin-left: 20rpx;
	}
	.vercode {
		color: rgba(0,0,0,0.7);
		font-size: 24rpx;
		line-height: 100rpx;
	}
	.vercode-run {
		color: rgba(0,0,0,0.4) !important;
	}
	.oBorder{
	    border: none;
	    border-radius: 2.5rem ;
	    -webkit-box-shadow: 0 0 60rpx 0 rgba(43,86,112,.1) ;
	    box-shadow: 0 0 60rpx 0 rgba(43,86,112,.1) ;
	}
	
	.country-int-code{
		background:transparent
	}
</style>
