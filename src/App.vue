<template>
  <div id="app">
    <view-box>
    		<x-header
    			class="header"
    			:left-options="{showBack: true,backText: 'nihhhjh'}"
    			title="网易">
    			<a slot="right">Feedback</a>
    			</x-header>
    			<scroller :on-refresh="refresh" :on-infinite="infinite" ref="myref">
    			 <sc
    			 	 :lock-y="true">
	    			 	<div class="tab">
			    			<tab>
						      <tab-item selected >已发货</tab-item>
						      <tab-item>未发货</tab-item>
						      <tab-item>全部订单</tab-item>
						      <tab-item>全部订单</tab-item>
						      <tab-item>全部订单</tab-item>
						      <tab-item>全部订单</tab-item>
						      <tab-item>全部订单</tab-item>
			  			  </tab>		  			      			 		
	    			 	</div>
  			    </sc>
    			
    			<swiper :list="list"  v-model="swiperItemIndex" :loop="true"></swiper>
    			
    			<marquee>
      			<marquee-item v-for="list in list3">{{list.title}}</marquee-item>
    			</marquee>
    			
    			<panel header="图文组合列表"  :list="list1" :type="type"></panel>
    			
    			</scroller>
    			<tabbar>
			      <tabbar-item>
			        <img slot="icon" src="./assets/icon_nav_article.png">
			        <span slot="label">Wechat</span>
			      </tabbar-item>
			       <tabbar-item>
			        <img slot="icon" src="./assets/icon_nav_article.png">
			        <span slot="label">Wechat</span>
			      </tabbar-item>
			       <tabbar-item>
			        <img slot="icon" src="./assets/icon_nav_article.png">
			        <span slot="label">Wechat</span>
			      </tabbar-item>
			       <tabbar-item>
			        <img slot="icon" src="./assets/icon_nav_article.png">
			        <span slot="label">Wechat</span>
			      </tabbar-item>
  			  </tabbar>
  			  
  			  
    </view-box>
    <router-view></router-view>
  </div>
</template>

<script>
import {ViewBox,XHeader} from 'vux';//组件引入
import { Tabbar,TabbarItem } from 'vux';
import { Tab, TabItem } from 'vux';
import { Scroller as sc } from 'vux';
import { Swiper } from 'vux';
import { Marquee, MarqueeItem} from 'vux';
import { Panel} from 'vux';

 var refreshkey=['A','B01','B02','B03','B04','B05','B06','B07','B08','B09','B10']
 
 var key=0;
 
 var keyValue="A";
 
 var start=0;
 
 var end=start+9;
 
 function getkey(){
 	  key++;
 	  if(key==refreshkey.length){
 	  	key=0;
 	  }
 	  keyValue=refreshkey[key];
 	  return keyValue;
 }
 var initloadered=false;
 
