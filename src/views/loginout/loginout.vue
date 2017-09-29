<template>
    <div class="loginout">
        <header>
            <span class="icon iconfont icon-back"></span>
            <span>设置</span>
            <span> </span>
        </header>
        <main>
            <div class="people">
        
               <h2><span>我的头像</span><small><img src="../../images/mine.png" alt=""><span>></span></small></h2>
               <h2><span>用户名</span><small>18310552816 &nbsp;&nbsp;&nbsp;<span>></span> </small></h2>
                <h2><span>我的二维码名片</span><small> <span>></span> </small></h2>
                 <h2><span>绑定手机号</span><small><span>></span> </small></h2>
            </div>
        </main>
         <button @click="loginout">退出登录</button> 
    </div>
</template>
<script>
   //将cookie进行拆分，点击退出登录时将cookie的当前对应值取消
    export default {
        data(){
            return {}
        },
        methods:{
            loginout:function () {
                let cookie_str = document.cookie;
                console.log(cookie_str)
                // 1505B-tocken=13636523652
             

                let arr =cookie_str.split(';');
                console.log(arr)
                arr.forEach((item,index)=>{
                    if(item.indexOf('1505B')>-1){
                        arr.splice(index,1)
                    }
                })


                //从cookie的值中删除某项
                function setCookie(cname, cvalue, exdays) {  
                    var d = new Date();  
                    d.setTime(d.getTime() + (exdays*24*60*60*1000));  
                    var expires = "expires="+d.toUTCString();  
                    document.cookie = cname + "=" + cvalue + "; " + expires;  
                } 
                setCookie('1505B-tocken','',-1);
                

                this.$router.replace({
                    name:'mine'
                })
            }
        }
    }
</script>
<style lang="" scoped>
     .loginout{
         width:100%;
         height: 100%;
         background: #eee;
     }
     header{
          width:100%;
          height: 44px;
          display: flex;
          justify-content:space-between;
          padding:0px 10px;
          background: #ffffff;
          margin-top: 10px;
     }
     main{
         display: flex;
         width:100%;
         height: auto;

     }
     .people{
         width:100%;
         height: auto;

     }
     .people h2{
         width:100%;
         height:40px;
         display: flex;
         justify-content: space-between;
         padding:0px 10px;
         line-height: 40px;
         font-weight: normal;
         position: relative;
         background: #fff;
     }
     .people h2 img{
         width:40px;
         height: 40px;
         margin-right: 20px;
     }
     .people h2 .icon{
       position: absolute;
       top:0px;
       right:10px;
     }
     button{
         width:40%;
         margin-left: 30%;
         height: 40px;
         border:none;
         outline: none;
         border-radius: 10px;
         margin-top: 20px;
         background:red;
         color: #fff;;

     }
</style>