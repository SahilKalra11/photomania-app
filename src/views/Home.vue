<template>
  <div>
    <div class="wrapper">
        <header class="header-section">
          <h1 class="logo-text">LOGO</h1>
          <ul>
            <li class="prev-button"><button @click = "previous()">prev</button></li>
            <li class="next-button"><button @click = "next()">next</button></li>
          </ul>
        </header>
        <section class="body-section">  
          <div v-if='loading'></div>
          <ul v-else class="image-card-grid">
              <image-card v-for="image in cleanImages" :key="image.id" :image="image" />
          </ul>
        </section>
    </div>
  </div>
</template>

<script>

import axios from "axios";
import ImageCard from "@/components/ImageCard";

export default {
    name: "home",
    components: {
        ImageCard
    },
    data() {
        return{
            images: [],
            count:1,
            limit:30,
            loading:false,
        };
    },
    watch:{
      count:function(newValue,oldValue){
        this.loading= true;
        if(newValue != oldValue && newValue >=1){
          axios.get(`https://picsum.photos/v2/list?page=${this.count}&limit=${this.limit}`)
          .then(res =>{
            if(res.data instanceof Array && res.data){
              this.images = res.data;
              this.loading = false;
            }
            else{
              this.loading = false;
            }
            this.loading = false;
          }).catch(err => {
            this.loading = false;
            throw err;
          })
        }
        else{
          this.loading = false;
        }
      },
      '$route.hash':function(to){
          if(to.charAt(0) === '#'){
            this.count = to.charAt(1);
          }
      }
    },
    methods:{
      previous:function(){
        this.count--;
        location.hash = this.count;
        if(this.count < 2){
            this.$router.push(`/`);
        } 
      },
      next:function(){        
        this.count++;
        location.hash = this.count;
        if(this.count < 2){
            this.$router.push(`/#2`);
        } 
      }
    },
    created(){
        axios({
          method: 'get',
          url: "https://picsum.photos/v2/list?page={this.count}&limit={this.limit}",
        }).then(res =>{
          if(res.data instanceof Array && res.data){
            this.images = res.data;
          }   
          this.loading = false;
        }).catch(err =>{
          throw err;
        })
    },    
    computed: {
        cleanImages() {
            return this.images.filter(image => image.url);
        },
    },
};
</script>

<style lang="scss">

.logo-text{
  color:#fff;
}
.header-section{
  text-align: center;
  padding-top: 15px;
  ul{
    list-style-type: none;
    padding: 0px 12px;
    padding-right: 20px;
    margin:0;
    li{
      margin-bottom: 0.5rem;
      button{
        background: -webkit-linear-gradient(top, #efcb5d 0%, #d18b0f 100%);
        border: none;
        padding: 0.5rem 2rem;
        border-radius: 30px;
        color: #fff;
        font-weight: 700;
        text-transform: uppercase;
        &:hover{
          cursor: pointer;
          outline: none;
        }
        &:active,&:focus{
          outline: none;
        }
      }
    }
    .prev-button{
      float: left;
    }
    .next-button{
      float: right;
    }
  }
  img{
    margin: auto;
    max-width:250px;
    width:100%
  }
}
.screen-reader-only {
  height: 1px;
  width: 1px;
  position: absolute;
  left: -100000px;
}
.text-centered {
  text-align: center;
}
.wrapper {
  margin: 0 auto;
  max-width: 1170px;
  @media only screen and (max-width: 799px) {
    max-width: 100%;
    margin: 0 1.5rem;
  }
}
.image-card-grid {
  list-style: none;
  margin: 0.5rem 0;
  padding: 0;
  display: flex;
  align-items: flex-start;
  flex-wrap: wrap;
  width:100%;
}
.navbar {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem;
  background: #f0f0f0;
}
.searchbar {
  width: 300px;
  display: flex;
  align-items: center;
  justify-content: center;
  @media only screen and (max-width: 549px) {
    width: 100%;
    label {
      width: 80%;
    }
  }
}
.searchbar-input {
  padding: 0.5rem 1rem;
  border-radius: 20px;
  font-size: 1rem;
  border: 1px solid gray;
  min-width: 300px;
  @media only screen and (max-width: 549px) {
    min-width: 0;
    width: 100%;
  }
}
.btn {
  padding: 0.5rem 1rem;
  font-size: 1rem;
  border-radius: 20px;
  background: transparent;
  border: none;
}
.btn--green {
  background: #42b983;
  color: white;
  font-weight: bold;
}
.btn--go {
  padding: 0.5rem 2rem;
  margin-left: 1rem;
}
</style>
