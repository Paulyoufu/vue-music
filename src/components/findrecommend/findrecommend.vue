<template>
	<div class="findrecommend">
		<swiper :options="swiperOption"  ref="mySwiper" v-if="reconmmend.findmusic">  
            <!-- 这部分放你要渲染的那些内容 -->  
            <swiper-slide v-for="item in reconmmend.findmusic.reconmmend.recommendImageList" :key="item.id" :songlist="JSON.stringify(item.songList)">
            	<img class="image" :src="item.src" alt="">
            	<span class="type" :style="{backgroundColor:item.color}">{{item.type}}</span>
            </swiper-slide>  
            <!-- 这是轮播的小圆点 -->  
            <div class="swiper-pagination" slot="pagination"></div>  
        </swiper>
        <div class="recommend-type">
        	<div class="div-type">
        		<i class="icon-type icon-fm"></i>
        		<p class="title">私人FM</p>
        	</div>
        	<div class="div-type">
        		<i class="icon-type icon-date"></i>
        		<p class="title">每日听歌推荐</p>
        	</div>
        	<div class="div-type">
        		<i class="icon-type icon-rank-list"></i>
        		<p class="title">云音乐热歌榜</p>
        	</div>
        </div>
	</div>
</template>
<script>
	import store from '../../store'
	import { swiper, swiperSlide } from 'vue-awesome-swiper'
	// import { mapState } from 'vuex'
	import { mapGetters } from 'vuex'
	export default {
		data () {
	      return {
			swiperOption: {
				// 是一个组件自有属性，如果notNextTick设置为true，组件则不会通过NextTick来实例化swiper，也就意味着你可以在第一时间获取到swiper对象，假如你需要刚加载遍使用获取swiper对象来做什么事，那么这个属性一定要是true
				autoplay: 1113000,
				grabCursor: true,
				setWrapperSize: true,
				autoHeight: true,
				pagination: '.swiper-pagination',
				paginationClickable: true,
				mousewheelControl: true,
				autoplayDisableOnInteraction: false,
				observeParents: true,
				loop: true,
				// lazyLoading: true,
				// lazyLoadingOnTransitionStart: true,
				onSlideChangeEnd: swiper => {
					// 这个位置放swiper的回调方法
					this.page = swiper.realIndex + 1
					this.index = swiper.realIndex
				},
				onClick: swiper => {
					const index = swiper.activeIndex
					// alert(swiper.slides[index].getAttribute('songlist'))
					store.commit({
						type: 'setMusicList',
						list: JSON.parse(swiper.slides[index].getAttribute('songlist'))
					})
					store.commit({
						type: 'playIndex',
						index: 0
					})
					store.dispatch({
						type: 'set_MusicDetail',
						isShow: true
					})
				}
			}
	      }
	    },
	    computed: {
			// reconmmend () {
			// 	return store.getters.getAllInfo.findmusic
			// },
			...mapGetters({
				reconmmend: 'getAllInfo'
			}),
			swiper () {
				return this.$refs.mySwiper.swiper
			}
	    },
	    methods: {
	    },
		mounted () {
			// you can use current swiper instance object to do something(swiper methods)
			// 然后你就可以使用当前上下文内的swiper对象去做你想做的事了
			// this.$refs.mySwiper.slideTo(0, 1000, false)
		},
	    components: {
            swiper,
            swiperSlide
        }
	}
</script>
<style lang="stylus" rel="stylesheet/stylus">
	@import "../../common/stylus/global.styl"
	.findrecommend
		position:fixed
		background:#fff
		overflow:auto
		top:86px
		bottom:46px
		left:0
		right:0
		z-index:11
		background:#fff
		-webkit-overflow-scrolling:touch
		a
			color:#666
			text-decoration:none
		.swiper-container
			.swiper-wrapper
				.swiper-slide
					.image
						width:100%
					.type
						position:absolute
						right:0
						bottom:20px
						padding:0 12px
						height:24px
						line-height:24px
						color:#fff
						font-size:14px
						font-weight:400
						border-radius:12px 0 0 12px
			.swiper-pagination
				.swiper-pagination-bullet
					margin:0 4px
					&.swiper-pagination-bullet-active
						background:$primarycolor!important
		.recommend-type
			width:100%
			height:90px
			display:flex
			align-items:center
			.div-type
				flex:1
				height:100%
				width:33.333333%
				font-size:0
				display:flex
				flex-direction: column
				justify-content: center
				align-items: center
				padding:0 10px
				.icon-type
					display:inline-block
					color:$primarycolor
					font-size:32px
					width:50px
					height:50px
					line-height:50px
					text-align:center
					border:1px solid $primarycolor
					border-radius:50%
				.title
					font-size:12px
					font-weight:400
					margin:0
					margin-top:8px
					text-overflow:ellipsis
					overflow:hidden
					white-space:nowrap
					width:100%
					text-align:center
				
</style>
