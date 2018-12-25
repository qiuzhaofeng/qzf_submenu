<template>
	<div class='system-sideMenu'>
		<!-- :default-openeds='defaultOpeneds' 当前打开的 sub-menu 的 index 的数组-->
    <!-- :default-active="defaultActive" 当 前激活菜单的 index-->
    <!-- unique-opened 去除此属性，可同时展开多个，保持和tms一致 -->
    <!-- router	属性可使菜单按照index属性切换路由，但是效果被加在渲染出来的li上面,此种方式路由状态
    不好添加， 不加router,下面用router-link包 裹 ,此种没毛病，只是点击router-link渲染出来的a才会触发，修改样式即可
    既加router,又加router-link,是为了点击el-menu-item渲染出来的li也可触发（路由不变），-->
    <el-menu
      background-color="#ffffff"
      text-color="#000"
      :default-active="defaultActive"
    >
      <!-- 此处index必填，但是并没有起到导航效果，因为效果是加在渲染出来的li上，也就是el-menu-item,但是需要和
        :default-openeds对应才能展开指定的二级导航-->
      <el-submenu :index="`/${sideNav.path}/${item.path}`" v-for="(item,index) in sideNav.sublist" :key="index">
        <template slot="title"> 
          <!-- 左侧logo -->
          <img :src="item.icon" alt="" class="iconImg">
          <!-- 这里可使用router-link代替点击二级导航跳转路由，这里tms保持tms一致，不加 -->
          <!-- <router-link :to="item.path" class="nav-link"> -->
          <!-- <router-link :to="`/${sideNav.path}/${item.path}`" class="thirdNav"> -->
          <span>{{item.name}}</span>
          <!-- </router-link> -->
        <!-- <template><i class="el-submenu__icon-arrow el-icon-arrow-down">aa</i></template> -->
        </template>
        <!-- 此处index必填，无用 -->
        <el-menu-item :index="`/${sideNav.path}/${item.path}/${itemCont.path}`" v-for="(itemCont,index) in item.sublist" :key="index" v-if="itemCont.is_show=='1'">
          <!-- 此处可以设置当前激活的样式 -->
          <router-link :to="{name:`${itemCont.path}`}" class="thirdNav">
            {{itemCont.name}}
          </router-link>
        </el-menu-item>
      </el-submenu>
    </el-menu>
	</div>
</template>
<script>
import store from '@/store'
import { mapGetters} from 'vuex'
export default {
	name: 'sideMenu',
	data () {
		return {
      sideNav:{},
			defaultActive:''
		}
	},
	computed: {
    // ...mapGetters({
		// 	get_side_menu: GET_SIDE_MENU
		// })
  },
  created () {
    this.sideNav = this.$store.state.side_menu
    // console.log(this.$store.state.side_menu,456)
    // if(this.$store.state.side_menu.length !=0) {
    //   this.sideNav = this.$store.state.side_menu
    // } else {
    //   setTimeout(()=>{
    //     this.sideNav = this.$store.state.side_menu
    //   },500)
    // }
    // 左侧激活及展开项
		this.defaultActive = this.$route.path
  },
	methods: {

	},
	watch: {

	},
}
</script>
<style lang="less">
.system-sideMenu {
  float: left;
  width: 168px;
  height: 100%;
  .el-menu {
    border-right: none;
    // height: calc(~'100% - 48px');
    height: 100%;
    overflow: hidden;
    overflow-y: auto;
    .el-submenu {
      &.is-active {
        .el-submenu__title {
          background-color: #EDF1F5 !important;
        }
      }
      .el-submenu__title {
        padding-left: 50px !important;
        font-size: 14px;
        height: 40px;
        line-height: 40px;
        position: relative;
        .iconImg {
          position: absolute;
          top: 13px;
          left: 24px;
          height: 16px;
          width: 16px;
        }
        .el-submenu__icon-arrow {
          font-size: 18px;
          font-weight: 700;
          margin-top: -8px;
          i {
            color: #c3c8d2;
          }
        }
        &:hover {
          background-color: #EDF1F5 !important;
        }
      }
      .el-menu {
        .el-menu-item {
          padding: 0px !important;
          min-width: 0px;
          height: 34px;
          line-height: 34px;
          // text-align: center;
          padding-left: 50px !important;
          .thirdNav {
            font-size: 12px;
            color: #6A6A6A;
            display: block;
            width: 100%;
            height: 100%;
            &.router-link-exact-active {
              color: #0052CC !important;
            }
          }
          &:hover {
            background-color: rgb(255,255,255) !important;
            a {
              color: #0052CC;
            }
          }
        }
      }
    }
  }
}
</style>