export default {
  name: 'app',
components: {
		ViewBox,
		XHeader,
		Tabbar,
		TabbarItem,
		Tab,
		TabItem,
		sc,
		Swiper,
		 Marquee,
    MarqueeItem,
    Panel
	},
	created(){//一上来就　调用ｊｓｏｎｐ
		 this.$jsonp('http://3g.163.com/touch/jsonp/sy/recommend/0-9.html').then(data => {
		 //	console.log(data)
		 	//console.log(this.list)
		 	//幻灯盘的数据
		 	this.list=data.focus.filter(item =>{
		 		return item.addata===null &&  item.picInfo[0];
		 	}).map(item => {//filter 是吧不需要的进行排除　　　通过　ｍａｐ方法　是把这个对象中的，每一个数据重新进行编排
		 		    return {
		 		    	url:item.link,
		 		    	img:item.picInfo[0].url,
		 		    	title:item.title
		 		    }
		 	})
		 	//首屏的新闻数据
		 	
		 	 	this.list1=data.list.filter(item =>{
		 		return item.addata===null &&  item.picInfo[0];
		 	}).map(item => {//filter 是吧不需要的进行排除　　　通过　ｍａｐ方法　是把这个对象中的，每一个数据重新进行编排
		 		    return {
		 		    	src: item.picInfo[0].url,
			        title:item.title,
			        desc: item.digest,
			        url: item.link
		 		    }
		 	})
		 	
		 	//滚动新闻
		 	
		 	this.list3=data.live.filter(item =>{
		 		return item.addata===null &&  item.picInfo[0];
		 	}).map(item => {//filter 是吧不需要的进行排除　　　通过　ｍａｐ方法　是把这个对象中的，每一个数据重新进行编排
		 		    return {
			        title:item.title,
		 		    }
		 	})
		 	initloadered=true;
		 	
		 });//第一个参数是地址　　　第二个参数是　返回的对象（数据）
	},
	data() {
		return {
			list:[{
				url: 'javascript:',
				img: 'https://static.vux.li/demo/1.jpg',
				title: '送你一朵fua'
			},{
				url: 'javascript:',
				img: 'https://static.vux.li/demo/2.jpg',
				title: '送你一辆车'
			},{
				url: 'javascript:',
				img: 'https://static.vux.li/demo/3.jpg',
				title: '送你一次旅行'
			}],
			swiperItemIndex: 0,
			type: '1',
      list1:[{
        src: 'http://placeholder.qiniudn.com/60x60/3cc51f/ffffff',
        title: '标题一',
        desc: '由各种物质组成的巨型球状天体，叫做星球。星球有一定的形状，有自己的运行轨道。',
        url: '/component/cell'
      },{
        src: 'http://placeholder.qiniudn.com/60x60/3cc51f/ffffff',
        title: '标题二',
        desc: '由各种物质组成的巨型球状天体，叫做星球。星球有一定的形状，有自己的运行轨道。',
        url: {
          path: '/component/radio',
          replace: false
       	 }
       },
       {
        src: 'http://placeholder.qiniudn.com/60x60/3cc51f/ffffff',
        title: '标题二',
        desc: '由各种物质组成的巨型球状天体，叫做星球。星球有一定的形状，有自己的运行轨道。',
        url: {
          path: '/component/radio',
          replace: false
       	 }
       },
       {
        src: 'http://placeholder.qiniudn.com/60x60/3cc51f/ffffff',
        title: '标题二',
        desc: '由各种物质组成的巨型球状天体，叫做星球。星球有一定的形状，有自己的运行轨道。',
        url: {
          path: '/component/radio',
          replace: false
       	 }
       },
       {
        src: 'http://placeholder.qiniudn.com/60x60/3cc51f/ffffff',
        title: '标题二',
        desc: '由各种物质组成的巨型球状天体，叫做星球。星球有一定的形状，有自己的运行轨道。',
        url: {
          path: '/component/radio',
          replace: false
       	 }
       },
       {
        src: 'http://placeholder.qiniudn.com/60x60/3cc51f/ffffff',
        title: '标题二',
        desc: '由各种物质组成的巨型球状天体，叫做星球。星球有一定的形状，有自己的运行轨道。',
        url: {
          path: '/component/radio',
          replace: false
       	 }
       },
       {
        src: 'http://placeholder.qiniudn.com/60x60/3cc51f/ffffff',
        title: '标题二',
        desc: '由各种物质组成的巨型球状天体，叫做星球。星球有一定的形状，有自己的运行轨道。',
        url: {
          path: '/component/radio',
          replace: false
       	 }
       },
       {
        src: 'http://placeholder.qiniudn.com/60x60/3cc51f/ffffff',
        title: '标题二',
        desc: '由各种物质组成的巨型球状天体，叫做星球。星球有一定的形状，有自己的运行轨道。',
        url: {
          path: '/component/radio',
          replace: false
       	 }
        }
       ],
       list3:[],
       list4:[]
     
   	 }
		},
		methods:{
			refresh(){
				/*alert(？)*/
				getkey();
				 this.$jsonp('http://3g.163.com/touch/jsonp/sy/recommend/0-9.html',{
				 	    miss:'00',
				 	    refresh:keyValue
				 }).then(data => {
				 	   
							this.list1=data.list.filter(item =>{
					 	return item.addata===null &&  item.picInfo[0];
					 	}).map(item => {//filter 是吧不需要的进行排除　　　通过　ｍａｐ方法　是把这个对象中的，每一个数据重新进行编排
					 		    return {
					 		    	src: item.picInfo[0].url,
						        title:item.title,
						        desc: item.digest,
						        url: item.link
					 		    }
					 	})
					 	this.$refs.myref.finishPullToRefresh();
					 	this.$vux.toast.text(this.list1.length, 'top');
				 })
				
				
			},
			infinite(){
				/*alert(2)*/
				
				if(!initloadered){
						this.$refs.myref.finishInfinite();
					 return;
				}
				
			
				this.$jsonp('http://3g.163.com/touch/jsonp/sy/recommend/'+start+'-'+end+'.html',{
				 	    miss:'00',
				 	    refresh:keyValue
				 }).then(data => {
				 	  
				 	  
				 	  this.list1=data.list.filter(item =>{
					 		return item.addata===null &&  item.picInfo[0];
					 	}).map(item => {//filter 是吧不需要的进行排除　　　通过　ｍａｐ方法　是把这个对象中的，每一个数据重新进行编排
					 		    return {
					 		    	src: item.picInfo[0].url,
						        title:item.title,
						        desc: item.digest,
						        url: item.link
					 		    }
					 	})
				 	  
				 	this.list4=this.list4.concat(this.list1);
				 	  start+=10;
				 	  end=start+9;
				 	  this.$refs.myref.finishInfinite();
				 })
				
				
			}
		}
	}

</script>

<style lang="less">
  @import '~vux/src/styles/reset.less';
  html,body{
  	margin:0;
  	width:100%;
  	height:100%;
  	overflow-x:hidden;
  }
  #app{
  	height:100%;
  	.tab{
  		width:1200px;
  		margin-top:40px;
  	}
  	.header{
  		position:absolute;
  		left:0;
  		top:0;
  		z-index:3;
  		width:100%;
  	}
  	/*.loading-layer{
  		padding-bottom:90px;
  	}*/
  }
</style>
