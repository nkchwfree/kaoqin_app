<template>
	<view>
		<main-head/>
		<view class="banner">
			
			<view v-for="(item,index) in lists" :key="index"  class="panel">
				<navigator :open-type="item.openType" :url="item.routerPath" >
					<view class="imgbox"><image :src="item.imgPath" /></view>
			        <text class="text">{{item.title}}</text>
				 </navigator>
			</view>
		</view>
		
	</view>
</template>

<script>
	
import mainHead from "@/components/main/main-head.vue"
export default {
	components: {mainHead},
	data() {
	  return {
		lists: [{
			title:'请 假',
			openType:'navigate',
			routerPath:'../vacation/vacation',
			imgPath: '../../static/qingjia.png'
		},{
			title:'加 班',
			openType:'navigate',
			routerPath:'../jiaban/jiaban',
			imgPath: '../../static/chucha.png'
		},{
			title:'补 卡',
			openType:'navigate',
			routerPath:'../buka/buka',
			imgPath: '../../static/location_t.png'
		},{
			title:'审批管理',
			openType:'navigate',
			routerPath:'../approval/approval',
			imgPath: '../../static/approved.png'
		},{
			title:'假期余额',
			openType:'navigate',
			routerPath:'../achievements/achievements',
			imgPath: '../../static/jixiao.png'
		},{
			title:'排班信息',
			openType:'navigate',
			routerPath:'../goOut/goOut',
			imgPath: '../../static/waichu.png'
		},{
			title:'工资单',
			openType:'switchTab',
			routerPath:'../record/record',
			imgPath: '../../static/search_t.png'
		},{
			title:'今日提醒',
			openType:'switchTab',
			routerPath:'../record/record',
			imgPath: '../../static/search_t.png'
		},{
			title:'修改密码',
			openType:'switchTab',
			routerPath:'../record/record',
			imgPath: '../../static/search_t.png'
		}]
		
	 }
	},
	methods: {
			
	}
 }
</script>

<style scoped>
.banner:after {
            content: "";
            width: 30%;
            height: 0px;
            visibility: hidden;
        }
.banner {
	display: flex;  
    flex-wrap: wrap;
	text-align: center;
	padding: 0 40upx;
	justify-content: space-between;
	}
.panel {
	display: flex;
	justify-content: center;
	align-items: center;
	width: 200upx;
	height: 200upx;
	margin-bottom: 60upx;
    justify-content: center;
	border-radius: 15%;
	/* box-shadow: 0 0 3px #00a0e9; */
	/* background-color: rgba(0,160, 233, 0.6); */
    /* background-color: rgba(162, 217, 192, 0.2); */
  }
  .imgbox {
	 display: flex;
	 justify-content: center;
	 align-items: center;
	 width: 110upx;
	 height: 110upx;
	 line-height: 150upx;
	 border-radius: 8px;
	 background-color: #00a0e9;
  }
  .imgbox image {
	  width: 70upx;
	  height: 70upx;
	  margin: 0;
	  padding: 0;
	  box-sizing: border-box;
  }
  .text {
    text-align: center;
  }
</style>
