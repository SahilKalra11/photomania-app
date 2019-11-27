<template>  
    <li class="image-card">
        <a :href="handlersrc(down)" class="image-card__anim" target="_blank" >
            <img class="image-card__image" :src="handlersrc(img)" :alt="image.id" />
        </a>
        <div class="image-card__body">
            <p class="image-owner"><a :href="image.url" target="_blank">{{image.author}}</a></p>
            <p v-if="image.id" class="image-title">{{image.id}}</p>
            <p v-else class="image-title">No Title Found</p>
       </div>
    </li>    
</template>


<script>
const url = require('url');
export default {
    name:"ImageCard",
    props:[ "image","count" ],    
    data(){
        return {
            parsed:'',
            width:367,
            height:267,
            down:"down",
            img:'image'
        } 
   }, 
    methods:{	
        handlersrc:function(flag){
        switch(flag)
        {
        case'down':
        return url.resolve(this.image.download_url,`/id/${this.image.id}/${this.image.width}/${this.image.height}`)
        case'image':
        return url.resolve(this.image.download_url,`/id/${this.image.id}/${this.width}/${this.height}`)
        }
	}  
   },
};
</script>

<style lang="scss" scoped>
.image-card{
    width: calc(33% - 1rem);
    margin: 0.5rem;
    border-radius: 5px;
    box-shadow: 0 5px 15px rgba(0, 0, 0, 0.15);
    margin-bottom: 1rem;
    overflow: hidden;
    background: #fff;
    .image-card__anim{
        position: relative;
        display: block;
        overflow: hidden;
    }
    &:hover{
            .image-card__image{
                transform: scale(1.12);
            }
            .image-card__body{
                background: -webkit-linear-gradient(top, #efcb5d 0%, #d18b0f 100%);
                a{
                    color:#fff;
                }
                .image-title{
                    color:#fff;
                }
            }
            background: -webkit-linear-gradient(top, #efcb5d 0%, #d18b0f 100%);
        }
    
    @media only screen and (max-width: 799px) {
        width: calc(50% - 1rem);
    }
    @media only screen and (max-width: 549px) {
        width: 100%;
        margin: 0.5rem 0;
    }
}
.image-card__image{
    border-radius: 5px 5px 0 0;
    width: 100%;    
    object-fit: cover;
    transition: all 0.2s linear
}
.image-card__body{
    padding:1rem 1rem;
    display:inline-block;
    width:100%;
    position: relative;
    top:-4px;
    background: #fff;
    .image-owner{
        float:left;
        margin: 0px;
        font-size: 16px;
        a{
            text-decoration: none;
            color: #000;
            font-weight: 700;
        }
    }
    .image-title{
        float: right;
        font-weight: 700
    }
}
.image-title{
    font-weight: bold;
    margin: 0;
}
.image-owner {
  margin-top: 0;
  font-size: 0.8rem;
}
.image-title,
.image-owner {
  white-space: nowrap;
  text-overflow: ellipsis;
  overflow: hidden;
  position: relative;
  top: 4px;
}
.image-date-view-wrapper {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.image-date,
.image-views {
  margin-bottom: 0;
  font-size: 0.8rem;
}
</style>