<template>
	<div class="container">
		<div class="top">
			<p>{{companyInfo.name}}</p>
			<!-- <p >中国石油化工股份有限公司山东石油分公司</p> -->

			<div class="weui-flex top_maddle">
				<div class="weui-flex__item">
					<div class="placeholder fontWight">{{companyInfo.balance}}</div>
					<div class="placeholder1">账户余额</div>
				</div>
				<div class="weui-flex__item">
					<div class="placeholder fontWight">{{companyInfo.orderedMoney}}</div>
					<div class="placeholder1">已下单</div>

				</div>
				<div class="weui-flex__item">
					<div class="placeholder fontWight">{{companyInfo.availableBalance}}</div>
					<div class="placeholder1">可用金额</div>

				</div>
			</div>
		</div>
		<!-- 订购货物信息 -->
		<div class="product">
			<p>
				<span class="line"></span>
				<span style="margin: 0px 30px;">订购货物信息</span>
				<span class="line"></span>
			</p>
			<div class="weui-cell" style="border-top: none">
				<div class="weui-cell__bd">
					<p>预提货品类</p>
				</div>
				<div class="weui-cell__ft">
					<picker @change="bindPickerChange" :value="index" :range="categories">
						<view class="picker">
							{{pickSelect}}
						</view>
					</picker>
				</div>
			</div>
			<div class="weui-cell">
				<div class="weui-cell__bd">
					<p>物料名称</p>
				</div>
				<div class="weui-cell__ft">
					<div class="ishave" v-if="clickPick" @click.stop="ishave" style="color:#000">{{pickSelect1}}</div>
					<div class="ishave" v-else>
						<picker @change="bindPickerChange1" :value="index" :range="oils">
							<view class="picker">
								{{pickSelect1}}
							</view>
						</picker>
					</div>
				</div>
			</div>
			<div class="weui-cell" style="border-bottom: 1px solid #ddd;position: relative;">
				<div class="weui-cell__bd">
					<p>物料重量</p>
				</div>
				<div class="weui-cell__ft">
					<span class="dw" style="color: #000">(吨)</span>
					<input class="input" style="color: #000" type="digit" @change="input" maxlength="5" v-model="num" placeholder="请输入">
				</div>
			</div>
		</div>
		<div class="choose">
			<p class="t">
				<span class="fontSize">选择司机</span>
			</p>
			<button class="weui-btn weui-btn_default" v-if="showDriver" @click="chooseDriver" style="background-color: #fff;border-color: #dedede">请选择</button>
			<div class="driver" v-else>
				<p class="driver_name driverInfo">
					<span>司机真实姓名：</span>
					<span>{{driverInfo.realName == "null" ? "暂无数据" : driverInfo.realName }}</span>
				</p>
				<p class="driver_phone driverInfo">
					<span>绑定手机号：</span>
					<span>{{driverInfo.phone == "null" ? "暂无数据" : driverInfo.phone}}</span>
				</p>
				<p class="driver_name driverInfo">
					<span>身份证号：</span>
					<span>{{driverInfo.idNumber == 'null' ? "暂无数据" : driverInfo.idNumber}}</span>
				</p>
				<p class="driver_name driverInfo">
					<span>驾证号码：</span>
					<span>{{driverInfo.driverNumber == "null" ? "暂无数据" : driverInfo.driverNumber}}</span>
				</p>
				<div class="btnGroup">
					<span class="reset" @click="reset">重新选择</span>
				</div>
			</div>
		</div>
		<div class="choose">
			<p class="t">
				<span class="fontSize">选择车辆</span>
			</p>
			<button class="weui-btn weui-btn_default" v-if="showCar" @click="chooseCar" style="background-color: #fff;border-color: #dedede">请选择</button>
			<div class="driver" v-else>
				<p class="driver_phone driverInfo">
					<span>车牌号：</span>
					<span>{{carInfo.carNumber == "null" ? "暂无数据" : carInfo.carNumber}}</span>
				</p>
				<p class="driver_name driverInfo">
					<span>行驶证号：</span>
					<span>{{carInfo.drivingNumber == "null" ? "暂无数据" : carInfo.drivingNumber}}</span>
				</p>
				<p class="driver_name driverInfo">
					<span>危险品运输证号：</span>
					<span>{{carInfo.transportNumber == "null" ? "暂无数据" : carInfo.transportNumber}}</span>
				</p>
				<div class="btnGroup">
					<span class="reset" @click="reset1">重新选择</span>
				</div>
			</div>

		</div>
		<div class="choose" style="margin-bottom: 60px">
			<p class="t">
				<span class="fontSize">选择已有押车员下单(可不选)</span>
			</p>
			<button class="weui-btn weui-btn_default" v-if="showPeople" @click="choosePeoPle" style="background-color: #fff;border-color: #dedede">请选择</button>
			<div class="driver" v-else>
				<p class="driver_name driverInfo">
					<span>用户名：</span>
					<span>{{escortInfo.username == "null" ? "暂无数据" : escortInfo.username}}</span>
				</p>
				<p class="driver_name driverInfo">
					<span>真实姓名：</span>
					<span>{{escortInfo.realName == "null" ? "暂无数据" : escortInfo.realName}}</span>
				</p>
				<p class="driver_phone driverInfo">
					<span>绑定手机号：</span>
					<span>{{escortInfo.phone == "null" ? "暂无数据" : escortInfo.phone}}</span>
				</p>
				<p class="driver_name driverInfo">
					<span>身份证号：</span>
					<span>{{escortInfo.idNumber == "null" ? "暂无数据" : escortInfo.idNumber}}</span>
				</p>
				<div class="btnGroup">
					<span class="reset" @click="reset2">重新选择</span>
				</div>
			</div>
		</div>
		<div class="footer">
			<span style="font-size: 20px">{{sumPrice}}</span>
			<button class="weui-btn weui-btn_primary" @click="creatOrder">提交订单</button>
		</div>
		<div class="js_dialog" id="iosDialog1" v-if="showDialog">
			<div class="weui-mask"></div>
			<div class="weui-dialog">
				<div class="weui-dialog__hd"><strong class="weui-dialog__title">警告</strong></div>
				<div class="weui-dialog__bd">
					<p>您当前可用余额不足，是否仍然下单？</p>
					<p>可用余额：{{companyInfo.balance}}元</p>
					<p>订单金额：{{sumPrice}}元</p>
				</div>
				<div class="weui-dialog__ft">
					<button class="weui-dialog__btn weui-dialog__btn_default" @click="concel">取消</button>
					<button class="weui-dialog__btn weui-dialog__btn_primary" @click="sure">继续下单</button>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				companyInfo: "",
				pickSelect: "请选择",
				pickSelect1: "请选择",
				index: 0,
				showDriver: true,
				showCar: true,
				showPeople: true,
				showDialog: false,
				clickPick: true,
				categories: [],
				num: "",
				num1: "",
				oils: [],
				// 库存
				stock: "",
				// 油品单价
				oilPrice: "",
				// 总价
				sumPrice: "¥0",
				driverInfo: "",
				carInfo: "",
				escortInfo: ""

			}
		},
		methods: {
			bindPickerChange: function (e) {
				console.log('picker发送选择改变，携带值为', e)
				this.pickSelect = this.categories[e.mp.detail.value];
				var categoryName = this.categories[e.mp.detail.value];
				this.$http.get("/self_order/oils", { "categoryName": categoryName })
					.then(res => {
						console.log(res)
						if (res.status == "200") {
							this.oils = [];
							this.pickSelect1 = "请选择"

							if (res) {
								for (var i = 0; i < res.data.length; i++) {
									this.oils.push(res.data[i].name)
								}
							}
						}

					})
					.catch(res => {
						wx.showToast({
							title: res.response.data.message,
							icon: 'none',
							duration: 2000
						})
					})
			},
			ishave: function () {
				if (this.pickSelect == "请选择") {
					wx.showToast({
						title: "请先选择预提货品类",
						icon: 'none',
						duration: 2000
					})
				} else {
					this.clickPick = false;
				}
			},
			bindPickerChange1: function (e) {
				if (this.pickSelect == "请选择") {
					wx.showToast({
						title: "请选择预提货品类",
						icon: 'none',
						duration: 2000
					})
				} else if (this.num && this.num > 50) {
					wx.showToast({
						title: "下单数量不能大于49吨",
						icon: 'none',
						duration: 2000
					})
					this.num = ""
				} else {
					console.log('picker发送选择改变，携带值为', e)
					this.pickSelect1 = this.oils[e.mp.detail.value];
					var oilName = this.oils[e.mp.detail.value];
					this.$http.get("/self_order/oil_product", { "categoryName": this.pickSelect, "oilName": oilName })
						.then(res => {
							console.log(res)
							if (res.status == "200") {
								this.oilPrice = res.data.price;
								this.stock = res.data.stock;
								this.sumPrice = "¥" + (res.data.price * this.num).toFixed(2)
							}
						})
						.catch(res => {
							wx.showToast({
								title: res.response.data.message,
								icon: 'none',
								duration: 2000
							})
						})

				}

			},
			input: function (val) {
				console.log(typeof(val))
				var num = val.mp.detail.value;
				console.log(isNaN(num))

				if(isNaN(num)){
					wx.showToast({
						title: "请输入正确的数值",
						icon: 'none',
						duration: 2000
					})
					this.num = ""
				}else if (this.pickSelect == "请选择") {
					wx.showToast({
						title: "请先选择预提货品类",
						icon: 'none',
						duration: 2000
					})
				} else if (this.pickSelect1 == "请选择") {
					wx.showToast({
						title: "请先选择物料品类",
						icon: 'none',
						duration: 2000
					})
				} else if (num >= 50) {
					wx.showToast({
						title: "下单数量不能大于49吨",
						icon: 'none',
						duration: 2000
					})
					this.num = ""
				}else  {
					this.sumPrice = "¥" + (this.oilPrice * num).toFixed(2)
				}

			},
			reset: function () {
				this.showDriver = true;
				wx.removeStorage({
					key: 'selectDriver',
					success: function (res) {
						console.log(res.data)

					}
				})
				wx.navigateTo({
					url: "../../pages/selectDriver/main",
					fail: function (res) {
						console.log(res)
					}
				})

			},
			reset1: function () {
				this.showCar = true;
				wx.removeStorage({
					key: 'selectCar',
					success: function (res) {
						console.log(res.data)

					}
				})
				wx.navigateTo({
					url: "../../pages/selectCar/main?from=selfHelp",
					fail: function (res) {
						console.log(res)
					}
				})

			},
			reset2: function () {
				this.showPeople = true;
				wx.removeStorage({
					key: 'selectescorts',
					success: function (res) {
						console.log(res.data)

					}
				})
				wx.navigateTo({
					url: "../../pages/selectEscort/main",
					fail: function (res) {
						console.log(res)
					}
				})

			},
			chooseDriver: function () {
				// 跳转到司机页面
				// this.showDriver = false;
				wx.navigateTo({
					url: "../../pages/selectDriver/main",
					fail: function (res) {
						console.log(res)
					}
				})
			},
			chooseCar: function () {
				// 跳转到车辆页面
				// this.showCar = false;
				wx.navigateTo({
					url: "../../pages/selectCar/main?from=selfHelp",
					fail: function (res) {
						console.log(res)
					}
				})

			},
			choosePeoPle: function () {
				// 跳转到押运员页面
				// this.showPeople = false;
				wx.navigateTo({
					url: "../../pages/selectEscort/main",
					fail: function (res) {
						console.log(res)
					}
				})

			},
			creatOrder: function () {

				// 判断当前账户余额是否能够支付，不能支付弹出警告，点击继续支付可以跳转到订单详情页，如果余额够支付 不提示直接跳转到订单详情
				if (this.sumPrice > this.companyInfo.balance) {
					// 余额不足弹窗
					this.showDialog = true;
				} else {
					// 下单
					var params = {
						categoryName: this.pickSelect,
						oilName: this.pickSelect1,
						orderWeight: this.num,
						driverId: this.driverInfo.key,
						escortId: this.escortInfo.length > 0 ? this.escortInfo.key : "",
						carId: this.carInfo.key
					}
					this.$http.post("/self_order", params)
						.then(res => {
							console.log(res)
							if (res.status == "200") {
								var orderInfo = res.data.id
								console.log(orderInfo)

								wx.navigateTo({
									url: "../../pages/order/orderInfo/main?orderInfo=" + orderInfo + "&from=selfHelp",
									fail: function (res) {
										console.log(res)
									}
								})
							} else {
								wx.showToast({
									title: res.statusText,
									icon: 'none',
									duration: 2000
								})
							}
						})
						.catch(res => {
							console.log(res)

							// .response.data.message
							wx.showToast({
								title: res.response.data.message,
								icon: 'none',
								duration: 2000
							})
						})
				}

			},
			concel: function () {
				this.showDialog = false;
			},
			sure: function () {
				var that = this
				var params = {
					categoryName: this.pickSelect,
					oilName: this.pickSelect1,
					orderWeight: this.num,
					driverId: this.driverInfo.key,
					escortId: this.escortInfo.length > 0 ? this.escortInfo.key : "",
					carId: this.carInfo.key
				}
				this.$http.post("/self_order", params)
					.then(res => {
						console.log(res)
						this.showDialog = false;
						if (res.status == "200") {
							var orderInfo = JSON.stringify(res.data)
							console.log(orderInfo)
							wx.redirectTo({
								url: "../../pages/order/orderInfo/main?orderInfo=" + JSON.stringify(res.data),
								fail: function (res) {
									console.log(res)
								}
							})
						} else {
							wx.showToast({
								title: res.statusText,
								icon: 'none',
								duration: 2000
							})
						}
					})
					.catch(res => {
						console.log(res)
						// .response.data.message
						wx.showToast({
							title: res.response.data.message,
							icon: 'none',
							duration: 2000
						})
					})

			}

		},

		onShow() {
			// 获取企业信息
			console.log("我展示了a")
			var selectCar = wx.getStorageSync('selectCar')
			if (selectCar) {
				this.carInfo = selectCar
				this.showCar = false;
			}
			var selectDriver = wx.getStorageSync('selectDriver')
			if (selectDriver) {
				this.driverInfo = selectDriver
				this.showDriver = false;
			}
			var selectescorts = wx.getStorageSync('selectescorts')
			if (selectescorts) {
				this.escortInfo = selectescorts;
				this.showPeople = false;
			}

			// 获取物料列表
		},
		mounted(query) {
			// 接收页面参数
			wx.removeStorage({
				key: 'selectCar',
				success: function (res) {
					console.log(res.data)
				}
			})
			wx.removeStorage({
				key: 'selectDriver',
				success: function (res) {
					console.log(res.data)
				}
			})
			wx.removeStorage({
				key: 'selectescorts',
				success: function (res) {
					console.log(res.data)
				}
			})
			Object.assign(this.$data, this.$options.data())
			console.log(this)
			this.$http.get("/self_order/company_account")
				.then(res => {
					console.log(res)
					if (res.status == "200") {
						this.companyInfo = res.data;
					} else {
						wx.showToast({
							title: res.statusText,
							icon: 'none',
							duration: 2000
						})
					}
				})
			// 获取预提货品类
			this.$http.get("/self_order/categories")
				.then(res => {
					this.categories = []
					console.log(res)

					if (res.status == "200") {
						for (var i = 0; i < res.data.length; i++) {
							this.categories.push(res.data[i].name)
						}
					} else {
						wx.showToast({
							title: res.statusText,
							icon: 'none',
							duration: 2000
						})
					}
				})
				.catch(res => {
					wx.showToast({
						title: res.response.data.message,
						icon: 'none',
						duration: 2000
					})
				})
		}
	}
