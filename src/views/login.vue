<template>
	<div class="login">
	    <div class="login-title">
	    	<h2>登录漂流书单</h2>
            <!--<i class="fa fa-times" aria-hidden="true" @click='closeDialog'></i>-->
	    </div>
		<div class="login-email">
			<input type="email" name='useremail' placeholder="邮箱" class="login-input" v-model="useremail">
		</div>
		<div class="login-pwd">
			<input type="{{ iptType }}" name="password" placeholder="密码" class="login-input" v-model='password'>
			<i class="fa fa-lg fa-eye" aria-hidden="true" @click='showPwd'></i>
		</div>
        <div class="login-remeber-me">
            <input type="checkbox" name="r">
            <label for='r'>记住我</label>
            <a v-link='{path:"/register"}'>没有账号，来注册</a>
        </div>
		<div class="login-btn" id="bbb">
			<button type="button" @click='sendInfo'>登录</button>
		</div>
	</div>
	<div class="modal"></div>
</template>
<script>
import  biu  from 'biu.js'
import  $  from  'jquery'
export default {
  ready(){
      document.title="登录漂流书单";
  },
	data () {
		return {
           useremail:' ',
           password: '',
           iptType:'password',
           hasLogin:false
		}
	},
	methods:{
		sendInfo:function(){
			   var  userData={
				      useremail:this.useremail,
				      password:this.password
			   };
        var self=this;
        if(this.validateEmail()){//如果格式正确
                   /*localStorage.userId=123344;
                   localStorage.username='浴火小青春';

                   var redirect = decodeURIComponent(self.$route.query.redirect || '/');
                   self.$route.router.go(redirect);*/
                  // 提交email和密码
                  $.ajax({
                    url:'http://192.168.155.1:8080/Test/login',
                    dataType:'json',
                    type:'post',
                    data:{
                      userData:JSON.stringify(userData)
                    },
                    success:function(data){
                        console.log(data);
                        if(data.status){//判断登录是否成功
                           // 记录重定向的地址
                           var redirect = decodeURIComponent(self.$route.query.redirect || '/');
                           self.$route.router.go(redirect);
                           
                           //登录成功，保存用户名和密码到本地
                           localStorage.userId=data.userId;
                           localStorage.username=data.username;
                           
                           /*if(data.isFisrtLogin){ //第一次登录，跳转到setting.vue
                               self.$route.router.go('/setting');
                           }*/
                          // self.$route.router.go('/');
                          }else{
                              biu("输入的账号或密码有误，请重新输入",{
                                  type:'warning'
                              });
                          }
                    },
                    error:function(){
                        console.log('error');
                    }
                });
        }else{
                //
        }
		},
    validate:function(){
        	if(this.validateEmail()){
        		return true;
        	}
        	return false;
    },
    showPwd:function(){
           if(this.iptType=='text'){
                this.iptType='password';
                return false;
            }
           if(this.iptType=='password'){
                this.iptType='text';
                return false;
           }
    },
    validateEmail:function(){
        	var filter  = /^([a-zA-Z0-9_\.\-])+\@(([a-zA-Z0-9\-])+\.)+([a-zA-Z0-9]{2,4})+$/;
        	if (filter.test(this.useremail)) {// 邮箱格式正确
                   return true;
             }
             biu('邮箱格式错误',{
                 type:'warning'
             });
               return false;
    }
  }
}
</script>
<style src="../assets/css/biu.css"></style>
<style scoped>
 .login{
 	border: 1px solid #cbcbcb;
 	width: 300px;
 	position: fixed;
    top:20vh;
    margin-left: -150px;
    left: 50vw;
    z-index: 999999;
 	box-shadow: 0 0 15px 0 rgba(0,0,0,.3);
 	border-radius: 5px;
 	background: #fff;
 }
 .login-title{
    text-align: center;
 }
 .login-title h2{
 	display: inline-block;
 }
 .login-title i{
    float: right;
    cursor: pointer;
    margin-top: 10px;
    margin-right: 10px;
 }
 .login-email,.login-pwd{
 	width: 100%;
 	text-align: center;
 }
 .login-pwd{
 	margin-top: 20px;
 	position: relative;
 }
 .login-pwd i{
 	position: absolute;
    top:5px;
    right: 50px;
    cursor: pointer;
 }
 .login-input{
 	width: 200px;
 	border-radius: 5px;
 	border: 1px solid rgba(142,122,122,0.8);
 	padding: 5px 5px;
 	text-indent: 10px;
 }
 .login-remeber-me{
    margin-top:20px;
    margin-left:45px;
    font-size:14px;
 }
 .login-remeber-me a{
   font-size: 12px;
  margin-left: 40px;
 }
 .login-btn{
     margin: 20px 0px;
     text-align: center;
 }
 .login-btn button{
 	-webkit-appearance: none;
 	border: 0px;
 	background: #0f88eb;
 	width: 220px;
 	color: #fff;
 	cursor: pointer;
 	height: 30px;
 	border-radius: 5px;
 	font-weight: bold;
 }
  .modal{
  	position: fixed;
  	top: 0px;
  	left: 0px;
 	width: 100%;
 	height: 100%;
  background: url("https://pixabay.com/static/uploads/photo/2015/05/20/06/25/hong-kong-774869_960_720.jpg");
  background-size: cover;
 	z-index: 1;
 }
</style>