<template>
	<view :style="style_window">
	<view style="height: 5%; text-align: center; border-bottom: solid gray; padding-bottom: 10px; margin-bottom: 10px;">
		<h1>重庆欢乐谷今日游园情况大屏</h1>
	</view>
	<view style="height: 87%;">
	<view class="left">
		<view class="unit1">
			<view class="unit1-1">今日游玩人数</view>
			<view class="unit1-2">{{totUserCount}}</view>
		</view>
		<view class="unit2">
			<view class="unit2-1">今日游乐设施排行榜</view>
			<view class="unit2-2">
				<uni-table border emptyText="暂无">
					<uni-tr class="unit-2-2-1">
						<uni-th width="5" align="center">排名</uni-th>
						<uni-th width="50" align="center">设施名称</uni-th>
						<uni-th width="50" align="center">游玩人数</uni-th>
					</uni-tr>
					<uni-tr class="unit-2-2-1" v-for="(item, idx) in facilityRank">
						<uni-td align="center">{{idx+1}}</uni-td>
						<uni-td align="center">{{item.name}}</uni-td>
						<uni-td align="center">{{item.count}}</uni-td>
					</uni-tr>
				</uni-table>
			</view>
		</view>
	</view>
	<view class="middle">
		<view class="unit3">
			<view class="unit3-1">
				今日游乐设施实时拥挤度
			</view>
			<view class="unit3-2">
				<qiun-data-charts 
					type="column"
					:opts="opts_unit3"
					:chartData="chartData_unit3"
				/>
			</view>
		</view>
		<view class="unit4">
			<view class="unit4-1">
				推荐路线选择情况
				<qiun-data-charts 
					type="rose"
					:opts="opts_unit4_1"
					:chartData="chartData_unit4_1"
				/>
			</view>
			<view class="unit4-2">
				游乐设施打卡情况
				<qiun-data-charts 
					type="pie"
					:opts="opts_unit4_2"
					:chartData="chartData_unit4_2"
				/>
			</view>
		</view>
	</view>
	<view class="right">
		<view class="unit5">
			<view class="unit5-1">
				游乐设施拥挤度变化曲线
			</view>
			<view class="unit5-2">
				<qiun-data-charts 
					type="line"
					:opts="opts_unit5"
					:chartData="chartData_unit5"
				/>
			</view>
		</view>
		<view class="unit6">
			游乐场地地图及当前位置<br/>
			<view class="unit6-1"><image :src="map" style="height: 100%;width: 90%;"/></view>
		</view>
	</view>
	</view>
	</view>
</template>

