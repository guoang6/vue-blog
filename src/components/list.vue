<template>
  <div id="list">
    <div class="c-list">
      <router-link tag="div" :to="'/articel/'+item.id" v-for="item in  articlelist" :key="item.id" class="post-content">
        <img class="picture" :src="item.image" />
        <div class="cont">
          <p>
            <em class="recommend" v-if="item.istop==='1'">置顶</em>
            <em class="tuijian" v-if="item.istop==='0'">推荐</em>
            <span class="content-title">{{item.title}}</span>
          </p>
          <div class="content">{{item.summary}}</div>
        </div>
        <div class="post-content-footer">
          <span class="tag tag-time">{{item.releasetime | dataFormat}}</span>
          <span class="post-tag" v-for="(label,index) in item.label" :key="index">{{label}}</span>
          <span class="tag right ipad">
            <span>浏览({{item.visits}})</span>
            <span>留言({{item.comment}})</span>
          </span>
        </div>
     </router-link>
      <div class="pagepagin-list">
        <div>
          <span class="bot_page ye-pc" v-if="!gengduo">
            <a class="pagef">
              <i class="iconfont page_icon" @click="more()" ></i>更多
            </a>
          </span>
          <span class="bot_page ye">
            <a href="#" class="pagef">
              <i class="iconfont page_icon" @click="first()"></i>首页
            </a>
          </span>
          <span
            v-for="(item,id) in pagelist"
            :key="id"
            :class="'page_span_'+id "
            class="pagination ye"
          >
            <a id="page_id3" :class="thispage==item?'pagef':''">
             {{item}}
              <i class="iconfont page_icon" @click="getthispagearticlelist(item)"></i>
            </a>
          </span>
          <span class="bot_page ye">
            <a href="#" class="pagef">
              <i class="iconfont page_icon" @click="end()"></i>尾页
            </a>
          </span>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  name: "list",
  data() {
    return {
      Labelid: "",
      seriesid: "",
      releasetimes: "",
      search: "",
      thisarticletype: "",
      pagenum: "",
      pagelist: [],
      articlelist: [],
      thispage: "",
      gengduo: false
    };
  },
  methods: {
    // get获取所有文章列表
    getallarticlelist() {
      this.$axios({
        method: "post",
        url: "/json/article/pageQuery"
      }).then(
        res => {
          //分割字符串
          let arr = Object.entries(res.data.data);
          for (var i = 0; i <= arr.length - 1; i++) {
            res.data.data[i].label = res.data.data[i].label.split(",");
            res.data.data[i].label = res.data.data[i].label.slice(0, 3);
          }
          this.articlelist = res.data.data;
          this.pagenum = res.data.total;
          this.thispage = 1;
          this.getpagelist();
        },
        err => {
          console.log(err);
        }
      );
    },
    // get获取【标签】文章列表
    getLabelarticlelist() {
      this.$axios({
        method: "post",
        url: "/json/article/pageQuery",

        data: this.qs.stringify({
          Label: this.Labelid
        })
      }).then(
        res => {
          //分割字符串
          let arr = Object.entries(res.data.data);
          for (var i = 0; i <= arr.length - 1; i++) {
            res.data.data[i].label = res.data.data[i].label.split(",");
            res.data.data[i].label = res.data.data[i].label.slice(0, 3);
          }
          this.articlelist = res.data.data;
          this.pagenum = res.data.total;
          this.thispage = 1;
          this.getpagelist();
        },
        err => {
          console.log(err);
        }
      );
    },
    // get获取【专栏】文章列表
    getseriesarticlelist() {
      this.$axios({
        method: "post",
        url: "/json/article/pageQuery",
        data: this.qs.stringify({
          columnid: this.seriesid
        })
      }).then(
        res => {
          //分割字符串
          let arr = Object.entries(res.data.data);
          for (var i = 0; i <= arr.length - 1; i++) {
            res.data.data[i].label = res.data.data[i].label.split(",");
            res.data.data[i].label = res.data.data[i].label.slice(0, 3);
          }
          this.articlelist = res.data.data;
          this.pagenum = res.data.total;
          this.getpagelist();
        },
        err => {
          console.log(err);
        }
      );
    },
    // get获取【时间】文章列表
    gettimearticlelist() {
      this.$axios({
        method: "post",
        url: "/json/article/pageQuery",
        data: this.qs.stringify({
          releasetimes: this.releasetimes
        })
      }).then(
        res => {
          //分割字符串
          let arr = Object.entries(res.data.data);
          for (var i = 0; i <= arr.length - 1; i++) {
            res.data.data[i].label = res.data.data[i].label.split(",");
            res.data.data[i].label = res.data.data[i].label.slice(0, 3);
          }
          this.articlelist = res.data.data;
          this.pagenum = res.data.total;
          this.getpagelist();
        },
        err => {
          console.log(err);
        }
      );
    },
    // get获取【搜索】文章列表
    getsearcharticlelist() {
      this.$axios({
        method: "post",
        url: "/json/article/pageQuery",
        data: this.qs.stringify({
          search: this.search
        })
      }).then(
        res => {
          //分割字符串
          let arr = Object.entries(res.data.data);
          for (var i = 0; i <= arr.length - 1; i++) {
            res.data.data[i].label = res.data.data[i].label.split(",");
            res.data.data[i].label = res.data.data[i].label.slice(0, 3);
          }
          this.articlelist = res.data.data;
          this.pagenum = res.data.total;
          this.getpagelist();
        },
        err => {
          console.log(err);
        }
      );
    },
   
    //获取【页码———标签】文章列表
    getpagelabelarticlelist(thispage) {
      this.$axios({
        method: "post",
        url: "/json/article/pageQuery",
        data: this.qs.stringify({
          Label: this.Labelid,
          page: thispage
        })
      }).then(
        res => {
          //分割字符串
          let arr = Object.entries(res.data.data);
          for (var i = 0; i <= arr.length - 1; i++) {
            res.data.data[i].label = res.data.data[i].label.split(",");
            res.data.data[i].label = res.data.data[i].label.slice(0, 3);
          }
          if (this.gengduo) {
            this.articlelist = this.articlelist.concat(res.data.data);
            this.gengduo = !this.gengduo;
          } else this.articlelist = res.data.data;
          this.getpagelist();
        },
        err => {
          console.log(err);
        }
      );
    },
    //获取【页码———专栏】文章列表
    getpageseriesarticlelist(thispage) {
      this.$axios({
        method: "post",
        url: "/json/article/pageQuery",
        data: this.qs.stringify({
          seriesid: this.seriesid,
          page: thispage
        })
      }).then(
        res => {
          //分割字符串
          let arr = Object.entries(res.data.data);
          for (var i = 0; i <= arr.length - 1; i++) {
            res.data.data[i].label = res.data.data[i].label.split(",");
            res.data.data[i].label = res.data.data[i].label.slice(0, 3);
          }
          if (this.gengduo) {
            this.articlelist = this.articlelist.concat(res.data.data);
            this.gengduo = !this.gengduo;
          } else this.articlelist = res.data.data;
          this.getpagelist();
        },
        err => {
          console.log(err);
        }
      );
    },
    //获取【页码———全部】文章列表
    getpageallarticlelist(thispage) {
      this.$axios({
        method: "post",
        url: "/json/article/pageQuery",
        data: this.qs.stringify({
          page: thispage
        })
      }).then(
        res => {
          //分割字符串
          let arr = Object.entries(res.data.data);
          for (var i = 0; i <= arr.length - 1; i++) {
            res.data.data[i].label = res.data.data[i].label.split(",");
            res.data.data[i].label = res.data.data[i].label.slice(0, 3);
          }
          if (this.gengduo) {
            this.articlelist = this.articlelist.concat(res.data.data);
            this.gengduo = !this.gengduo;
          } else this.articlelist = res.data.data;
          this.getpagelist();
        },
        err => {
          console.log(err);
        }
      );
    },
    //获取【页码———时间】文章列表
    getpagetimearticlelist(thispage) {
      this.$axios({
        method: "post",
        url: "/json/article/pageQuery",
        data: this.qs.stringify({
          page: thispage,
          releasetimes:this.releasetimes,
        })
      }).then(
        res => {
          //分割字符串
          let arr = Object.entries(res.data.data);
          for (var i = 0; i <= arr.length - 1; i++) {
            res.data.data[i].label = res.data.data[i].label.split(",");
            res.data.data[i].label = res.data.data[i].label.slice(0, 3);
          }
          if (this.gengduo) {
            this.articlelist = this.articlelist.concat(res.data.data);
            this.gengduo = !this.gengduo;
          } else this.articlelist = res.data.data;
          this.getpagelist();
        },
        err => {
          console.log(err);
        }
      );
    },
    //获取【页码———搜索】文章列表
    getpagesearcharticlelist(thispage) {
      this.$axios({
        method: "post",
        url: "/json/article/pageQuery",
        data: this.qs.stringify({
          page: thispage,
          search:this.search,
        })
      }).then(
        res => {
          //分割字符串
          let arr = Object.entries(res.data.data);
          for (var i = 0; i <= arr.length - 1; i++) {
            res.data.data[i].label = res.data.data[i].label.split(",");
            res.data.data[i].label = res.data.data[i].label.slice(0, 3);
          }
          if (this.gengduo) {
            this.articlelist = this.articlelist.concat(res.data.data);
            this.gengduo = !this.gengduo;
          } else this.articlelist = res.data.data;
          this.getpagelist();
        },
        err => {
          console.log(err);
        }
      );
    },
     // 获取特定页码文章列表
    getpagearticlelist(thispage) {
      if (this.Labelid || this.seriesid || this.releasetimes || this.search) {
        this.Labelid && this.getpagelabelarticlelist(thispage);
        this.seriesid && this.getpageseriesarticlelist(thispage);
        this.releasetimes && this.getpagetimearticlelist(thispage);
        this.search && this.getpagesearcharticlelist(thispage);
      } else {
        this.getpageallarticlelist(thispage);
      }
    },
    //计算【页码】
    getpagelist() {
      var i;
      this.pagelist = [];
      var pagenum = this.pagenum;
      var thispage = this.thispage;
      if (pagenum > 7) {
        if (thispage < 4) {
          for (i = 1; i <= 7; i++) {
            this.pagelist.push(i);
          }
        } else {
          if (thispage > this.pagenum - 4) {
            for (i = pagenum - 6; i <= pagenum; i++) {
              this.pagelist.push(i);
            }
          } else {
            for (i = thispage - 3; i <= thispage + 3; i++) {
              this.pagelist.push(i);
            }
          }
        }
      } else {
        for (i = 1; i <= pagenum; i++) {
          this.pagelist.push(i);
        }
      }
    },
    //点击【更多】获取相应的特定属性文章文章
    more() {
      if (this.thispage < this.pagenum) {
        this.gengduo = !this.gengduo;
        this.thispage = this.thispage + 1;
        this.getpagearticlelist(this.thispage);
      }
    },
    //点击【首页】获取相应的特定属性文章文章
    first() {
      this.thispage = 1;
      this.getpagearticlelist(this.thispage);
    },
    //点击【尾页】获取相应的特定属性文章文章
    end() {
      this.thispage = this.pagenum;
      this.getpagearticlelist(this.thispage);
    },
    //点击某一页获取相应的特定属性文章文章
    getthispagearticlelist(thispage) {
      this.thispage = thispage;
      this.getpagearticlelist(thispage);
    },
    // 回到顶部方法，加计时器是为了过渡顺滑
    backTop() {
      let that = this;
      let timer = setInterval(() => {
        let ispeed = Math.floor(-that.scrollTop / 5);
        document.documentElement.scrollTop = document.body.scrollTop =
          that.scrollTop + ispeed;
        if (that.scrollTop === 0) {
          clearInterval(timer);
        }
      }, 16);
    }
  },
  mounted() {
    if (
        this.$route.query.Labelid ||
        this.$route.query.seriesid ||
        this.$route.query.releasetimes || 
        this.$route.query.search
      ) {
          if (this.$route.query.Labelid) {
          this.Labelid = this.$route.query.Labelid;
          this.search = "";
          this.releasetimes = "";
          this.seriesid = "";
          this.getLabelarticlelist();
          this.thispage = 1;
        }
           //当series发生变化
        if (this.$route.query.seriesid) {
          this.seriesid = this.$route.query.seriesid;
          this.Labelid = "";
          this.releasetimes = "";
          this.search = "";
          this.getseriesarticlelist();
        }
        if (this.$route.query.releasetimes) {
          this.releasetimes = this.$route.query.releasetimes;
          this.Labelid = "";
          this.seriesid = "";
          this.search = "";
          this.gettimearticlelist();
        }
        if (this.$route.query.search) {
          this.search = this.$route.query.search;
          this.Labelid = "";
          this.seriesid = "";
          this.releasetimes = "";
          this.getsearcharticlelist();
        } 
        }
     //无参数时
      else {
        this.seriesid = "";
        this.Labelid = "";
        this.getallarticlelist();
      }  
  },
  //监听
  watch: {
    $route(to, from) {
      //监听路由是否变化
      //有参数时
      if (
        to.query.Labelid ||
        to.query.seriesid ||
        to.query.releasetimes ||
        to.query.search
      ) {
        //当label变化
        if (to.query.Labelid) {
          this.Labelid = to.query.Labelid;
          this.search = "";
          this.releasetimes = "";
          this.seriesid = "";
          this.getLabelarticlelist();
          this.thispage = 1;
        }
        //当series发生变化
        if (to.query.seriesid) {
          this.seriesid = to.query.seriesid;
          this.Labelid = "";
          this.releasetimes = "";
          this.search = "";
          this.getseriesarticlelist();
        }
        if (to.query.releasetimes) {
          this.releasetimes = to.query.releasetimes;
          this.Labelid = "";
          this.seriesid = "";
          this.search = "";
          this.gettimearticlelist();
        }
        if (to.query.search) {
          this.search = to.query.search;
          this.Labelid = "";
          this.seriesid = "";
          this.releasetimes = "";
          this.getsearcharticlelist();
        }
      }
      //无参数时
      else {
        this.seriesid = "";
        this.Labelid = "";
        this.getallarticlelist();
      }
    }
  }
};
</script>
<style scoped>
.tag {
  color: #000;
}
.post-content {
  overflow: hidden;
  padding-top: 16px;
  padding-bottom: 5px;
  border-radius: 10px;
  margin: 13px 4px;
  transition: all 0.28s;
  box-shadow: 0 6px 12px 0 rgba(0, 0, 0, 0.15);
  width: 770px;
  position: relative;
  height: 156px;
}
.post-content:hover {
  cursor: pointer;
   opacity: 1;
  background-color: #FFFFFF;
}
.content-title {
  font-size: 16px;
}
.pagef {
  color: white;
}
p {
  margin-top: 0px;
}
.ye {
  cursor: pointer;
}
.picture {
  width: 220px;
  margin-left: 20px;
}
.cont {
  margin-left: 1%;
  height: 120px;
  overflow: hidden;
}
.cont p {
  margin-bottom: 0px;
}
.content {
  font-size: 14px;
  height: 80px;
}
.recommend {
  margin-left: 10px;
}
.pagepagin-list {
  float: left;
  width: 100%;
  height: 100px;
  margin: 0 auto;
}
@media screen and (min-width: 800px) {
  .ye-pc {
    display: none;
  }
  .post-content {
    margin-top: 0px;
  }
  .pagepagin-list {
    margin-bottom: 30px;
  }
  .post-content-footer {
    margin-top: -26px;
    min-height: 5px;
  }
  .cont{
    float: right;
    width: 63%;
}
}
@media screen and (max-width: 800px) {
  .ll {
    position: relative;
    right: 80px;
  }
  .tag {
    float: left;
    width: 200px;
  }
  .tag-time {
    margin-left: -250px;
    
  }
  .post-content-footer span {
    float: left;
  }
  .content {
    margin-left: 10px;
    margin-right: 10px;
  }
  .ye {
    display: none;
  }
  .bot_page {
    margin-left: 40%;
  }
  @media screen and (max-width: 540px) {
    .picture {
      display: none;
    }
    .post-tag {
      display: none;
    }
    .post-content {
      width: 98%;
    }
    .cont {
      width: 100%;
      float: right;
    }
    /* .tag-time{
      margin-top: 123px;
    } */
    .ipad {
      position: absolute;
      left: 60%;
      top: 136px;
      margin-right: 10px;
    }
  }
  @media (min-width: 540px) and (max-width: 800px) {
    .post-content-footer {
      margin-top: 1px;
      min-height: 5px;
    }
    
    .ipad {
      position: absolute;
      left: 80%;
      top: 140px;
      margin-right: 10px;
    }
.post-content .cont{
    float: right;
    width: 70%;
}
    .post-content {
      width: 98%;
    }
  }
  .picture {
    width: 25%;
  }

  .post-tag {
    margin-right: 5%;
  }
}
</style>