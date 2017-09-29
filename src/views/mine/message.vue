<template>
    <div class="add_peo_wrap">
         <tips :active="tips_active">{{tip_info}}</tips> 
        <header class="header">
          <span class="icon iconfont icon-back"  @click="go_to_back"></span>
          <span class="add_head">收件人</span>
          <span class="icon iconfont icon-home right" @click="go_home"></span>
        </header>
        <main class="main_add">
            <input type="text" placeholder="收件人姓名" v-model="name" ref="name">
            <input type="text" placeholder="手机号" v-model="phone">
            <p>                                                       
                <select v-model="province">
                     <option value="">请选择省</option>
                     <option v-for="x in list" :value="x.name" :key="x.name">{{x.name}}</option>
                </select>
                <select v-model="city">
                     <option value="">请选择城市</option>
                     <option v-for="x in city_list" :value="x.name" :key="x.name">{{x.name}}</option>
                </select>
            </p>
            <select name="" id="sel" v-model="area">
                <option value="">请选择区</option>
                <option v-for='x in area_list' :value="x" :key="x">{{x}}</option>
            </select>
            <input type="text" placeholder="详细地址">
            <p class="pps"><span class="add_shop ">√</span>&nbsp;&nbsp;设为默认地址</p>
            <button class="btn" @click="verify">保存</button>
        </main>
      <loadings :isActive="show"></loadings>
    </div>
</template>
<script>
import loading from "../../components/loading.vue"
import token from '../../utils/getcookie.js'
import tips from '../../components/tips.vue'

 export default {
     data(){
        return{
           show:true,
           name:'',
           phone:'',
           list:[], 
           province:'',
           city:'',
           area:'',
           area_list:[],
           city_list:[],
           tips_active:false,
           tip_info:""
        }
    
     },
      methods:{
        go_to_back(){
             this.$router.push('/site')
        },
        go_home(){
            this.$router.push('/home')
        },
        tips(info){
           this.tips_active = true;
           setTimeout(()=>{
               this.tips_active = false
           },1800)
            this.tip_info = info;
        },
        verify(info){
            let reg_phone=/^1[34578]\d{9}$/;
             let str="";
               let that = this;
             if(!this.name){
                
                  this.$modal.show({
                     title:'提示信息',
                     info:'请输入姓名',
                     callback:function(){
                         that.$refs.name.focus()
                     }
                 })
            }
            if(!this.phone  || !reg_phone.test(this.phone)){
                this.tips("请输入正确的手机号")
                return 
            }
            if(!this.province){
                this.tips("请选择省")
                return 
            }
            if(!this.city){
               this.tips("请选择市")
               return
            }
            if (!this.area){
                this.tips("请输入区域")
                return
            }
            this.$http.post('/add_new_address',{
                name:this.name,
                phone:this.phone,
                province:this.province,
                city:this.city,
                area:this.area,
                token:token(),
                street:this.address
            }).then((res)=>{
                console.log(res)
            })
              
            this.$router.push('/site')

        }
           
    },
    components:{
        "loadings":loading,
        "tips":tips
    },
    created(){
        this.show= true;
       
        let get_address_info = new Promise((resolve,reject)=>{
            this.$http.post('/get_address_info').then((res)=>{
                
                resolve(res.data)
                
            })
        })
        let edit_address_info = new Promise((resolve,reject)=>{
            let id = this.$route.params.id
            if(id){
                this.$http.get('/edit_delivery_info',{id:id,token:token()}).then((res)=>{
                    resolve(res.data)
                    
                })
            }else{
                resolve(0)
            }
            
        })

        
        Promise.all([get_address_info,edit_address_info]).then((res)=>{
            this.list = res[0];
         //   console.log(res);
         if(res[1]){
              this.name = res[1].name;
            this.phone = res[1].phone;
            this.province = res[1].province;
            this.city = res[1].city;
            this.area = res[1].area;
            this.street = res[1].street;
         }
           
            this.show=false
            
        })


    },
    watch:{
        'province':function(params){
          // console.log(params)
           let lists=this.list;
           console.log(lists)
           if(lists.length>0){
               lists.forEach((item)=>{
                   if(item.name == params){
                       this.city_list = item.city;
                       
                   }
               })
           }
        },
        "city":function(params){
          let c_lists = this.city_list;
          console.log(c_lists);
          if(c_lists.length>0){
              c_lists.forEach((item)=>{
                  if(item.name == params){
                      this.area_list =item.area;
                      //console.log(this.area_list)
                  }
              })
          }
        }
    },
   
 }
</script>

<style scoped>
  .add_peo_wrap{
        width:100%;
       height: 100%;
       display: flex;
       flex-direction: column;
       background: #eee;
  }
    .header{
        width:100%;
        height: 44px;
        display:flex;
        justify-content: space-between;
        align-items: center;
        padding:0px 10px;
        border-bottom: 1px solid #ccc;
        background: #fff;                                                                                                                                                                                                                                
   }
     .right {
        border: 1px solid #ddd;
        background: #fff;;
        border-radius: 50%;
        width: 30px;
        height: 30px;
        line-height: 30px;
        text-align: center;
    }
    .add_head {
      font-size: 15px;
    }
    .main_add{
        width:100%;
        height: auto;
    }
    .main_add input{
        width:90%;
        height: 37px;
        margin-top: 15px;
        text-indent: 10px;
        border:none;
        outline: none;
        line-height: 37px;
        margin-left: 5%;
    }
   .main_add p {
       width:90%;
       height: 50px;
       margin-left: 5%;
       display: flex;
       justify-content:space-between;
   }
   select{
       width:40%;
      /* margin-left: 7%;*/
       height: 35px;
       outline: none;
       border:none;
       color: #666;
       margin-top: 15px;
   }
   #sel{
       width:90%;
       height: 35px;
       line-height: 35px;
       margin-left: 5%;
       
   }
   .pps{
       width:80%;
       height: 35px;
       margin-top: 20px;
       line-height: 35px;
       
   }
   .add_shop{
        width:20px;border-radius: 50%;
        height:20px;
        border:1px solid #ccc;
        display: inline-block;
        margin-left: 30px;
        background: #fff;
        line-height: 20px;
        text-align: center;
        color: #555;

     }
      .col_span{
        width:20px;border-radius: 50%;
        height:20px;
        border:1px solid #ccc;
        display: inline-block;
        margin-left: 30px;
        background:#FB4141;
        color: #fff;
        line-height: 20px;
        text-align: center;
     }
     .btn{
         width:80%;
         height: 35px;
         line-height: 35px;
         text-align: center;
         margin-left: 10%;
         background: #FF3A44;
         border:none;
         outline: none;
         border-radius: 15px;
         color: #fff;
         margin-top: 20px;
     }
</style>