<script>
export default {
  data() {
    return {
		chartData_unit3: {},
		opts_unit3: {
			color: ["#1890FF","#91CB74","#FAC858","#EE6666","#73C0DE","#3CA272","#FC8452","#9A60B4","#ea7ccc","#0624ea","#fff700"],
			padding: [15,15,0,5],
			enableScroll: false,
			legend: {
				show: true,
				position: "bottom",
				lineHeight: 25,
			},
			xAxis: {
				disableGrid: true,
				fontSize: 11
			},
			yAxis: {
				showTitle: true,
				data: [
					{
						title: "预计排队等待时间(分钟)",
						min: 0,
						titleOffsetY: -8,
						titleOffsetX: 50
					}
				]
			},
			extra: {
				column: {
					type: "group",
					width: 30,
					meterBorde: 1,
					meterFillColor: "#FFFFFF",
					activeBgColor: "#000000",
					activeBgOpacity: 0.08,
					linearType: "custom",
					// barBorderCircle: true,
					seriesGap: 2,
					categoryGap: 2,
					labelPosition: "outside"
				}
			}
		},
		chartData_unit4_1: {},
		opts_unit4_1: {
			color: ["#1890FF","#91CB74","#FAC858","#EE6666","#73C0DE","#3CA272","#FC8452","#9A60B4","#ea7ccc","#0624ea","#fff700"],
			padding: [5,5,5,5],
			enableScroll: false,
			legend: {
				show: true,
				position: "bottom",
				lineHeight: 25
			},
			extra: {
				rose: {
					type: "radius",
					minRadius: 50,
					activeOpacity: 0.5,
					activeRadius: 10,
					offsetAngle: 0,
					labelWidth: 15,
					border: true,
					borderWidth: 2,
					borderColor: "#FFFFFF",
					linearType: "custom"
				}
			},
		},
		chartData_unit4_2: {},
		opts_unit4_2: {
			color: ["#1890FF","#91CB74","#FAC858","#EE6666","#73C0DE","#3CA272","#FC8452","#9A60B4","#ea7ccc","#0624ea","#fff700"],
			padding: [5,5,5,5],
			enableScroll: false,
			extra: {
				pie: {
					activeOpacity: 0.5,
					activeRadius: 10,
					offsetAngle: 0,
					labelWidth: 15,
					border: true,
					borderWidth: 3,
					borderColor: "#FFFFFF",
					linearType: "custom"
				}
			}
		},
		chartData_unit5: {},
		opts_unit5: {
			color: ["#1890FF","#91CB74","#FAC858","#EE6666","#73C0DE","#3CA272","#FC8452","#9A60B4","#ea7ccc","#0624ea","#fff700"],
			padding: [15,10,0,15],
			dataLabel: false,
			// dataPointShape: false,
			enableScroll: false,
			update: true,
			legend: {},
			xAxis: {
				disableGrid: true,
				showTitle: true,
				title: "时间(小时)",
				titleOffsetY: -20,
				titleOffsetX: -50
				// labelCount: 10
			},
			yAxis: {
				// disableGrid: true,
				gridType: "dash",
				dashLength: 3,
				showTitle: true,
				data: [
					{
						title: "预计排队等待时间(分钟)",
						min: 0,
						max: null,
						titleOffsetY: -8,
						titleOffsetX: 50
					}
				]
			},
			extra: {
				line: {
					type: "curve",
					// type: "straight",
					width: 2,
					activeType: "hollow",
					linearType: "custom",
				}
			}
		},
		totUserCount: 0,
		style_window: "",
		facilityRank: [
			{name: 'none', count: 0},
			{name: 'none', count: 0},
			{name: 'none', count: 0},
			{name: 'none', count: 0},
			{name: 'none', count: 0},
			{name: 'none', count: 0},
			{name: 'none', count: 0},
			{name: 'none', count: 0},
			{name: 'none', count: 0},
		],
		map: "https://th.bing.com/th?id=OIP.qMXLFasxGYaT2lx9FjRJMgHaFm&w=287&h=217&c=8&rs=1&qlt=90&o=6&dpr=1.3&pid=3.1&rm=2",
		API : "http://120.46.211.213:8888",
		// API: "http://192.168.1.59:8888",
    };
  },
  onReady() {
	this.unit5_swiper();
	this.unit1_refresh();
	this.refresh();
  },
  methods: {
	unit1_refresh() {
		const api = this.API;
		
		uni.request({
			url: api + "/statistics/headCount/total",
			method: "GET",
			success: res=>{
				let res_body = res.data;
				if(res_body.code == 200){
					// unit1
					this.totUserCount = res_body.data;
				}else {
					console.log("获取数据失败");
				}
			}
		});
		setTimeout(this.unit1_refresh, 1000 * 1);
	},
	unit5_repack() {
		// return;
		if(this.chartData_unit5.series){
			const std = ["8","9","10","11","12","13","14","15","16","17","18","19","20"];
			let N = this.chartData_unit5.series.length;
			let L = null, R = null;
			for(let i=0; i<N; i++){
				if(this.chartData_unit5.series[i].show){
					let maxValue = 0;
					let M = this.chartData_unit5.series[i].data.length;
					if(M < std.length){
						let tmp = [];
						for(let j=M; j<std.length; j++) tmp.push(null);
						this.chartData_unit5.series[i].data = tmp.concat(this.chartData_unit5.series[i].data);
						M = std.length;
					}
					for(let j=0; j<M; j++){
						if(this.chartData_unit5.series[i].data[j] != null){
							if(null == L) {L = j}
							R = j;
							maxValue = Math.max(maxValue, this.chartData_unit5.series[i].data[j]);
						}
					}
					maxValue = Math.round(maxValue + 10);
					while(maxValue % 10 != 0) maxValue++;
					this.opts_unit5.yAxis.data[0].max = maxValue;
					this.chartData_unit5.series[i].data = this.chartData_unit5.series[i].data.slice(L, M);
					break;
				}
			}
			if(L == null){
				this.unit5_swiper(true);
				return;
			}
			this.chartData_unit5.categories = std.slice(L, R+1);
		}
	},
	unit5_swiper(fl) {
		if(this.chartData_unit5.series){
			let N = this.chartData_unit5.series.length;
			for(let i=0; i<N; i++){
				if(this.chartData_unit5.series[i].show){
					this.chartData_unit5.series[i].show = false;
					this.chartData_unit5.series[(i+1)%N].show = true;
					break;
				}
			}
			this.unit5_repack();
		}
		if(!fl) setTimeout(this.unit5_swiper, 1000 * 5);
	},
	refresh() {
		this.style_window = "";
		uni.getSystemInfo({
			success: (res)=>{
				this.style_window +="height:" + res.windowHeight + "px;";
				this.style_window +="width:" + res.windowWidth + "px;";
			},
		});
		this.getServerData();
		setTimeout(this.refresh, 1000 * 60);
	},
    getServerData() {
		// setTimeout(() => {
		// 	let res = {
		// 		categories: ["过山车","大摆锤","跳伞","蹦极","划船","旋转木马"],
		// 		series: [
		// 		  {
		// 			name: "设施名称",
		// 			data: [35,36,31,33,13,34]
		// 		  }
		// 		]
		// 	  };
		// 	this.chartData_unit3 = JSON.parse(JSON.stringify(res));
		// }, 0);
	  
		// setTimeout(() => {
		// 	let res = {
		// 		series: [
		// 		{
		// 		  data: [{"name":"路线1","value":50},{"name":"路线2","value":30},{"name":"路线3","value":20},{"name":"路线4","value":18},{"name":"路线5","value":8}]
		// 		}
		// 		]
		// 	};
		// 	this.chartData_unit4_1 = JSON.parse(JSON.stringify(res));
		// }, 0);
		
		// setTimeout(() => {
		// 	let res = {
		// 		series: [
		// 		  {
		// 			data: [{"name":"过山车","value":50},{"name":"大摆锤","value":30},{"name":"跳伞","value":20},{"name":"蹦极","value":18},{"name":"划船","value":8},{"name":"旋转木马","value":30}]
		// 		  }
		// 		]
		// 	  };
		// 	this.chartData_unit4_2 = JSON.parse(JSON.stringify(res));
		// }, 0);
		
		// setTimeout(() => {
		// 	let res = {
		// 		categories: ["8","9","10","11","12","13","14","15","16","17","18","19","20"],
		// 		series: [
		// 			{
		// 				name: "过山车",
		// 				lineType: "solid",
		// 				data: [35,8,25,37,4,20,28]
		// 			},
		// 			{
		// 				name: "大摆锤",
		// 				lineType: "solid",
		// 				data: [10,8,25,37,4,20,28]
		// 			},
		// 			{
		// 				name: "跳伞",
		// 				lineType: "solid",
		// 				data: [35,8,25,37,4,20,28]
		// 			},
		// 			{
		// 				name: "蹦极",
		// 				lineType: "solid",
		// 				data: [35,8,25,37,4,20,28]
		// 			}
		// 		]
		// 	};
		// 	this.chartData_unit5 = JSON.parse(JSON.stringify(res));
		// }, 0);
		
		const api = this.API;
		
		uni.request({
			url: api + "/statistics/headCount/facility",
			method: "GET",
			success: res=>{
				let res_body = res.data;
				if(res_body.code == 200){
					let facilities = res_body.data;
					// unit2
					this.facilityRank = [];
					facilities.forEach(item=>{
						this.facilityRank.push({
							name: item.facilityName,
							count: item.headCount
						});
					});
					this.facilityRank.sort((x,y)=>{
						return y.count - x.count;
					});
				}else {
					console.log("获取数据失败")
				}
			}
		});
		
		// 获取设施信息
		uni.request({
			url: api + "/filter/amusement",
			method: "POST",
			data: {},
			success: res=>{
				let res_body = res.data;
				if(res_body.code == 200){
					let facilities = res_body.data;
					
					// unit3
					let payload = {
						categories: [],
						series: [
							{
								name: "设施名称",
								data: []
							}
						]
					};
					facilities.forEach(item=>{
						payload.categories.push(item.name);
						payload.series[0].data.push(item.expectWaitTime);
					});
					this.chartData_unit3 = JSON.parse(JSON.stringify(payload));
				}else{
					console.log("获取数据失败");
				}
			}
		});
		
		uni.request({
			url: api + "/statistics/route/useCount",
			method: "GET",
			success: res=>{
				let res_body = res.data;
				if(res_body.code == 200){
					let route = res_body.data;
					// unit4_1
					let payload = {
						series: [
						{
						  data: []
						}
						]
					};
					route.forEach(item=>{
						payload.series[0].data.push({
							"name": item.name,
							"value": item.useCount
						});
					});
					this.chartData_unit4_1 = JSON.parse(JSON.stringify(payload));
				}else {
					console.log("获取数据失败");
				}
			}
		});
		
		uni.request({
			url: api + "/statistics/visit/count",
			method: "GET",
			success: res=>{
				let res_body = res.data;
				if(res_body.code == 200){
					let route = res_body.data;
					// unit4_2
					let payload = {
						series: [
						{
						  data: []
						}
						]
					};
					route.forEach(item=>{
						payload.series[0].data.push({
							"name": item.facilityName,
							"value": item.visitCount
						});
					});
					this.chartData_unit4_2 = JSON.parse(JSON.stringify(payload));
				}else {
					console.log("获取数据失败");
				}
			}
		});
		
		uni.request({
			url: api + "/statistics/crowingTimeCount",
			method: "GET",
			success: res=>{
				let res_body = res.data;
				if(res_body.code == 200){
					let facilities = res_body.data;
					console.log(facilities);
					// unit5
					let payload = {
						categories: ["8","9","10","11","12","13","14","15","16","17","18","19","20"],
						series: []
					};
					facilities.forEach((item,idx)=>{
						let arr = [];
						let N = item.crowingTimeItemVOList.length;
						for(let i=0, j=8; i<N; i++){
							while(j < item.crowingTimeItemVOList[i].time){
								arr.push(null);
								j++;
							}
							arr.push(item.crowingTimeItemVOList[i].expectWaitTime);
							j++;
						}
						payload.series.push({
							name: item.facilityName,
							lineType: "solid",
							show: false,
							data: arr
						});
					});
					payload.series[0].show = true;
					this.chartData_unit5 = JSON.parse(JSON.stringify(payload));
					this.unit5_repack();
				}else {
					console.log("获取数据失败");
				}
			}
		})
    },
  }
};
</script>

