<template>
	<el-menu class="navbar" mode="horizontal">
		<hamburger class="hamburger-container" :toggleClick="toggleSideBar" :isActive="sidebar.opened"></hamburger>
		<levelbar></levelbar>
		<tabs-view></tabs-view>
		<error-log v-if="log.length>0" class="errLog-container" :logsList="log"></error-log>
    <!--<div class="switch-lang">-->
      <!--<span :class="{ active:zhActive}" @click="switchLang('zh')">中文</span>-->
      <!--<span> / </span>-->
      <!--<span :class="{active:enActive}" @click="switchLang('en')">En</span>-->
    <!--</div>-->
		<el-dropdown class="avatar-container" trigger="click">
			<div class="avatar-wrapper">
        <span class="ad_name">{{name}}</span>
				<img class="user-avatar" :src="'/static/images/'+ad_level+'.jpg'">
				<i class="el-icon-caret-bottom"></i>
			</div>
			<el-dropdown-menu class="user-dropdown" slot="dropdown">
				<router-link class='inlineBlock' to="/">
					<el-dropdown-item>{{$t('utils.home')}}</el-dropdown-item>
				</router-link>
				<el-dropdown-item divided><span @click="logout" style="display:block;">{{$t('utils.logout')}}</span></el-dropdown-item>
			</el-dropdown-menu>
		</el-dropdown>
	</el-menu>
</template>

<script>
import Cookies from 'js-cookie'
import { mapGetters } from 'vuex'
import Levelbar from './Levelbar'
import TabsView from './TabsView'
import Hamburger from 'components/Hamburger'
import Screenfull from 'components/Screenfull'
import ErrorLog from 'components/ErrLog'
import errLogStore from 'store/errLog'

export default {
  components: {
    Levelbar,
    TabsView,
    Hamburger,
    ErrorLog,
    Screenfull
  },
  data() {
    return {
      log: errLogStore.state.errLog,
      zhActive: true,
      enActive: false
    }
  },
  computed: {
    ...mapGetters([
      'sidebar',
      'name',
      'ad_level'
    ])
  },
  created() {
    const lang = Cookies.get('lang') || 'zh'
    if (lang === 'zh') {
      this.zhActive = true
      this.enActive = false
    } else {
      this.zhActive = false
      this.enActive = true
    }
    this.switchLangInit()
  },
  methods: {
    toggleSideBar() {
      this.$store.dispatch('ToggleSideBar')
    },
    logout() {
      this.$store.dispatch('LogOut').then(() => {
        location.reload()// 为了重新实例化vue-router对象 避免bug
      })
    },
    switchLang(str) {
      if (str === 'zh') {
        if (this.zhActive) {
          return false
        } else {
//          locale.use(langEn)
//          Vue.locale('zh-cn', langZh)
          Cookies.set('lang', 'zh')
          this.zhActive = true
          this.enActive = false
          this.$i18n.locale = 'zh'
          location.reload()
        }
      }
      if (str === 'en') {
        if (this.enActive) {
          return false
        } else {
          Cookies.set('lang', 'en')
          this.zhActive = false
          this.enActive = true
          this.$i18n.locale = 'en'
          location.reload()
        }
      }
    },
    switchLangInit() {
      const $lang = Cookies.get('lang') || 'zh'
      this.$i18n.locale = $lang
    }
  }
}
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
	.navbar {
			height: 50px;
			line-height: 50px;
			border-radius: 0px !important;
			.hamburger-container {
					line-height: 58px;
					height: 50px;
					float: left;
					padding: 0 10px;
			}
			.errLog-container {
					display: inline-block;
					position: absolute;
					right: 150px;
			}
			.screenfull {
					position: absolute;
					right: 90px;
					top: 16px;
					color: red;
			}
      .switch-lang{
        display: inline-block;
        position: absolute;
        right: 130px;
        span{
          font-size: 18px;
          cursor: pointer;
        }
        .active{
          color: #409eff;
        }
      }
			.avatar-container {
					height: 50px;
					display: inline-block;
					position: absolute;
					right: 35px;
					.avatar-wrapper {
							cursor: pointer;
							margin-top: 5px;
							position: relative;
              .ad_name{
                vertical-align: top;
                padding-right: 5px;
              }
							.user-avatar {
									width: 40px;
									height: 40px;
									border-radius: 10px;
							}
							.el-icon-caret-bottom {
									position: absolute;
									right: -20px;
									top: 25px;
									font-size: 12px;
							}
					}
			}
	}
</style>



