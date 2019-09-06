

//存放store对象管理所有组件的状态
const state = {
	mapView: null, //二维地图视图
	webmap: null, //二维地图webmap
	currentWidget: null
};

//获取组件状态（数据）
const getters = {};

//用来存放页面逻辑的JS容器
const mutations = {

	//清除地图绘制
	clearMapGra(state) {
		if(state.currentWidget){
			state.currentWidget.destroy();
			state.currentWidget = null;
		}		
	},
	
	//定位用户当前位置
	getLocation(state){
		uni.getLocation({
			success: (res) => {
				let longitude=res.longitude;
				let latitud=res.latitud;
				let point = {
				  type: "point", // autocasts as /Point
				  x: longitude,
				  y: latitud,
				  spatialReference: _self.view.spatialReference
				};
				console.log("longitude----------"+longitude);
				console.log("latitud----------"+latitud);
			},
			fail: (err) => {
				console.log("----------获取定位失败----------");
				uni.showToast({
					title: err.errMsg
				})
			}
		})
	},

	//测量面积
	measureArea(state) {
		let _self = this;
		esriLoader
			.loadModules(["esri/views/MapView", "esri/WebMap", "esri/widgets/DistanceMeasurement2D",
				"esri/widgets/AreaMeasurement2D"
			], {
				url: mapConfig.jsapiUrl
			})
			.then(function([MapView, WebMap, DistanceMeasurement2D, AreaMeasurement2D]) {
				if (state.currentWidget) {
					if (state.currentWidget.id == "area") {
						state.currentWidget.destroy();
						state.currentWidget = null;
						return;
					} else {
						state.currentWidget.destroy();
						state.currentWidget = null;
					}
				}

				state.currentWidget = new AreaMeasurement2D({
					id: "area",
					view: state.mapView
				});
				state.currentWidget.viewModel.newMeasurement();
			});
	},

	//测量距离
	measureLength(state) {
		let _self = this;
		esriLoader
			.loadModules(["esri/views/MapView", "esri/WebMap", "esri/widgets/DistanceMeasurement2D",
				"esri/widgets/AreaMeasurement2D"
			], {
				url: mapConfig.jsapiUrl
			})
			.then(function([MapView, WebMap, DistanceMeasurement2D, AreaMeasurement2D]) {
				if (state.currentWidget) {
					if (state.currentWidget.id == "distance") {
						state.currentWidget.destroy();
						state.currentWidget = null;
						return;
					} else {
						state.currentWidget.destroy();
						state.currentWidget = null;
					}
				}
				state.currentWidget = new DistanceMeasurement2D({
					id: "distance",
					view: state.mapView
				});
				state.currentWidget.viewModel.newMeasurement();
			});
	},

	//加载瓦片图
	loadTileMap(state) {
		esriLoader
			.loadModules(
				["esri/Map", "esri/views/MapView", "esri/layers/TileLayer"], {
					url: mapConfig.jsapiUrl
				}
			)
			.then(([Map, MapView, TileLayer]) => {
				const layer = new TileLayer({
					url: mapConfig.TileLayer
				});
				const map = new Map({
					// basemap: "topo"
				});
				map.add(layer);
				state.mapView = new MapView({
					container: "mapDiv",
					map: map,
					center: [108.9, 34.25],
					zoom: 15
				});
				console.log("-------------------加载地图成功--------------------------");
			});
	},
	loadTdtWMTS(state) {
		Vue.prototype.esriLoader
			.loadModules(
				[
					"esri/Map",
					"esri/views/MapView",
					"esri/layers/WebTileLayer",
					"esri/layers/support/TileInfo"
				], {
					url: window.mapConfig.jsapiUrl
				}
			)
			.then(([Map, MapView, WebTileLayer, TileInfo]) => {
				let tileInfo = new TileInfo({
					dpi: 90.71428571427429,
					rows: 256,
					cols: 256,
					compressionQuality: 0,
					origin: {
						x: -180,
						y: 90
					},
					spatialReference: {
						wkid: 4326
					},
					
};

// 包含任何异步的操作，以及状态改变的状态
const actions = {};
export default {
	state,
	getters,
	mutations,
	actions
};
