<template>
  <div class="container">
    <div class="top ">
      <Nav :islogin="islogin"></Nav>
    </div>
    <div class="main col-md-12">
      <div class="bg-boxtop"></div>
      <div class="bg-content">
        <div class="row">
          <div class="col-md-3">
            <ul class="list-group">
              <li class="list-group-item">
                <router-link to="/center/base/">基本信息</router-link>
              </li>
              <li class="list-group-item">
                <router-link :to="{name:'resumecenter',params:{id:this.reid}}">我的简历</router-link>
              </li>
              <li class="list-group-item">
                <router-link to="/center/delivery_record/">投递记录</router-link>
              </li>
              <li class="list-group-item">
                <router-link to="">我的申请</router-link>
              </li>
            </ul>
          </div>
          <div class="col-md-9">
            <!--<Cput></Cput>-->
            <!--<Cresume></Cresume>-->
            <router-view></router-view>
          </div>
        </div>
      </div>
    </div>
    <Foot class="col-md-12 t"></Foot>
  </div>

</template>

<script>
  // import UserCenter from '@/components/generic_components/UserCenter'

  import Nav from '../components/generic_components/Nav'
  import Foot from './generic_components/Foot'
  import BaseMessage from './generic_components/BaseMessage'
  // import  Cput from './generic_components/Cput'
  // import Cresume from './generic_components/Cresume'
  import axios from 'axios'


  export default {
    name: 'center',
    data(){
      return{
        islogin:false,
        reid:[]
      }
    },
    mounted:function(){
      this.isLogin();
      this.getreid();
    },

    watch:{
      islogin:function (newvalue,oldvalue) {
        console.log(newvalue);
        console.log(oldvalue);
        return newvalue}
    },

    methods:{

      isLogin: function () {
        let token=sessionStorage.getItem('token');
        // console.log( token);
        if(token){
          let vm=this;
          axios.post('http://127.0.0.1:8000/user/verify/',
            {
              token:token
            }
          ).then(function (response) {
            let res=response.data;
            console.log(res);
            vm.reid=res.id;
            vm.islogin= res.user


          }).catch(function (error) {
            vm.islogin= false;
            console.log(error);

          })
        }
        // axios.get()
      },
      getreid:function(){
        let vm=this;
        let token=sessionStorage.getItem('token');
        axios.post('http://localhost:8000/resume/getresumemess/',{
          token:token
        })
          .then(function (response) {
            // this.res=eval(response.data);
            let res=response.data;
            vm.reid=[];
            for(let i=0;i<res.length;i++){
              vm.reid.push(res[i].id)
            }
            console.log(vm.reid)
          })
          .catch(function (error) {
            console.log(error)
          })
      }


    },

    watch:{
      islogin:function () {
        return this.isLogin
      },
    },

    components: {
      // UserCenter
      Nav, Foot, BaseMessage,
    },
   child: {}
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>


  .main {
    height: 100%;
    margin: auto;
  }

  ul li {
    list-style: none;
  }

  .list-group {
    overflow: hidden;
    width: 200px;
    font-size: 1em;
    /*margin-left: 40px;*/
    /*margin-top: 60px;*/
    /*margin-bottom: 30px;*/

  }

  i {
    color: red;
  }

  .list-group-item {
    border: 2px solid #eee;
  }

  .list-group-item:hover {
    font-weight: bold;
    color: #ff6000;
    background-color: #fffff9;
    border: 1px solid #eee;
    border-right-color: #fff;
    border-left: 2px solid #ff6000;
  }

  .bg-boxtop {
    background: rgba(192, 199, 191, 0.17);
    width: 1000px;
    height: 50px;
    margin: 20px auto;
    box-shadow: 2px 2px 8px grey;
  }

  .bg-content {
    width: 1000px;
    margin: auto;
    border: solid 1px #eee;
    border-radius: 5px;
    box-shadow: 2px 2px 8px grey;
  }

  .t {
    margin-top: 20px;
  }

</style>
