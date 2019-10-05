<template>
  <div id="tou">
    <header>
      <div class="nav">
        <router-link to="/homepage" id="menu_btn" class="target menu_btn">
          <i class="iconfont">&#xe632;</i>
          <span>主页</span>
        </router-link>
       <!-- <router-link to="/" id="menu_btn" class="target menu_btn">
          <i class="iconfont">&#xe632;</i>
          <span>关于</span>
        </router-link> -->
        <form class="search-form" autocomplete="off" @submit.prevent="onSubmit">
          <input
            type="text"
            v-model="search"
            placeholder="请输入你要搜索的关键字"
            class="searchW"
            @keyup.enter="seek"
          />
          <input class="searchB" type="submit" value="..  .." />
          <div class="search_cat" @click="seek">
            <div class="mao_box">
              <div class="mao">
                <div class="erduo">
                  <div></div>     
                  <div></div>
                </div>
                <div class="yanjing">
                  <div>
                    <div class="yanquan">
                      <div></div>
                    </div>
                    <div class="yanquan_hedding"></div>
                    <div class="hong"></div>
                  </div>
                  <div class="yan_right">
                    <div class="yanquan">
                      <div></div>
                    </div>
                    <div class="yanquan_hedding"></div>
                    <div class="hong"></div>
                  </div>
                  <div style="clear:both"></div>
                </div>
                <div class="bizi">
                  <div></div>
                  <div></div>
                  <div></div>
                  <div></div>
                </div>
                <div class="zuiba_box">
                  <div class="zuiba">
                    <div></div>
                    <div></div>
                    <div></div>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </form>
      </div>
    </header>
  </div>
</template>
<script>
export default {
  name: "tou",
  data() {
    return {
      search: "",
    };
  },
  methods: {
    onSubmit() {
      return false;
    },
    seek() {
      if(this.search)
      {
          this.$router.push({ path: "/list", query: { search: this.search } });
          this.search=""
      }
    }
  }
};
</script>
<style scoped>
a {
  color: #000;
}
.search-form {
  width: 33%;
}
@media screen and (max-width: 800px) {
  .man {
    display: none;
  }
  header .target {
    margin-left: 5px;
  }
  .searchW {
    margin-left: 0px;
  }
  .search-form {
    width: 250px;
  }
  .search_cat {
    left: 157px;
  }
}
</style>