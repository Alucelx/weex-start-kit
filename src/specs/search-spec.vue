<template>
	<div>
		<div class="place-blank"></div>
		<navigator-bar @changeGrid="changeGrid" :totalNum="totalNum"></navigator-bar>
		<pro-list :proList="proList" @loadMore="loadMore" :grid="grid"></pro-list>
		<search-bar @triggerSearch="triggerSearch"></search-bar>
	</div>
</template>
<style scoped>
	.micon {
		font-family: iconfont;
		font-size: 40px;
	}

	.place-blank {
		margin-top: 106px;
	}

</style>
<script>
	import searchBar from "../components/SearchBar.vue"
	import navigatorBar from "../components/NavigatorBar.vue"
	import proList from "../components/ProList.vue"
	import ledJson from "../assets/simulation/led-json"
	import {mapGetters, mapActions} from "vuex"
	var stream = weex.requireModule("stream")

	export default{
		data(){
			return {
				totalNum: "",
				proList: "",
				currPage: 1,
				searchWord: "",
				showLoading: false,
				grid: 1
			}
		},
		components: {
			searchBar,
			navigatorBar,
			proList,
		},
		created (){
			this.search({
				word: 'led',
				page: 1
			}).then(ret => {
				this.proList = ret.dataList
				this.totalNum = ret.totalNum
			})
		},
		methods: {
			...mapActions(['setSearchData', 'triggerLoading']),
			changeGrid(){
				this.grid = +!(this.grid - 1) + 1
			},
			search(params){
				this.triggerLoading('on')
				this.$modal.toast({
					message: this.app.ctx
				})
				return new Promise((succ, error) => {
					stream.fetch({
						method: "POST",
						url: `${this.app.ctx}/search/product`,
						type: "json",
						"Content-Type": "application/json",
						headers: {
							"Content-Type": "application/json"
						},
						body: JSON.stringify(params)
					}, res => {
						this.triggerLoading('off')
//						this.$modal.toast({
//							message: res.ok+'123'
//						})
						!res.ok && _self.$modal.alert({
							message: `${res.statusText}::${res.data}`
						})
						res.ok && succ(res.data)
					})
				})
			},
			loadMore(){
				if (this.lmPending) return
				const _self = this;
				this.lmPending = true;
				this.currPage += 1;
				this.search({word: this.searchWord, page: this.currPage}).then(ret => {
					this.proList = _self.proList.concat(ret.dataList)
					this.lmPending = false
				})

			},
			triggerSearch(word){
				this.searchWord = word
				this.page = 1
				this.search({
					word,
					page: 1
				}).then((ret) => {
					this.proList = ret.dataList
					this.totalNum = ret.totalNum
				})
			}
		},
		computed: {
			...mapGetters(['searchData', 'app'])
		}
	}
</script>
