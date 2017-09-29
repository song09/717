  <template>
    <div class="address_wrap">
        <header class="header">
            <span class="icon iconfont icon-back" @click="go_to_back"></span>
            <span class="add_head">收货地址</span>
            <span class="icon iconfont icon-home right" @click="go_home"></span>
        </header>
        <main>
            <ul class="uls">
                <li v-for="x in add_list" :id="id" v-show="show" :key="x.name">
                    <span class="name_phone">{{x.name}}
                        <b>{{x.phone}}</b>
                    </span>
                    <small class="site">{{x.province+" "+x.city+' '+x.area}}</small>
                    <p class="default">
                        <span class="dui">
                            <i>✔</i>设为默认</span>
                        <span class="cancel">
                           <span @click="edit(x.id)"><i class="iconfont icon-send"></i>编辑</span>
                             <span @click="delete_info(x.id)"><i class="iconfont icon-apps"></i>删除</span>   
                        </span>
                    </p>
                </li>
            </ul>
            <button class="btn_address" @click="go_to_add_peo">
                <span>+</span>新添加收货地址</button>
        </main>
    </div>
</template>
<script>
import token from '../../utils/getcookie.js'
export default {
    data() {
        return {
            add_list: [],
            show: true,
            id: "",


        }
    },
    created() {
        if (token()) {
            this.$http.get('/get_address_list', { token: token() }).then(res => {
                //  console.log(res.data[1].id)
                if(res.data){
                    this.add_list = res.data
                    this.add_list.forEach((item, index) => {
                        this.id = res.data[index].id;
                        // console.log(this.id)
                    })
                }
            })
        }

    },
    methods: {
        go_to_add_peo() {
            this.$router.push('/message')
        },
        go_to_back() {
            this.$router.push({ name: 'mine' })
        },
         go_home(){
            this.$router.push('/home')
        },
          edit(id){
             // alert('dbjnd')
            this.$router.push({
                name:'message',
                params:{
                    id:id
                }
            })
        },
        delete_info(id){
            this.$http.get('/delete_delivery_info',{id:id,token:token()}).then(res=>{
                
                let idx;
                if(res.data =='success'){

                    this.active = true;
                    setTimeout(()=> {
                        this.active = false
                    }, 100);

                    this.add_list.forEach((item,index)=>{
                        if(item.id == id){
                            idx=index
                        }
                    })
                    this.add_list.splice(idx,1);
                    
                    
                }
            })
        }

    }

}
</script>
<style scoped>
.address_wrap {
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
}

.header {
    width: 100%;
    height: 44px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0px 10px;
    border-bottom: 1px solid #ccc;
}

.right {
    border: 1px solid #ddd;
    background: #fff;
    ;
    border-radius: 50%;
    width: 30px;
    height: 30px;
    line-height: 30px;
    text-align: center;
}

.add_head {
    font-size: 15px;
}

main {
    width: 100%;
    height: auto;
    flex: 1;
    overflow-y: scroll;
}

main .btn_address {
    width: 60%;
    margin: 30px 20%;
    height: 40px;
    border: none;
    outline: none;
    background: #FF3A44;
    color: #fff;
    border-radius: 18px;
    line-height: 36px;
    text-align: center;
    font-size: 13px;
}

main .btn_address span {
    font-size: 20px;
    margin-right: 10px;
}

.uls {
    width: 100%;
    background: #fff;
}

.uls li {
    min-height: 2rem;
}

.name_phone {
    display: block;
    padding-top: .28rem;
    font-size: .3rem;
    padding-left: .3rem;
}

.name_phone b {
    font-weight: normal;
    padding-left: .3rem;
}

.site {
    display: block;
    color: #666;
    padding-left: .3rem;
    margin-top: .2rem;
}

.default {
    width: 100%;
    margin-top: .28rem;
    line-height: .8rem;
    border-top: 1px solid #eee;
    padding-left: .3rem;
    display: flex;
    justify-content: space-between;
    padding-right: .2rem;
    color: #666;
}

.dui {
    font-size: 14px;
}

.dui i {
    display: inline-block;
    font-style: normal;
    width: .5rem;
    height: 0.5rem;
    border-radius: 50%;
    margin-right: .2rem;
    border: 1px solid #ccc;
    line-height: .5rem;
    text-align: center;
    font-size: .32rem;
    background: red;
    color: #fff;
}

.cancel i {
    padding: 0 .2rem;
}
</style> 
  

