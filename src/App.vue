<template>
    <div id="app">
        <!--头部 on-->
        <div class="is-header">
            <div class="m-top visible-lg visible-md">
                <div class="container f-clearfix">
                    <div class="u-topl f-fl">
                        欢迎访问
                        <a href="/">{{ siteInfo.name }}</a>&nbsp;&nbsp;&nbsp;&nbsp;
                    </div>
                </div>
            </div>

            <div class="m-head f-lg-ha">
                <div class="container f-clearfix">
                    <a href="/" class="m-logo f-db f-fl">
                        <img :src="siteInfo.logo" alt="" style="max-height: 90px;overflow: hidden">
                    </a>
                    <nav class="navbar navbar-default f-fl" role="navigation">
                        <div class="container-fluid">
                            <div class="navbar-header f-fl">
                                <button type="button" class="navbar-toggle" data-toggle="collapse"
                                        data-target="#example-navbar-collapse">
                                    <span class="sr-only">切换导航</span>
                                    <span class="icon-bar"></span>
                                    <span class="icon-bar"></span>
                                    <span class="icon-bar"></span>
                                </button>
                                <a class="navbar-brand" @click="$router.push('/')">网站首页</a>
                            </div>

                            <div class="collapse navbar-collapse f-fl" id="example-navbar-collapse">
                                <ul class="nav navbar-nav">
                                    <li v-for="(item, index) in cateTree">
                                        <a @click="handleNavigate(item, -1)"  target="_blank" v-if="item.childs === undefined || item.childs === null">{{ item.name }}</a>

                                        <a href="#" @mouseover="showDropdown(index)" @mouseout="hideDropdown(index)" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false" v-if="item.childs !== undefined && item.childs.length !== 0">
                                            {{ item.name }} <span class="caret"></span>
                                        </a>
                                        <ul class="dropdown-menu" v-if="item.childs !== undefined && item.childs.length !== 0" style="background-color: #fff;text-align: center">
                                            <li>
                                                <a href="#" v-for="(child, childIndex) in item.childs" :key="childIndex" @click="handleNavigate(item, childIndex)">
                                                    {{ child.name }}
                                                </a>
                                            </li>
                                        </ul>
                                    </li>
<!--                                    <li>-->
<!--                                        <a @click="handleNavigateAbout" target="_blank">关于我们</a>-->
<!--                                        <dl class="f-dn">-->
<!--                                        </dl>-->
<!--                                    </li>-->
                                </ul>
                            </div>
                        </div>
                    </nav>
                </div>
            </div>
        </div>
        <!--头部 end-->

        <!--主体 on-->
        <router-view style="z-index: -1" v-if="flag"/>
        <!--主体 end-->

        <!--底部 on-->
        <div class="is-footer">
            <div class="container f-clearfix">
                <div class="m-footerl f-fl">
                    <p>
<!--                        <a href="" target="_blank">法律声明</a><a href="/sitemap/" target="_blank"> | 网站地图 </a>-->
                        <a href="/about" target="_blank">联系我们</a>
<!--                        <a href="/cmsAdmin" target="_blank">| 管理后台</a>-->
                    </p>
                    <p>版权所有：{{ siteInfo.name }}&nbsp;&nbsp;&nbsp; 地址：{{ siteInfo.address }} &nbsp; &nbsp; &nbsp;
                        电话：{{ siteInfo.phone }}</p>
                    <p><a href="https://beian.miit.gov.cn/" target="_blank">{{ siteInfo.beian }}</a></p>
                </div>
            </div>
        </div>
        <!--底部 end-->
    </div>
</template>

<script>
import * as api from '@/api/api'

export default {
    name: 'App',
    components: {},
    data() {
        return {
            siteInfo: {},
            cateTree: [],
            flag: false
        }
    },
    created() {
        Promise.all([
            api.siteItem({siteId: this.$store.getters.siteId}),
            api.cateTree({siteId: this.$store.getters.siteId})
        ]).then(res => {
            this.$router.push('/')
            res[0].banner = JSON.parse(res[0].banner)
            this.$store.commit('SET_SITEINFO', res[0])
            this.$store.commit('SET_CETELIST', res[1])
            sessionStorage.setItem("SITEINFO", JSON.stringify(res[0]))
            sessionStorage.setItem("CETELIST", JSON.stringify(res[1]))
            this.siteInfo = res[0]
            this.cateTree = res[1]
            this.flag = true
        })
    },
    methods: {
        handleNavigate(item, index) {
            this.$store.commit('SET_ACTIVEID', item.id)
            this.$store.commit('SET_ACTIVEIDDEX', index)
            this.$router.push({
                path: `/channel/${item.id}`,
            })
        },
        handleNavigateAbout() {
            this.$router.push({
                path: `/about`,
            })
        },
      showDropdown(index) {
        this.activeIndex = index;
      },
      hideDropdown() {
        this.activeIndex = -1;
      },
    }
}
</script>

<style>
@import "assets/bootstrap-3.3.7/css/bootstrap.min.css";
@import "assets/css/home.css";
@import "assets/css/page.css";
@import "assets/css/public.css";

#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
}

.dropdown:hover > .dropdown-menu {
  display: block;
}

.dropdown > .dropdown-toggle:active {
  pointer-events: none;
}
</style>
