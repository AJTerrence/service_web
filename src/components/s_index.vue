<template>
  <div id="app">
    <div class="concern">
    	<swiper :options="swiperOption" ref="mySwiper">
		    <!-- slides -->
		    <swiper-slide v-for="(item,index) in data" :key="index">
				<img  :src="item.qrcodeUrl" height="200" width="200">
		    </swiper-slide>
		    <!-- Optional controls -->
		    <div class="swiper-pagination"  slot="pagination"></div>
		    <div class="swiper-button-prev" slot="button-prev"></div>
		    <div class="swiper-button-next" slot="button-next"></div>
		</swiper>
		<!-- <button btn = "1" class="changan">点击关注公众号</button> -->
		<button class="weui-btn weui-btn_primary" v-touch:press>长按关注公众号</button>
    </div>
    <div class="pay" @click="_pay()">
    	<div>
	    	<img src="../assets/logo.png" height="50" width="50">
	    	<p>微信支付购币</p>
    	</div>	
    </div>
  </div>
</template>

<script>

	// var wifi = true;
 //    var ua = window.navigator.userAgent;
 //    var con = window.navigator.connection;
 //    // 如果是微信
 //    if(/MicroMessenger/.test(ua)){
 //        // 如果是微信6.0以上版本，用UA来判断
 //        if(/NetType/.test(ua)){
 //            if(ua.match(/NetType\/(\S*)$/)[1] != 'WIFI'){
 //                wifi = false;
 //            }
 //        // 如果是微信6.0以下版本，调用微信私有接口WeixinJSBridge
 //        }else{
 //            document.addEventListener("WeixinJSBridgeReady",function onBridgeReady(){
 //                WeixinJSBridge.invoke('getNetworkType',{},function(e){
 //                    if(e.err_msg != "network_type:wifi"){
 //                        wifi = false;
 //                    }
 //                });
 //            });
 //        }
 //    // 如果支持navigator.connection
 //    }else if(con){
 //        var network = con.type;
 //        if(network != "wifi" && network != "2" && network != "unknown"){  // unknown是为了兼容Chrome Canary
 //            wifi = false;
 //        }
 //    }
 //    alert(wifi)
	// window.networkWIFI = wifi
	var con = window.navigator
	if(con.connection){
		alert(con.connection.type)
	}
	if(con.userAgent.match(/Android/i)){
		alert('android')
	}
	if(con.userAgent.match(/iPhone/i)){
		alert('ios')
	}
	import Vue from 'vue'

	import VueAwesomeSwiper from 'vue-awesome-swiper'
	Vue.use(VueAwesomeSwiper)
	// if (process.BROWSER_BUILD) {
	//   const VueAwesomeSwiper = require('vue-awesome-swiper/ssr')
	//   Vue.use(VueAwesomeSwiper)
	// }
	import { swiper, swiperSlide } from 'vue-awesome-swiper'

	import Vuwe from 'vuwe'
	Vue.use(Vuwe)

	import VueResource from 'vue-resource'
	Vue.use(VueResource)

	import directives from '../js/touch.js'
	directives(Vue)

	export default {
		components: {
		    swiper,
	    	swiperSlide
		},
		data () {
		  	return{
		  		swiperOption: {
		          	pagination: '.swiper-pagination',
		          	nextButton: '.swiper-button-next',
		          	prevButton: '.swiper-button-prev',
		          	slidesPerView: 1,
		          	paginationClickable: true,
		          	spaceBetween: 30,
		          	loop: true
		        },
		        data: []
		  	}
		},
		methods:{
		  	_pay () {
		  		location.href = './pay.html'
		  	}
		},
		beforeCreate(){
			
		},
		mounted (){
			var innerHeight = window.innerHeight
			$('.concern').css('height', (innerHeight- 60)/2 + 'px')
			$('.pay').css('height', (innerHeight- 60)/2 + 'px')
			this.$http.get('/api/qrcode')
			.then(function(res){
				console.log(res.data)
				this.data = res.data
			},function(err){
				console.log(err)
			})
		}
	}
</script>

<style lang="less">
  @import '../less/index.less';
</style>
