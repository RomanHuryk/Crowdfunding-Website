<template>
	<div>
	<el-row class="content">

		<el-menu theme="dark" :default-active="activeIndex" class="el-menu-demo" mode="horizontal" @select="handleSelect">
			<el-row :gutter="20">
				<el-col :xs="4" :sm="4" :md="4" :lg="4">
					<el-menu-item index="1">
						<router-link to="/Projectview">ProjectX</router-link>
					</el-menu-item>
				</el-col>
				<el-col :xs="4" :sm="4" :md="4" :lg="4">
					<el-submenu index="2">
						<template slot="title">Tags</template>
						<el-menu-item index="2-1" :click="goart">Arts</el-menu-item>
						<el-menu-item index="2-2">IT</el-menu-item>
						<el-menu-item index="2-3">Music</el-menu-item>
					</el-submenu>
				</el-col>
				<el-col :xs="6" :sm="4" :md="12" :lg="4">
					<el-menu-item index="3"><router-link to="/friendzone">Friends</router-link></el-menu-item>
				</el-col>
				<el-col :xs="6" :sm="6" :md="12" :lg="7" class="searchbar">
					<el-menu-item index="3">
						<el-input
						  placeholder="Search..."
						  icon="search"
						  v-model="input2"
						  :on-icon-click="handleIconClick"
						  @keyup.enter.native="handleIconClick">
						</el-input>
					</el-menu-item>
				</el-col>
				<el-col :xs="4" :sm="2" :md="2" :lg="2">
					<el-menu-item index="4">
						<router-link to="/userfile">
						<img class="img-circle" :src="picurl" width="60px" height="50px" alt="logo">
						</router-link>
					</el-menu-item>
				</el-col>
				<el-col :xs="4" :sm="4" :md="2" :lg="2">
					<el-submenu index="5">
						<template slot="title">{{name}}</template>
						<router-link to="/userfile">
						<el-menu-item index="2-1">Profile</el-menu-item>
						</router-link>
						<el-menu-item index="2-3" @click="logout">Log out</el-menu-item>

					</el-submenu>
				</el-col>
				<!-- <el-col :span="16"></el-col>
				<el-col :span="16"></el-col>
				<el-col :span="16"></el-col> -->
				
				
			</el-row>
		</el-menu>

	</el-row>
	</div>
</template>

<script>
// import jwt from 'jsonwebtoken'
import router from '../router/index'
export default {
	name: 'nav-bar',
	created(){ // 组件创建时调用
		const userInfo = this.getUserInfo(); // 新增一个获取用户信息的方法
		if(userInfo != null){
			this.id = userInfo.id;

			this.name = userInfo.name;
			let temp = userInfo.pic;
			// if(temp==null){
			// 	this.$router.go('/')
			// }
			this.picurl = temp;
			// console.log(this.picurl);

		}else{
			this.id = '';
			this.name = ''
		}
	},
	data () {
		return {
			name: '', // 用户名改为空
			todos: '',
			activeName: 'first',
			list:[],
			count: 0,
			id: '', // 新增用户id属性，用于区别用户
			activeIndex: '1',
			activeIndex2: '1',
			input2: '',
			picurl: ''

		};
	},
	computed: { // 计算属性用于计算是否已经完成了所有任务
		Done(){
			let count = 0;
			let length = this.list.length;
			for(let i in this.list){
				this.list[i].status == true ? count += 1 : '';
			}
			this.count = count;
			if(count == length || length == 0){
				return true
			}else{
				return false
			}
		}
	},
	watch: {
    	$route (to, from) {
    		this.$router.go()
    	}
    },
	methods: {
			handleSelect(key, keyPath) {
				// console.log(key, keyPath);
			},
			getUserInfo(){ // 获取用户信息
				
				const name = localStorage.getItem('name');
				const id = localStorage.getItem('id');
				let pic='';
				if (localStorage.getItem('picurl')=='null'){
					pic = 'static/album/avatar.png'
				}
				else{
					pic = localStorage.getItem('picurl');}	

				if(name != null && name != 'null'){
					// let decode = jwt.verify(token,'vue-koa-demo'); // 解析token
					return {name: name,id: id, pic: pic} // decode解析出来实际上就是{name: XXX,id: XXX}
				}else {
					return null
				}
			},
			handleIconClick(ev) {
     			 console.log('/projectdetail/'+this.input2);
     			 this.$router.push('/projectview/'+this.input2)
    		},
    		logout() {
    			// this.$session.destroy();
    			localStorage.setItem('demo-token', null);
    			localStorage.setItem('name',null);
    			localStorage.setItem('id',null);
				localStorage.setItem('email',null);
          		localStorage.setItem('city',null);
          		localStorage.setItem('occupation',null);
          		localStorage.setItem('picurl',null);

          		localStorage.setItem('demo-token', null);
    			localStorage.setItem('name',null);
    			localStorage.setItem('id',null);
				localStorage.setItem('email',null);
          		localStorage.setItem('city',null);
          		localStorage.setItem('occupation',null);
          		localStorage.setItem('picurl',null);
    			this.$router.push('login');
    		}
	},

};
</script>

<style lang="stylus" scoped>
	.el-menu-demo
		margin-top:0
	.el-input
		margin 10px auto
	.todo-list
		width 100%
		margin-top 20px
		padding-bottom 8px
		border-bottom 1px solid #eee
		overflow hidden
		text-align left
		.item
			font-size 20px
			&.finished
				text-decoration line-through
				color #ddd
	.searchbar{
		margin-right: 0%
	}
	.pull-right
		float right
	.img-circle {  
    -moz-border-radius: 50%;  
    -webkit-border-radius: 50%;  
    border-radius: 50%;  
	}  
  
	.img-circle:hover {  
    -moz-box-shadow: 0px 0px 25px rgba(0, 255, 178, 1);  
    -webkit-box-shadow: 0px 0px 25px rgba(0, 255, 178, 1);  
    box-shadow: 0px 0px 25px rgba(0, 255, 178, 1);  
    cursor: pointer;  
	} 
  .el-col {
    border-radius: 4px;
  }
</style>