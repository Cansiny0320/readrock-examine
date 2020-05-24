<template>
  <div id="app">
    <!-- S header -->
    <header>
      <div class="header__top">
        <div class="hamburger">
          <i class="fas fa-list"></i>
        </div>
        <div class="logo">V2EX</div>
        <div class="search">
          <i class="fas fa-search"></i>
        </div>
      </div>
      <nav>
        <ul>
          <li :class="{active:scViewStatus[0]} " @click="showHiddenView(0)">
            最新
            <i class="fas fa-caret-down"></i>
          </li>
          <li @click="showHiddenView(1);getHotContent()" :class="{active:scViewStatus[1]} ">热门</li>
          <li @click="showHiddenView(2)" :class="{active:scViewStatus[2]} ">节点</li>
        </ul>
      </nav>
    </header>
    <!-- E header -->
    <!-- S 帖子 -->
    <!-- S 全部 -->
    <div class="content-wrapper" v-show="scViewStatus[0]" :class="{scroll: !isViewing}">
      <div class="content-item" v-for="item in contentList" :key="item.node.name">
        <div class="item__user">
          <div class="user__avatar">
            <img :src="item.member.avatar_normal" />
          </div>
          <div class="user__nickname">{{ item.member.username }}</div>
          <div
            class="user__latest-reply-time"
          >{{Math.round((new Date().getTime()/1000 - item.last_modified) /60)}}分钟前</div>
          <div class="user__reply-number">评论{{item.replies}}</div>
        </div>
        <div class="item__origin">
          <span>{{ item.node.title }}</span>
        </div>
        <div
          class="item__title"
          @click="viewContent({rendered:item.content_rendered,avatar:item.member.avatar_normal,userName:item.member.username,origin:item.node.title,title:item.title})"
        >
          <a href="#">{{ item.title }}</a>
        </div>
      </div>
      <div class="content" v-show="isViewing">
        <div class="back" @click="back">
          <i class="fas fa-chevron-left"></i>
        </div>
        <div class="info">
          <div class="item__user">
            <div class="user__avatar">
              <img :src="content.avatar" />
            </div>
            <div class="user__nickname">{{ content.userName }}</div>
            <div class="item__origin">
              <span>{{ content.origin }}</span>
            </div>
          </div>
        </div>
        <h1 class="content-title">{{ content.title }}</h1>
        <div class="content-detail" v-html="content.rendered"></div>
      </div>
    </div>
    <!-- E 全部 -->
    <!-- S 热门 -->
    <div
      class="content-wrapper"
      v-show="scViewStatus[1]"
      style="display:none"
      :class="{scroll: !isViewing}"
    >
      <div class="content-item" v-for="item in hotContentList" :key="item.node.name">
        <div class="item__user">
          <div class="user__avatar">
            <img :src="item.member.avatar_normal" />
          </div>
          <div class="user__nickname">{{ item.member.username }}</div>
          <div
            class="user__latest-reply-time"
          >{{Math.round((new Date().getTime()/1000 - item.last_touched) /60)}}分钟前</div>
          <div class="user__reply-number">评论{{item.replies}}</div>
        </div>
        <div class="item__origin">
          <span>{{ item.node.title }}</span>
        </div>
        <div
          class="item__title"
          @click="viewContent({rendered:item.content_rendered,avatar:item.member.avatar_normal,userName:item.member.username,origin:item.node.title,title:item.title})"
        >{{ item.title }}</div>
      </div>
      <div class="content" v-show="isViewing">
        <div class="back" @click="back">
          <i class="fas fa-chevron-left"></i>
        </div>
        <div class="info">
          <div class="item__user">
            <div class="user__avatar">
              <img :src="content.avatar" />
            </div>
            <div class="user__nickname">{{ content.userName }}</div>
            <div class="item__origin">
              <span>{{ content.origin }}</span>
            </div>
          </div>
        </div>
        <h1 class="content-title">{{ content.title }}</h1>
        <div class="content-detail" v-html="content.rendered"></div>
      </div>
    </div>
    <!-- S 热门 -->
    <!-- S 节点 -->
    <div class="node-wrapper" v-show="scViewStatus[2]" style="display:none">
      <div class="node__item">
        <div class="type-title">分享与探索</div>
        <div class="node">
          <div class="node__title">问与答</div>
          <div class="node__title">分享发现</div>
          <div class="node__title">分享创造</div>
          <div class="node__title">奇思妙想</div>
          <div class="node__title">分享邀请码</div>
          <div class="node__title">自言自语</div>
          <div class="node__title">随想</div>
          <div class="node__title">设计</div>
          <div class="node__title">Blog</div>
        </div>
      </div>
      <div class="node__item">
        <div class="type-title">V2EX</div>
        <div class="node">
          <div class="node__title">V2EX</div>
          <div class="node__title">DNS</div>
          <div class="node__title">反馈</div>
          <div class="node__title">Project Babel</div>
          <div class="node__title">使用指南</div>
        </div>
      </div>
      <div class="node__item">
        <div class="type-title">前端开发</div>
        <div class="node">
          <div class="node__title">Chrome</div>
          <div class="node__title">Vue.js</div>
          <div class="node__title">CSS</div>
          <div class="node__title">Firefox</div>
          <div class="node__title">React</div>
          <div class="node__title">Angular</div>
          <div class="node__title">Flutter</div>
          <div class="node__title">Edge</div>
          <div class="node__title">Web Dev</div>
          <div class="node__title">Lonic</div>
        </div>
      </div>
      <div class="node__item">
        <div class="type-title">机器学习</div>
        <div class="node">
          <div class="node__title">机器学习</div>
          <div class="node__title">数学</div>
          <div class="node__title">TensorFlow</div>
          <div class="node__title">自然语言处理</div>
          <div class="node__title">CUDA</div>
          <div class="node__title">Torch</div>
          <div class="node__title">Core ML</div>
          <div class="node__title">Keras</div>
        </div>
      </div>
      <div class="node__item">
        <div class="type-title">编程语言</div>
        <div class="node">
          <div class="node__title">Python</div>
          <div class="node__title">PHP</div>
          <div class="node__title">Java</div>
          <div class="node__title">JavaScript</div>
          <div class="node__title">Node.js</div>
          <div class="node__title">Go</div>
          <div class="node__title">HTML</div>
          <div class="node__title">Swift</div>
          <div class="node__title">Ruby on Rails</div>
          <div class="node__title">.NET</div>
          <div class="node__title">Ruby</div>
          <div class="node__title">C#</div>
          <div class="node__title">Rust</div>
          <div class="node__title">Kotlin</div>
          <div class="node__title">Lua</div>
          <div class="node__title">TypeScript</div>
        </div>
      </div>
      <div class="node__item">
        <div class="type-title">编程语言</div>
        <div class="node">
          <div class="node__title">Python</div>
          <div class="node__title">PHP</div>
          <div class="node__title">Java</div>
          <div class="node__title">JavaScript</div>
          <div class="node__title">Node.js</div>
          <div class="node__title">Go</div>
          <div class="node__title">HTML</div>
          <div class="node__title">Swift</div>
          <div class="node__title">Ruby on Rails</div>
          <div class="node__title">.NET</div>
          <div class="node__title">Ruby</div>
          <div class="node__title">C#</div>
          <div class="node__title">Rust</div>
          <div class="node__title">Kotlin</div>
          <div class="node__title">Lua</div>
          <div class="node__title">TypeScript</div>
        </div>
      </div>
      <div class="node__item">
        <div class="type-title">编程语言</div>
        <div class="node">
          <div class="node__title">Python</div>
          <div class="node__title">PHP</div>
          <div class="node__title">Java</div>
          <div class="node__title">JavaScript</div>
          <div class="node__title">Node.js</div>
          <div class="node__title">Go</div>
          <div class="node__title">HTML</div>
          <div class="node__title">Swift</div>
          <div class="node__title">Ruby on Rails</div>
          <div class="node__title">.NET</div>
          <div class="node__title">Ruby</div>
          <div class="node__title">C#</div>
          <div class="node__title">Rust</div>
          <div class="node__title">Kotlin</div>
          <div class="node__title">Lua</div>
          <div class="node__title">TypeScript</div>
        </div>
      </div>
    </div>
    <!-- E 节点 -->
    <!-- E 帖子 -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      contentList: [],
      hotContentList: [],
      scViewStatus: [true, false, false],
      content: {
        avatar: "",
        userName: "",
        title: "",
        rendered: "",
        origin: ""
      },
      isViewing: false
    };
  },
  methods: {
    sendXhr: function({
      method = "GET",
      url,
      async = true,
      success,
      error = () => {
        console.log("error");
      },
      beforeSuccess = null,
      afterSuccess = null
    } = {}) {
      const xhr = new XMLHttpRequest();
      xhr.onreadystatechange = function() {
        if (xhr.readyState == 1 && beforeSuccess) beforeSuccess();
        if (xhr.readyState == 4) {
          if (xhr.status >= 200 && xhr.status < 300) {
            let res = JSON.parse(xhr.responseText);
            success(res);

            afterSuccess && afterSuccess();
          } else {
            error();
          }
        }
      };
      xhr.open(method, url, async);
      xhr.send();
    },
    getContentList: function(res) {
      this.contentList = res;
    },
    showHiddenView(index) {
      let _this = this;
      for (let i = 0; i < this.scViewStatus.length; i++) {
        this.$set(this.scViewStatus, i, false);
      }
      this.$set(this.scViewStatus, index, true);
      //第一个参数为数组，第二个参数为数组下标，第三个参数为设置的值，
    },
    getHotContent: function() {
      let _this = this;
      this.sendXhr({
        method: "GET",
        url: `/api/api/topics/hot.json`,
        async: true,
        success: res => {
          _this.hotContentList = res;
        }
      });
    },
    viewContent: function({ rendered, avatar, userName, origin, title } = {}) {
      this.content.rendered = rendered;
      this.content.avatar = avatar;
      this.content.userName = userName;
      this.content.origin = origin;
      this.content.title = title;
      this.isViewing = true;
    },
    back: function() {
      this.isViewing = false;
    }
  },
  created() {
    let _this = this;
    this.sendXhr({
      method: "GET",
      url: `/api/api/topics/latest.json`,
      async: true,
      success: _this.getContentList
    });
  }
};
</script>



