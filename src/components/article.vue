<template>
  <div id="article">
    <div 
    :class="this.big?'quanping':''"
    class="xiangqing" >
    <i class="iconfont" @click="quanping">&#xe690;</i>
    <div class="wenzhang" v-html="article"  @click='photo($event)' />
    <div class="foot">
        &ensp;如有疑问请联系作者，QQ：384019118
    </div>
    </div>
    <div @click="close" class="photo" v-if="src">
      <img  :src="src" alt="图片请求失败">
    </div>
  </div>
</template> 
 <script>
export default {
  // Cmd markdown解析所用的文件
  name: "article",
  data() {
    return {
      id: this.$route.params.id,
      article: "",
      big:false,
      src:'',
    };
  },
  methods: {
    quanping(){
      this.big=!this.big;
    },
    photo(e){
       if(event.target.nodeName === 'IMG'){
          this.src=e.target.src
       }
    },
    close(){
      this.src="";
    },
    getarticle() {
      // get请求
      this.$axios({
        method: "get",
        url: "/json/article",
        params: {
          id: this.$route.params.id
        }
      }).then(
        res => {
          // //给所有img添加事件
          // res.data.data.htmlContents= res.data.data.htmlContents.replace(/img/g, "img click='photo()'")
          // // 1，匹配出图片img标签（即匹配出所有图片），过滤其他不需要的字符
          // // 2.从匹配出来的结果（img标签中）循环匹配出图片地址（即src属性）
          // var str =res.data.data.htmlContents;
          // //匹配图片（g表示匹配所有结果i表示区分大小写）
          // var imgReg = /<img.*?(?:>|\/>)/gi;
          // //匹配src属性
          // var srcReg = /src=[\'\"]?([^\'\"]*)[\'\"]?/i;
          // var arr = str.match(imgReg);//所有已成功匹配图片的数组：
          // for (var i = 0; i < arr.length; i++) {
          //   var src = arr[i].match(srcReg);
          //   //获取图片地址
          //   if (src[1]) {
          //     src=(i + 1) + "：" + src[1]
          //   }
          //   //当然你也可以替换src属性
          //   if (src[0]) {
          //     var t = src[0].replace(/src/i, "href");
          //     alert(t);
          //   }
          // }
          this.article=res.data.data.htmlContents
        },
        err => {
          console.log(err);
        }
      );
    }
  },
   created(){
 this.getarticle();
   },
};
</script>
<style scoped>
.photo{
  position: fixed;
  background-color:rgb(7, 7, 7);
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  z-index: 99999;
}
.photo img{
  width: 70%;
  position: absolute;
  left: 50%;
  top: 50%;
  transform:translate(-50%,-50%);
}
.wenzhang{
  min-height: 1100px; 
}
.foot{
  border-top:2px solid #000;
   width: 100%;
}
.backTo {
  width: 100%;
}
.xiangqing {
  width: 90%;
  margin: 0 auto;
  background-color: #F9F9F5;   
  margin-bottom: 30px;
  border-radius: 20px;
  padding: 5%;
  padding-top: 2%;
}
.quanping{
   border-radius: 0px;
  position: absolute; 
  top:-100px;
  width: 60%;
  left: 0;
  right: 0;
  z-index:999;
 padding-left: 20%;
 padding-right: 20%;
 opacity: 1;
  }
  .iconfont{
    font-size: 25px;
  position: relative;
  left: 93%;
  }
  .iconfont:hover {
  color: #ff6600;
  cursor:pointer;
}
@media screen and (max-width: 800px) {
  .quanping{
  width: 96%;
 padding-left: 2%;
 padding-right: 2%
  }
  .iconfont{
     left: 88%;
  }
}
</style>
     