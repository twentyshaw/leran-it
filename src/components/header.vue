<template>
<header v-bind:class="{'no-login':!isLogin,'login':isLogin}">
	<template v-if="!isLogin">
		<div class="nav-bar">
			<div class="logo"><a href="#">LearnIT</a></div>
			<div class="navlist">
				<ul>
					<li><a href="#" data-view="home-top" @click="scrollTo">Home</a></li><li><a href="#" data-view="home-course" @click="scrollTo">Course</a></li><li><a href="#" data-view="home-merit" @click="scrollTo">Programs</a></li><li><a href="#" data-view="home-contact" @click="scrollTo">Contact</a></li>
				</ul>
			</div>
			<div class="login-btn"><a class="btn" href="#" @click="showLogin">SIGN IN</a></div>
		</div>
		<Login class="userLogin"></Login>
	</template>
	<template v-if="isLogin">
		<div class="nav-bar">
			<div class="moto">
				<p>お疲れ様でした。<a href="https://forms.gle/8D1SkiZ3xo9yBtQ96" target="_blank">アンケート</a>の協力お願いいたします。</p>
			</div>
			<div class="user-tag">
				<span class="greet">Hi, <em>{{username}}</em></span>
				<el-dropdown>
				<span class="user-icon el-dropdown-link" :style="avatar"></span>
				<el-dropdown-menu slot="dropdown" style="padding:0;">
					<el-dropdown-item><span @click="toSet">Settings</span></el-dropdown-item>
					<el-dropdown-item>Help</el-dropdown-item>
				</el-dropdown-menu>
				</el-dropdown>
			</div>
		</div>
	</template>
</header>
</template>

<script type="text/javascript">
import Login from '@/components/loginbox'
import { mapGetters, mapActions} from 'vuex'
	export default{
		components:{
			Login
		},
		computed:{
			...mapGetters([
					'user',
					'isLogin',
					'usericon'
				]),
			username(){
				return this.user.username
			},
			avatar(){
				return {
					backgroundImage:"url("+ this.usericon +")", //计算属性依赖this.usericon，当该值改变时计算属性会更新
					backgroundRepeat:"no-repeat",
					backgroundSize:'cover',
					backgroundPosition:'center'
				}
			}
		},
		methods:{
			scrollTo(e){
				e.preventDefault()
				var target = document.querySelector(`#${e.target.dataset.view}`)
				target.scrollIntoView()
			},

			showLogin(e){
				e.preventDefault()
				var box = document.querySelector('.userLogin')
				box.style.display = 'flex'
			},

			toSet(e){
				console.log(this.$router)
				this.$router.push({path:'/user/setting'})
			}
		}
	}
</script>

<style scoped>
/* 未登录状态 */
header.no-login{
	height: 90px;
	padding: 24px;
	position: fixed;
	width: 100%;
	z-index: 9999;
	transition: all 0.8s;
}

/* 导航栏的动画 */
header.no-login.active{
	background: #fff;
	height: 70px;
	box-shadow: 0 2px 2px 1px rgba(0, 0, 0, 0.3);
	padding: 16px;
}
header.no-login.active .logo a,
header.no-login.active .navlist ul li a{
	color: #000;
}
header.no-login.active .navlist ul li a:hover{
	color: #00A491;
}
header.no-login .navlist ul li.inview a{
	border-bottom-color: #00A491; 
	color: #00A491;
}

/* ******** */
.nav-bar{
	display: flex;
	flex-wrap: wrap;
	justify-content: space-between;
	padding: 0 112px;
	height: 100%;
	align-items: center;
}
.logo a{
	font-weight: bold;
	font-size: 22px;
	color: #fff;
	border-bottom: 5px solid transparent;
}
.navlist ul{
	display: flex;
	flex-wrap: wrap;
}
.navlist ul li{
	margin: 0 20px;
}
.navlist ul li a{
	padding: 5px 0 4px;
	border-bottom: 2px solid transparent;
	color: #fff;
}
.navlist ul li a:hover{
	border-bottom-color: #00A491; 
}

/* 登陆状态 */
header.login{
	position: fixed;
	width: 80%;
	right: 0;
	border-bottom: 1px solid #eff0f6;
	background: #fff;
	z-index: 999999;
}
header.login .nav-bar{
	padding: 14px 20px;
}
header.login .user-tag{
	display: flex;
	flex-wrap: wrap;
	align-items: center;
}
header.login .moto{
	color: #00A491;
}
header.login .moto a{
	color: #00A491;
	text-decoration: underline;
}
header.login .greet em{
	font-style: normal;
	font-weight: bold;
}
header.login .user-icon{
	display: block;
	width: 50px;
	height: 50px;
	border-radius: 50%;
	margin-left: 20px;
	border: 1px solid #00A491;
}

</style>
