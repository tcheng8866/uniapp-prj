<template>
	<view class="page">
		<view class="wrap">
			<!-- 请求封装 -->
			<view v-for="(item, index) in list" :key="index">
				<view>{{item.content}}</view>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	name: '',
	mixins: [],
	components: {},
	props: {},
	data() {
		return {
			list: []
		};
	},
	computed: {},
	watch: {},
	async created() {
		console.log(1)
		await this.queryData()
		console.log(2)
	},
	mounted() {},
	destroyed() {},
	methods: {
		async queryData() {
			// async await 包裹promise
			// 这里写法就多了：接收变量(判断条件) / .then的写法都行
			await this.requestPromise()
				.then(res => {
					console.log('success', res);
					this.list=res.data.result.data
				})
				.catch(e => {
					console.log('error', e);
				});
		},
		requestPromise() {
			// 把回调封装成promise形式
			return new Promise((resolve, reject) => {
				let time = (new Date().getTime() + '').substr(0, 10);
				uni.request({
					url: 'http://v.juhe.cn/joke/content/list.php',
					data: {
						key: 'bd50f1ee7e58f2e76eb39cf20618fbda',
						page: 1,
						pagesize: 1,
						sort: 'desc1',
						time: time
					},
          method: "POST",
          header: { "content-type": "application/x-www-form-urlencoded" },					
					success: res => {
						resolve(res);
					},
					fail: e => {
						reject(e);
					},
					complete: () => {
						console.log('complete do');
					}
				});
			});
		}
	}
};
</script>
<style lang="less" scoped>
</style>
