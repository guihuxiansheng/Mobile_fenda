<template>
	<div class="kwenAsk">
		<div class="header">
			<span class="kwenAskQx" @click="kwenAskQx()">取消</span>
			<span class="kwenAskFb" :class="{showcolor:inputVal.length>0}" @click="kwenAskFb()">发布</span>
		</div>
		<div class="input">
			<textarea :placeholder="cateData.introduction + '48小时内无人抢答，将按支付路径全额退款。'" 
			maxlength="300" v-model="inputVal"></textarea>
			<p>￥10	
				<span>
					<div class="image">
					<img src="../../assets/pic.png"></div>
					<span>{{inputVal.length}}</span>
				/300</span>
			</p>
		</div>
		<div class="foot">
			<div class="niMing">
				匿名
				<mt-switch v-model="value" class="btn" @change="turn()"></mt-switch>
			</div>
			<div class="introduction">
				查看问答细则及责任声明
				<span></span>
			</div>
		</div>
	</div>
</template>
<script type="text/javascript">
import {Toast} from 'mint-ui'
export default {
  data () {
    return {
      value: true,
      inputVal: '',
      id: '',
      cateData: '', // 接收的数据
      whether: 1
    }
  },
  created () {
    this.id = this.$route.params.id
    this.init()
  },
  methods: {
    init: function () {
      this.$http
        .get('/api/kuaiwen/index')
        .then(rtnData => {
          this.cateData = rtnData.data[this.id - 1]
          // console.log(this.cateData)
        })
    },
    kwenAskQx: function () {
      this.$router.back(-1)
    },
    kwenAskFb: function () {
      if (this.inputVal.length > 0) {
        // this.$router.push('/pay')
        this.$http
        .post('/api/kuaiwen/ask', {
          content: this.inputVal,
          // picture
          whether: this.whether,
          userId: 2, // 暂时设为2
          kwCateId: this.cateData.id
        })
        .then(rtnData => {
          if (rtnData.data['status'] === 0) {
            this.$router.push('/kuaiwen')
          } else {
            Toast(rtnData.data['message'])
          }
        })
      } else {
        Toast('提出的问题不能为空')
      }
    },
    turn: function () {
      if (this.value === true) {
        this.whether = 1
        Toast('匿名后您的个人信息将保密~')
      } else {
        this.whether = 0
      }
    }
  }
}
</script>
<style type="text/css">
p,html,body,ul,li,a,i,div,h5{
  margin: 0;
  padding: 0;
  list-style: none;
}
a{
  text-decoration: none;
  color: #191919;
  display: inline-block;
}
.kwenAsk{
  font-size: 0.8rem;
  background: #f4f4f4;
}
.kwenAsk .header{
	position: fixed;
	left: 0;
	right: 0;
	top: 0;
	padding: 0.2rem 0.8rem;
	height: 2rem;
	line-height: 2rem;
	background: #fff;
	border: 1px solid #DED9D9;
	color: #F85F48;
	clear: both;
}
.kwenAsk .header .kwenAskFb{
	float: right;
	color: #999;
}
.kwenAsk .header .kwenAskFb.showcolor{
	color: #F85F48;
}
.kwenAsk .input{
	margin-top: 2.5rem;
	padding: 0.8rem 0.8rem 0 0.8rem;
	background: #fff;
	margin-bottom: 0.5rem;
}
.kwenAsk .input >textarea{
	width: 100%;
	height: 6rem;
	border: none;
	resize: none;
	outline: none;
}
.kwenAsk .input >p{
	height: 2rem;
	line-height: 2rem;
	color: #F85F48;
}
.kwenAsk .input >p >span{
	float: right;
	color: #999;
}
.kwenAsk .input >p >span .image{
	display: inline-block;
}
.kwenAsk .input >p >span .image >img{
	height: 1rem;
	vertical-align: middle;
	margin-top: -0.5rem;
}
.kwenAsk .foot{
	background: #fff;
	
}
.kwenAsk .foot .niMing{
	clear: both;
	height: 2.5rem;
	line-height: 2.5rem;
	border-bottom: 1px solid #DED9D9;
	padding: 0 0.8rem;
}
.kwenAsk .foot .niMing .btn{
	display: inline-block;
	float: right;
	margin-top: 0.4rem;
	width: 2.53rem;
	height: 2rem;
}
.kwenAsk .foot .introduction{
	clear: both;
	padding: 0 0.8rem;
	height: 2.5rem;
	line-height: 2.5rem;
}
.kwenAsk .foot .introduction >span{
	float: right;
	position: relative;
}
.kwenAsk .foot .introduction >span:before{
	content: '';
	width: 0.7rem;
	height: 0.1rem;
	background: #999;
	position: absolute;
	top: 0.923rem;
	left: -0.8rem;
	transform: rotate(-150deg);
}
.kwenAsk .foot .introduction >span:after{
	content: '';
	width: 0.7rem;
	height: 0.1rem;
	background: #999;
	top: 1.25rem;
	left: -0.8rem;
	position: absolute;
	transform: rotate(150deg);
}
</style>