<style lang="scss">
@import url(https://cdn.bootcdn.net/ajax/libs/font-awesome/5.13.0/css/all.css);
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
:root {
  --main-text-color: #3a3a3a;
  --secondary--text-color: #9a9a9a;
}
body {
  background-color: #eee;
}
img {
  width: 100%;
}
a {
  text-decoration: none;
  color: #333;
  &:link,
  &:visited {
    color: #333;
  }
}
#app {
  background-color: #fff;
  width: 500px;
  margin: 0 auto;
  &::-webkit-scrollbar {
    display: none;
  }
}
/* header */
header {
  color: var(--main-text-color);
  background-color: #f4f4f4;
  -webkit-user-select: none;
  .header__top {
    padding: 10px 20px 0;
    display: flex;
    justify-content: space-between;
    align-items: center;
    cursor: pointer;
    .logo {
      font-size: 24px;
      font-weight: bolder;
      cursor: unset;
    }
  }
  nav {
    ul {
      padding: 20px 0 10px;
      color: var(--secondary--text-color);
      list-style: none;
      display: flex;
      justify-content: space-evenly;
      align-items: center;
      cursor: pointer;
      li {
        &:hover {
          color: var(--main-text-color);
        }
        &.active {
          position: relative;
          color: var(--main-text-color);
          &::after {
            content: "";
            display: block;
            position: absolute;
            height: 3px;
            width: 90%;
            background-color: var(--main-text-color);
            bottom: -10px;
            left: 5%;
            border-radius: 10px;
          }
        }
      }
    }
  }
}
/* header */
/* S 帖子 */
.scroll {
  overflow: unset;
  overflow-y: auto !important;
  overflow-x: hidden !important;
}
.content-wrapper {
  padding: 20px;
  width: 100%;
  max-height: calc(100vh - 98px);
  color: var(--main-text-color);
  position: relative;
  overflow: hidden;
  &::-webkit-scrollbar {
    display: none;
  }
  .content {
    width: 500px;
    height: 100vh;
    position: fixed;
    overflow-y: auto;
    overflow-x: hidden;
    top: 0;
    left: calc(50% - 250px);
    padding: 20px;
    background-color: #fff;
    z-index: 10;
    &::-webkit-scrollbar {
      display: none;
    }
    .back {
      cursor: pointer;
    }
    .content-detail {
      padding: 10px 0 0;
    }
    .info {
      width: 100%;
      .item__user {
        width: 100%;
        display: flex;
        align-items: center;
        .user__avatar {
          margin-right: 20px;
        }
        .item__origin {
          margin-left: auto;
          span {
            background-color: #f5f5f5;
            padding: 5px;
            color: var(--main-text-color);
            font-size: 14px;
          }
        }
      }
    }
  }
  .content-item {
    width: 100%;
    display: grid;
    position: relative;
    grid-template-areas:
      "user user . . origin"
      "title title title title title";
    grid-template-columns: 2fr repeat(4, 1fr);
    grid-template-rows: 1fr 1fr;
    padding: 0 0 20px;
  }
}
.content-item {
  user-select: none;
  .item__user {
    grid-area: user;
    display: grid;
    grid-template-areas:
      "avatar nickname nickname"
      "avatar time number";
    grid-template-columns: 50px repeat(2, 1fr);
    grid-template-rows: 1fr 1fr;
    column-gap: 5px;
  }
  .item__origin {
    grid-area: origin;
    justify-self: center;
    span {
      background-color: #f5f5f5;
      padding: 5px;
      color: var(--main-text-color);
      font-size: 14px;
    }
  }
  .item__title {
    grid-area: title;
    line-height: 25px;
    cursor: pointer;
  }
}
.item__user {
  .user__avatar {
    grid-area: avatar;
    img {
      width: 50px;
      height: 50px;
      border-radius: 100%;
      object-fit: cover;
    }
  }
  .user__nickname {
    grid-area: nickname;
  }
  .user__latest-reply-time {
    grid-area: time;
  }
  .user__reply-number {
    margin-left: -30px;
    grid-area: number;
  }
}
.item__user .user__reply-number,
.item__user .user__latest-reply-time {
  color: var(--secondary--text-color);
  font-size: 12px;
}

.node-wrapper {
  padding: 20px;
  .node__item {
    margin-bottom: 10px;
  }
  .node {
    display: flex;
    flex-wrap: wrap;
    justify-content: flex-start;
    position: relative;
    &::after {
      content: "";
      display: block;
      position: absolute;
      top: -20px;
      left: -5px;
      height: 20px;
      border-radius: 4px;
      width: 2px;
      background-color: #333;
    }
    .node__title {
      user-select: none;
      cursor: pointer;
      padding: 5px;
      margin-right: 5px;
      background-color: #eee;
      margin-top: 5px;
      font-size: 14px;
    }
  }
}
/* E 帖子 */
</style>