</script>

<style scoped>
	.container {
		width: 100%;
		padding: 0px;
		background-color: #efeff4;
		overflow-x: hidden;
	}

	.btnGroup {
		width: 100%;
		height: 50px;
		text-align: right;
		border-top: 1px solid #ddd;

	}

	.reset {
		float: right;
		width: 60px;
		height: 30px;
		line-height: 30px;
		font-size: 12px;
		text-align: center;
		margin-right: 20px;
		background-color: #2E79FF;
		color: #fff;
		margin-top: 10px;
		border-radius: 5px;
	}

	.line {
		display: inline-block;
		height: 1px;
		width: 50px;
		background-color: #898989;
		vertical-align: middle
	}

	.weui-flex__item {
		width: 33%;
		display: inline-block;
	}

	.input {
		float: right;
		width: 50%;
		padding: 0px;
		text-align: right;
		height: 100%;
	}

	input-placeholder {
		text-align: right;
	}

	.dw {
		float: right;

	}

	.weui-cell__bd {
		margin-left: 10px;
	}

	.weui-cell__ft {
		color: #b5b5b5 !important
	}

	.weui-cell {
		background-color: #fff;
		position: initial;
		border-top: 1px solid #efeff4;
	}

	.top {
		background-color: #fff;
		color: #4a4a4a;
		width: 100%;
		padding-bottom: 10px;
	}

	.top p {
		padding-top: 10px;
		text-align: center;
		font-size: 20px;
		font-weight: bold;
		width: 90%;
		margin-left: 5%;
		margin-bottom: 40px;
	}

	.weui-flex__item {
		text-align: center;
		font-size: 22px;
		color: #2E79FF;
	}

	.placeholder1 {
		font-size: 14px;
	}

	.product {
		margin-top: 10px;
		background-color: #fff;
		width: 100%;
		box-sizing: border-box;
	}

	.product>p {
		text-align: center;
		margin: 15px 0px;
		color: #898989;
	}

	.product p span {
		color: #898989;

	}

	.picker {
		color: #000
	}

	.choose {
		background-color: #fff;
		margin-top: 10px;
		width: 100%;
		box-sizing: border-box;

	}

	.choose p {
		text-align: center;
		padding: 10px 0px;

	}

	.t {
		border-bottom: 1px solid #ddd
	}

	.weui-btn {
		margin: 25px 0px;
		width: 80%;
		color: #2E79FF;
		margin-left: 10%;
		border-color: #2E79FF
	}

	.driver {
		margin: 20px 3%;
		width: 94%;
		margin-top: 0px
	}

	.fontWight {
		font-weight: bold
	}

	.close {
		width: 94%;
		padding-right: 3%;
		text-align: right !important;
		font-size: 18px;
	}

	.driverInfo {

		text-align: center !important;
		color: #4a4a4a;
		font-size: 14px;
		box-sizing: border-box
	}

	.driverInfo:last-child {
		margin-bottom: 20px;
	}

	.footer {
		width: 100%;
		position: fixed;
		bottom: 0px;
		left: 0px;
		height: 50px;
		z-index: 9999999;
		border-top: 1px solid #ddd;
		background-color: #fff;
	}

	.footer span {
		float: left;
		width: 60%;
		height: 50px;
		line-height: 50px;
		text-align: center;
		color: red;
		font-size: 16px;
	}

	.footer button {
		color: #fff;
		float: left;
		width: 40%;
		height: 50px;
		line-height: 50px;
		background-color: #2E79FF;
		margin: 0px;
		border: none !important;
		border-radius: 0px !important;
		box-shadow: 0px 0px 0px #fff;
	}

	.weui-dialog__hd {
		padding: 10px 0px 20px 0px;
	}

	.fontSize {
		font-size: 18px;
	}

	.weui-dialog {
		width: 90%;
		max-width: 650rpx;
		top: 40%;
		left: 50%;
	}

	.weui-dialog__bd p {
		width: 100%;
		height: 30px;
		line-height: 30px;
		text-align: center;
		margin-bottom: 10px;
	}

	.weui-dialog__bd p:nth-child(2) {
		width: 70%;
		margin-left: 15%;
		color: #fff;
		background-color: #1fa4e4;
		height: 40px;
		line-height: 40px;
		border-radius: 5px;
	}

	.weui-dialog__bd p:nth-child(3) {
		width: 70%;
		background-color: #33d38c;
		margin-left: 15%;
		color: #fff;
		height: 40px;
		line-height: 40px;
		border-radius: 5px;
	}
</style>