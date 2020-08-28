<template>
	<view class="main flex">
		<Title :curPageTitle="curPageTitle" />
		<view class="page" @touchstart="touchStart" @touchend="touchEnd">
			<page1 v-if="curPage==0" :curPageIcon="curPage" />
			<page2 v-if="curPage==1" :curPageIcon="curPage" />
			<page3 v-if="curPage==2" :curPageIcon="curPage" />
			<page4 v-if="curPage==3" :curPageIcon="curPage" />
		</view>
		<tar-bar @tabChange="tabChange" :curPage="curPage" :tarBar="tarBar" />
	</view>
</template>

<script>
	import Title from "@/components/Title.vue";
	import page1 from "./page1/page1.vue";
	import page2 from "./page2/page2.vue";
	import page3 from "./page3/page3.vue";
	import page4 from "./page4/page4.vue";
	import tarBar from "@/components/tarbar.vue";
	import {
		getTouchData
	} from '../../utils/util.js';
	export default {
		components: {
			Title,
			page1,
			page2,
			page3,
			page4,
			tarBar
		},
		data() {
			return {
				curPage: '页面一', //默认显示 number string
				curPageTitle: null, //不可设置默认标题
				touch: {
					x: null,
					y: null
				},
				tarBar: {
					color: '#999999', //tarbar字体颜色
					backgroundColor: '#13227a',
					options: [{
							iconPath: '/static/images/tab1.png',
							iconPathSelected: '/static/images/tab1_selected.png',
							title: '页面一'
						},
						{
							iconPath: '/static/images/tab2.png',
							iconPathSelected: '/static/images/tab2_selected.png',
							title: '页面二'
						},
						{
							iconPath: '/static/images/tab3.png',
							iconPathSelected: '/static/images/tab3_selected.png',
							title: '页面三'
						},
						{
							iconPath: '/static/images/tab4.png',
							iconPathSelected: '/static/images/tab4_selected.png',
							title: '页面四'
						},
					],
				}
			}
		},
		created() {
			uni.$on('titleBoxHeight', data => console.log(data))
		},
		methods: {
			tabChange(curPage) {
				this.$set(this, 'curPage', curPage.page); // 当前加载的页面
				this.curPageTitle = curPage.title;
			},
			touchStart(e) {
				this.touch.x = e.changedTouches[0].clientX;
				this.touch.y = e.changedTouches[0].clientY;
			},
			touchEnd(e) {
				let x = e.changedTouches[0].clientX;
				let y = e.changedTouches[0].clientY;
				const touch = getTouchData(x, y, this.touch.x, this.touch.y);
				if (touch == 'left') {
					this.curPage += 1;
					if (this.curPage > this.tarBar.options.length - 1) this.curPage = this.tarBar.options.length - 1;
				} else if (touch == 'right') {
					this.curPage -= 1;
					if (this.curPage < 0) this.curPage = 0;
				}
			}
		}
	}
</script>

<style lang="scss">
	/* #ifdef MP-WEIXIN || APP-PLUS*/
	page {
		margin: 0;
		padding: 0;
		width: 100vw;
		height: 100vh;
		// overflow: hidden;
	}

	/*#endif*/
	.main {
		/* #ifdef H5 */
		flex-direction: column-reverse;
		/*#endif*/
		/* #ifdef MP-WEIXIN || APP-PLUS*/
		height: 100vh;
		flex-direction: column;

		/*#endif*/
		.page {
			flex: 1;
			height: 100%;
			background-color: #f0f0f0;
			/* #ifdef MP-WEIXIN || APP-PLUS*/
			overflow-y: scroll;
			/*#endif*/
		}
	}
</style>
