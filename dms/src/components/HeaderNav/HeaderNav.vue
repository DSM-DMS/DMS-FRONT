<template>
  <div id="header-wrapper" :class="{mini: mini}" @click="mini = !mini" v-bind:style="menu ? { width: 'calc(100% - 350px)', 'border-top-right-radius' : '0', 'border-bottom-right-radius' : '0'} : { width: 'calc(100% - 100px)'}">
    <div id="brand-wrapper">
      <img id="header-ic-logo" src="../../assets/icon/ic_logo.png" @click.stop="$router.push('/')"/>
    </div>

    <div id="header-menu-wrapper">
      <span class="header-menu" :class="{'header-menu-responsive': menu}" @click.stop="scrollRouting('/')">메인화면</span>
      <span class="header-menu" :class="{'header-menu-responsive': menu}" @click.stop="scrollRouting('/apply')">신청화면</span>
      <span class="header-menu" :class="{'header-menu-responsive': menu}" @click.stop="scrollRouting('/notice')">공지사항</span>
      <span class="header-menu" :class="{'header-menu-responsive': menu}" @click.stop="loginButton">{{ !$store.getters.isLogin? '로그인': '로그아웃' }}</span>
      <div id="menu-btn-wrapper" @click.stop="menuButton">
        <span>메뉴</span>
        <img id="header-ic-menu" :src="menu ? require('../../assets/Main_menu/ic_navigation/ic_nav_back.png') : require('../../assets/Main_menu/ic_navigation/ic_menu.png')" />
      </div>
    </div>

    <menu-section :menu="menu" @update:menu="val => menu = val"/>

    <login-modal v-if="computedLoginModal" @close="loginModal = false"/>
  </div>
</template>

<script>
import LoginModal from '@/components/HeaderNav/LoginModal'
import MenuSection from '@/components/MenuSection/MenuSection'

export default {
  name: 'HeaderNav',
  components: {
    LoginModal,
    MenuSection
  },
  data: function () {
    return {
      loginModal: false,
      menu: false,
      mini: true
    }
  },
  methods: {
    loginButton: function () {
      if (this.$store.getters.isLogin) {
        this.$store.dispatch('logout')
        this.loginModal = false
      } else {
        this.loginModal = true
      }
    },
    menuButton: function () {
      this.menu = !this.menu
    },
    scrollRouting: function (path) {
      if (this.$route.path === path) {
        document.querySelector(this.$route.meta.scroll.selector).scrollIntoView()
      } else {
        this.$router.replace(path)
      }
    }
  },
  computed: {
    computedLoginModal: function () {
      return this.$store.getters.isLogin ? false : this.loginModal
    }
  }
}
</script>

<style>
/*header*/
@media screen and (min-width: 1025px){
  .header-menu {
    margin-right: 60px; 
  }
}

@media screen and (max-width: 1024px){
  .header-menu {
    margin-right: 30px; 
  }
}

@media screen and (max-width: 660px) {
  .header-menu:not(:last-of-type) {
    display: none;
  }
}

@media screen and (max-width: 950px) {
  .header-menu-responsive {
    display: none;
  }
}

@media screen and (max-height: 700px) {
  #header-wrapper {
    top: 0px !important;
    cursor: pointer;
    will-change: top;
    transition: top .2s ease-out !important;
  }

  #header-wrapper.mini {
    top: -70px !important;
  }

  #header-wrapper.mini:hover {
    top: -60px !important; 
  }
}

#header-wrapper { 
  font-family: 'NanumSquareB';
  margin: 20px 50px 0 50px;
  position: fixed;
  top: 0;
  z-index: 10;
  height: 60px;
  border-radius: 10px;
  background-color: white;
  box-shadow: 4px 4px rgba(126, 126, 126, 0.6);
  transition: .4s width ease-out;
}

.header-wrapper-isMenu{
  width: calc(100% - 400px)
}

#brand-wrapper {
  display: inline-block;
  margin: 15px 10px 15px 10px;
  cursor: pointer;
}

#header-menu-wrapper {
  float: right;
  margin-right: 20px;
  margin-top: 20px;
}

#header-menu-wrapper:nth-child(n) {
  cursor: pointer;
}

#header-ic-menu {
  width: 15px;
}

#header-ic-logo {
  height: 30px;
}

#menu-btn-wrapper{
  display: inline-block;
}

#menu-btn-wrapper span {
  font-family: 'Nanum Square';
  font-weight: bold;
  font-size: 15px;  
}

.header-menu {
  display: inline-block;
  transition: transform .15s ease-out;
}

.header-menu:hover {
  transition: transform .15s ease-out;
  transform: scale(1.15);
}
</style>