<style scoped>
	body{
		/* background: norepeat url("https://th.bing.com/th?id=OIP.27iFzU7VzMz2ChSTr2mVwgHaCg&w=349&h=118&c=8&rs=1&qlt=90&o=6&dpr=1.3&pid=3.1&rm=2"); */
		/* background-color: lightblue; */
		color: black;
	}
	view {
		/* border: solid red 1px; */
	}
	.left, .middle, .right {
		/* border: solid blue 1px; */
		vertical-align: top;
		display: inline-block;
		height: 100%;
	}
	.left {
		width: 25%;
	}
	.middle {
		width: 40%;
	}
	.right {
		width: 34%;
	}
	
	.unit1 {
		height: 22%;
	}
	.unit1-1 {
		text-align: center;
		vertical-align: middle;
		padding-top: 3%;
		height: 20%;
		font: 1.8em sans-serif;
	}
	.unit1-2 {
		color: red;
		text-align: center;
		vertical-align: middle;
		padding-top: 4%;
		height: 40%;
		font: 4em sans-serif;
	}
	
	.unit2 {
		height: 75%;
	}
	.unit2-1 {
		text-align: center;
		vertical-align: middle;
		padding-top: 6%;
		height: 10%;
		font: 1.8em sans-serif;
	}
	.unit2-2 {
		text-align: center;
		vertical-align: middle;
		height: 80%;
		/* font: 3em sans-serif; */
		padding: 5%;
	}
	.unit-2-2-1 {
		/* background-color: lightblue; */
	}
	
	.unit3 {
		text-align: center;
		font: 1.8em sans-serif;
		height: 55%;
		padding-bottom: 5%;
	}
	.unit3-1 {
		height: 10%;
	}
	.unit3-2 {
		height: 90%;
		padding-top: 5%;
	}
	
	.unit4 {
		height: 40%;
	}
	.unit4-1, .unit4-2 {
		text-align: center;
		font: 1.8em sans-serif;
		vertical-align: top;
		display: inline-block;
		width: 49.7%;
		height: 92%;
	}
	
	.unit5 {
		text-align: center;
		font: 1.8em sans-serif;
		vertical-align: top;
		display: inline-block;
		width: 100%;
		height: 50%;
		padding-bottom: 6%;
	}
	.unit5-1 {
		height: 10%;
	}
	.unit5-2 {
		height: 90%;
		padding-top: 5%;
		padding-inline: 5%;
	}
	
	.unit6 {
		text-align: center;
		font: 1.8em sans-serif;
		vertical-align: top;
		display: inline-block;
		width: 100%;
		height: 40%;
		padding-bottom: 6%;
	}
	.unit6-1 {
		margin-top: 2%;
		width: 100%;
		height: 95%;
		padding-inline: 5%;
	}
</style>