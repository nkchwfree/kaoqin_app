<template>
	<view class="basement">
		<uni-forms ref="form" :modelValue="formData" :rules="rules" label-position="top">
			<uni-forms-item label="申请人 :" name="name" label-position="left">
				<input type="text" disabled v-model="formData.name" />
			</uni-forms-item>
			<!-- <uni-icons type="cart"></uni-icons> -->
			</uni-forms-item>
			<uni-forms-item label="开始时间:" name="name">
				<uni-datetime-picker :border='false' return-type="timestamp" v-model="formData.starTime" type="datetime"
					start="2023-03-01" end="2023-06-01" :hide-second="true" @change="timestampChange"
					placeholder="请选择开始时间" />
			</uni-forms-item>
			<uni-forms-item label="结束时间:" name="name">
				<uni-datetime-picker :border='false' v-model="formData.endTime" return-type="timestamp"
					start="2023-03-01" end="2023-06-01" :hide-second="true" @change="timestampChange"
					placeholder="请选择结束时间" />
			</uni-forms-item>
			<uni-forms-item label="时长(时):" name="duration">
				<input class="uni-input" :value="formData.duration" disabled type="text" placeholder="" />
			</uni-forms-item>


			<uni-forms-item label="加班事由:" name="reason">
				<input class="uni-input" v-model="formData.reason" type="text" placeholder="请填写加班事由" />
			</uni-forms-item>

			<uni-forms-item label="附件 :" name="name">
				<view>
					<uni-file-picker fileMediatype="image" :image-styles="imageStyles" />
					<!-- <uni-file-picker fileMediatype="fli" :list-styles="listStyles"/> -->
				</view>
			</uni-forms-item>
			<uni-forms-item label="审批人:" name="approval">

			</uni-forms-item>
			<button @click="submit" type="primary" class="submitBtn">提交 </button>
		</uni-forms>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				form: "",
				dataList: "", //假期类型
				chooseType: "", //选中假期类型
				formData: {
					name: 'LiMing',
					reason: '',
					starTime: '',
					endTime: '',
					adderss: '',
					duration: '',
					approval: ''
				},
				rules: {
					// 对name字段进行必填验证
					name: {
						rules: [{
								required: true,
								errorMessage: '请输入姓名',
							},
							{
								minLength: 3,
								maxLength: 5,
								errorMessage: '姓名长度在 {minLength} 到 {maxLength} 个字符',
							}
						]
					},
					// 对email字段进行必填验证
					email: {
						rules: [{
							format: 'email',
							errorMessage: '请输入正确的邮箱地址',
						}]
					},
				},
				imageStyles: {
					width: 98,
					height: 98,
					border: {
						width: 2,
						style: 'dashed',
						radius: '2px'
					}
				},
			}
		},
		methods: {
			submit() {
				this.$refs.form.submit().then(res => {
					console.log('表单数据信息：', res);
				}).catch(err => {
					console.log('表单错误信息：', err);
				})
			},
			timestampChange(e) {
				let self = this
				console.log('picker发送选择改变，携带值为', e)
				//this.index = e.target.value
				// if (self.formData.starTime && self.formData.endTime) {
				// 	if (self.formData.starTime >= self.formData.endTime) {
				// 		alert("开始时间必须小于结束时间")
				// 	}
				// }
				setTimeout(function(){
					if (self.formData.starTime && self.formData.endTime) {
						let hours = Math.floor((self.formData.endTime-self.formData.starTime)/1000/900)*0.25
						self.formData.duration = hours
					}
					console.log(self.formData.endTime-self.formData.starTime,"x")
					
					console.log(self.formData.starTime, self.formData.endTime,"y")
				},10)
				
			},

		}
	}
</script>

<style scoped>
	.basement {
		width: 100%;
		height: 100%;
		padding: 30upx;
		box-sizing: border-box;
	}

	/* input {
		border: 0;
		border-color: #fff;
	} */
	/deep/ .uni-forms-item__inner {
		padding: 0;
		margin: 0;
		border-bottom: 1px solid #eee;
	}

	/deep/.uni-forms-item__content {
		display: flex;
		align-items: center;

	}

	.rules {
		width: 100%;
		display: flex;
		color: #ccc;
		justify-content: space-between;
		align-items: center;
	}

	.submitBtn {
		margin-top: 18px;
	}
</style>
