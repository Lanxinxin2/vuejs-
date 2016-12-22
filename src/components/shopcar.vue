<template>
	<div class="marginAuto">
		<table id="cartTable">
		    <thead>
		        <tr>
		            <th><label><input class="check-all check" type="checkbox" @click="allCheck($event)" />&nbsp;全选</label></th>
		            <th>商品</th>
		            <th>单价</th>
		            <th>数量</th>
		            <th>小计</th>
		            <th>操作</th>
		        </tr>
		    </thead>
		    <tbody>
		        <tr v-for="(v,k) in datashop">
		            <td class="checkbox"><input class="check-one check" type="checkbox" @click="check($event)" v-model="checkboxArr" :value="k" /></td>
		            <td class="goods"><img :src="v.img" alt=""/><span>{{ v.name }}</span></td>
		            <td class="price">{{ v.price }}</td>
		            <td class="count">
		                <span class="reduce" @click="reduce(k)">-</span>
		                <input class="count-input" type="text" :value="v.count"/>
		                <span class="add" @click="add(k)">+</span></td>
		            <td class="subtotal">{{ v.price*v.count | currency }}</td>
		            <td class="operation"><span class="delete" @click="del(index)">删除</span></td>
		        </tr>
		    </tbody>
		</table>
		<div class="foot" id="foot">
		    <label class="fl select-all"><input type="checkbox" class="check-all check" @click="allCheck($event)" />&nbsp;全选</label>
		    <a class="fl delete" id="deleteAll" href="javascript:void(0);" @click="allDel">删除</a>
		    <div class="fr closing">结 算</div>
		    <div class="fr total">合计：￥<span id="priceTotal">{{ allTotal | currency }}</span></div>
		    <div class="fr selected" id="selected">已选商品
		        <span id="selectedTotal">{{ allCount }}</span>件
		    </div>
		</div>
	</div>
</template>

<script>

	import Vue from 'vue'

	//过滤器
	Vue.filter('currency', function(value) {
	  if (!value) { return 0 }
	  return  value.toFixed(2)
	})

	export default {
		data () {
			return {
				datashop: datashop,
				allTotal: 0,
				allCount: 0,
				checkboxArr: []
			}
		},
		computed: {
			allCount: function() {
		      var count = 0
		      for (var i = 0; i < this.datashop.length; i++) {
		        count += this.datashop[i].count
		      }
		      return count
		    },
		    allTotal: function() {
		      var total = 0
		      for (var i = 0; i < this.datashop.length; i++) {
		        total += this.datashop[i].price * this.datashop[i].count
		      }
		      return total
		    }
		},
		methods: {
			compare: function(value1, value2){
				if (value1 < value2) {
				    return -1;
				} else if (value1 > value2) {
				    return 1;
				} else {
				    return 0;
				}
			},
			add: function(index){
				this.datashop[index].count ++
			},
			reduce: function(index){
				if(this.datashop[index].count > 1) this.datashop[index].count--
			},
			del: function(index){
				this.datashop.splice(index,1)
			},
			check: function(event){
				var e = event || window.event
				var checkAll = document.querySelectorAll(".check-all")
				if( !e.currentTarget.checked ){
					for(var i=0;i<checkAll.length;i++)   
					{   
						checkAll[i].checked = false
					}
				}
			},
			allCheck: function(event){
				var e = event || window.event
				var checkAll = document.querySelectorAll(".check-all")
				if(e.currentTarget.checked){
					for(var i=0;i<checkAll.length;i++)   
					{   
						checkAll[i].checked = true
					}
					for (var i = 0; i < this.datashop.length; i++) {
				        this.checkboxArr.push(i)
				    }
				}else{
					this.checkboxArr = []
				}
			},
			allDel: function(){
				if(this.checkboxArr.length > 0){
					var checkAll = document.querySelectorAll(".check-all")
					this.checkboxArr.sort(this.compare);
					for (var i = 0; i < this.checkboxArr.length; i++) {
						this.datashop.splice(this.checkboxArr[i] - i,1)
				    }
					for(var i=0;i<checkAll.length;i++){   
						checkAll[i].checked = false
					}
				    this.checkboxArr = []
				}
			}
		}
	}

	var datashop = [{
		img: require('../assets/images/img1.jpg'),
		name: "Casio/卡西欧 EX-TR350",
		price: 5999.88,
		count: 1
	},{
		img: require('../assets/images/img2.jpg'),
		name: "Canon/佳能 PowerShot SX50 HS",
		price: 3888.50,
		count: 1
	},{
		img: require('../assets/images/img3.jpg'),
		name: "Sony/索尼 DSC-WX300",
		price: 1428.50,
		count: 1
	},{
		img: require('../assets/images/img4.jpg'),
		name: "Fujifilm/富士 instax mini 25",
		price: 640.60,
		count: 3
	}]
</script>