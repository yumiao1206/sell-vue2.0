<template>
	<div class="ratingselect">
		<div class="rating-type">
			<span @click="select(2,$event)" class="block positive" :class="{'active':selectType1===2}">{{desc.all}}<span class="count">{{ratings.length}}</span></span>
			<span @click="select(0,$event)" class="block positive" :class="{'active':selectType1===0}">{{desc.positive}}<span class="count">{{positives.length}}</span></span>
			<span @click="select(1,$event)" class="block negative" :class="{'active':selectType1===1}">{{desc.negative}}<span class="count">{{negatives.length}}</span></span>
		</div>
		<div @click="toggleContent($event)" class="switch" :class="{'on':onlyContent1===true}">
			<span class="icon-check_circle"></span>
			<span class="text">只看有内容的评价</span>
		</div>
	</div>
</template>

<script type="text/javascript">
const POSITIVE=0
const NEGATIVE=1
const ALL=2

export default{
	props:{
		ratings:{
			type:Array,
			default(){
				return []
			}
		},
		selectType:{
			type:Number,
			default:ALL
		},
		onlyContent:{
			type:Boolean,
			default:true
		},
		desc:{
			type:Object,
			default(){
				return {
					all:'全部',
					positive:'满意',
					negative:'不满意'
				}
			}
		}
	},
	computed:{
		positives(){
			return this.ratings.filter((rating)=>{
				return rating.rateType===POSITIVE
			})
		},
		negatives(){
			return this.ratings.filter((rating)=>{
				return rating.rateType===NEGATIVE
			})
		}
	},
	data(){
		return {
			//子组件不能直接修改父组件传过来的值，用一个变量将父组件传过来的值保存起来
			selectType1:this.selectType,
			onlyContent1:this.onlyContent
		}
	},
	methods:{
		select(type,event){
			if(!event._constructed){
				return;
			}
			this.selectType1=type
			this.$emit('ratingtypeselect',type)
		},
		toggleContent(event){
			if(!event._constructed){
				return;
			}
			this.onlyContent1=!this.onlyContent1
			this.$emit('togglecontent',this.onlyContent1)
		}
	},
}
</script>

<style type="text/css">
.ratingselect .rating-type{
	padding:18px 0;
	margin:0 18px;
	border-bottom:1px solid rgba(7,17,27,0.1);
	font-size:0;
}
.ratingselect .rating-type .block{
	display:inline-block;
	padding:8px 12px;
	border-radius:2px;
	margin-right:8px;
	color:rgb(77,85,93);
	line-height:16px;
	font-size:12px;
}
.ratingselect .rating-type .block .count{
	font-size:8px;
	margin-left:2px;
}
.ratingselect .rating-type .block.positive{
	background:rgba(0,160,220,0.2);
}
.ratingselect .rating-type .block.positive.active{
	color:#fff;
	background:rgba(0,160,220,1);
}
.ratingselect .rating-type .block.negative{
	background:rgba(77,85,93,0.2);
}
.ratingselect .rating-type .block.negative.active{
	color:#fff;
	background:rgba(77,85,93,1);
}
.ratingselect .switch{
	padding:12px 18px;
	line-height:24px;
	border-bottom:1px solid rgba(7,17,27,0.1);
	color:rgb(147,153,159);
	font-size:0;
}
.ratingselect .switch .icon-check_circle{
	display: inline-block;
	vertical-align: top;
	font-size:24px;
	margin-right:4px;
}
.ratingselect .switch.on .icon-check_circle{
	color:#00c850;
}
.ratingselect .switch .text{
	display: inline-block;
	vertical-align: top;
	font-size:12px;
}
</style>