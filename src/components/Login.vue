<template>
	<div class = "login_container">
		<!-- 登陆组件 -->
		<div class="login_box">
			<!-- 头像 -->
			<div class="avater_box">
				<img src="../assets/logo.png">
			</div>
			<!-- 登录表单 -->
			<el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules" label-width="0px" class="login_form">
				<el-form-item prop="staffId">
					<el-input v-model="loginForm.staffId" prefix-icon="el-icon-user"></el-input>
				</el-form-item>
				<el-form-item prop="password">
					<el-input v-model="loginForm.password" prefix-icon="el-icon-lock" type="password"></el-input>
				</el-form-item>
				<el-form-item class="btns" >
					<el-button type="primary" @click="login">登录</el-button>
					<el-button type="info">注册</el-button>
				</el-form-item>
			</el-form>
		</div>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				loginForm: {
					staffId:'',
					password:''
				},
				//验证规则
				loginFormRules: {
				staffId: [
					{ required: true, message:"请输入用户名", trigger: "blur"}
				],
				password: [
					{ required: true, message:"请输入密码", trigger: "blur"}
				]
			}
		}
		},
		methods: {
			login() {
				this.$refs.loginFormRef.validate(async valid =>{
					if (!valid) return;
					/* 配置请求路径，对接口 */
					const {data: res} = await this.$http.post('/api1/login/logins',this.loginForm)
					console.log(res,"www");
					/* 配置状态码 */
					if(res.code !==0) return this.$message.error('用户名或密码错误！');
					this.$message.success('登录成功！');
					//将返回的token保存到客户端的sessionStorage中
					window.sessionStorage.setItem("name",res.data.data.staffName);
					window.sessionStorage.setItem("id",res.data.data.id);
					window.sessionStorage.setItem("departmentId",res.data.data.departmentId);
					window.sessionStorage.setItem("token",res.data.token);
					//window.sessionStorage.setItem("token",res.data.token);
					//页面跳转 
					this.$router.push("/welcome");
				} );
			}
		}
	
}
</script>
<!--  -->
<style scoped>
	.login_container {
		background-color: #2b4b6b;
		height: 100%;
	}
	.login_box{
		width: 450px;
		height: 300px;
		background-color: #fff;
		border-radius: 3px;
		position: absolute;
		left: 50%;
		top:50%;
		transform: translate(-50%,-50%);
		.avater_box{
			height: 130px;
			width: 130px;
			border: 1px solid #eee;
			border-radius: 50%;
			padding: 10px;
			box-shadow: 0 0 10px #ddd;
			position: absolute;
			left: 50%;
			transform: translate(-50%,-50%);
			background-color: #fff;
			img{
				width: 100%;
				height: 100%;
				border-radius: 50%;
				background-color: #eee;
			}
		}
	}
	.login_form{
		position: absolute;
		bottom: 0;
		width: 100%;
		padding: 0 20px;
		box-sizing: border-box;
	}
	.btns {
		display: flex;
		justify-content: flex-end;
	}
</style>
