<template>
    <div class="pie" v-bind:style="colorStyle">
        <strong v-if="this.isShowNumber=='true'" v-bind:style="strongStyle">{{(this.value * 100).toFixed()}}%</strong>
        <div class="div_1" v-bind:style="div1Style"></div>
        <div class="div_2" v-bind:style="div2Style"></div>
        <span v-bind:style="ringStyle" v-if="this.ring!=0"></span>
    </div>
</template>

<script>
    export default {
        name: "VuePie",
        props: {
            "value": String, 
            "ring" : String,
            "isShowNumber" : String,
            "color" : String,
            "rotate" : String,
            "isAnimate" : String
        },
        data() {
            return {
                ringStyle : {},
                colorStyle : {},
                div1Style : {
                    transform : 'rotate(0turn)'
                },
                div2Style : {
                    opacity : 0,
                    transform : 'rotate(0turn)'
                },
                strongStyle : {}
            }
        },
        methods: {
            initPie(){
                var self = this;
                var isAnimate = this.isAnimate; // 是否动画
                if (isAnimate=='true') {   
                    setTimeout(self._showPie,100);
                }else{
                    self._showPie()
                }
            },
            _showPie(){
                var self = this;
                var percent = this.value, // 百分比
                    isRing = this.ring || 0,  // 是否空心
                    color = this.color,  // 自定义颜色
                    rotate = this.rotate,    // 是否旋转
                    isAnimate = this.isAnimate || 'false'; // 是否动画

                if (isRing) {
                    var w = isRing*100;
                    this.ringStyle = {
                        width : w + '%',
                        height : w + '%',
                        top: (50-w/2) +'%',
                        left: (50-w/2) +'%',
                        background:'#fff'
                    }
                }

                if(color){
                    this.colorStyle.backgroundImage = 'linear-gradient(to right, transparent 50%, '+ color +' 0)';
                    this.div2Style.background=color;
                }            
                if (percent > 0.5) {   
                    self.div1Style.transform = 'rotate(0.5turn)';
                    if (isAnimate=='true') {
                        setTimeout(function(){
                            self.div2Style.opacity = 1;
                            self.div2Style.transform = 'rotate('+ (percent-0.5) +'turn)';
                        },800);
                    }else{
                        self.div2Style.opacity = 1;
                        self.div2Style.transform = 'rotate('+ (percent-0.5) +'turn)';
                    }
                }else{
                    self.div1Style.transform = 'rotate('+ percent +'turn)';
                }

                if(rotate){
                    this.colorStyle.transform = 'rotate('+ rotate +'deg)';
                    // 文字转回来
                    this.strongStyle.transform = 'rotate('+ -rotate +'deg)';
                }
            }
        },
        created() {
            this.initPie();
        }
    }
</script>

<style lang="less" rel="stylesheet/less">
    .pie{
        position: relative;
        z-index: 0;
        left:0;
        top:0;
        width: 140px;
        height: 140px;
        line-height: 140px;
        border-radius:50%;
        text-align: center;
        color: #000;
        font-size: 140%;
        background-color:#f0f0f0;
        background-image: linear-gradient(to right, transparent 50%, yellowgreen 0);
        cursor:pointer;
        overflow: hidden;
        strong{
            position: absolute;
            width: 100%;
            left: 0;
            z-index: 1;
            margin:0;
            padding:0;
        }
        span{
            display: block;
            border-radius: 50%;
            position: absolute;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            z-index: 0;
            margin:0;
            padding:0;
        }
        div{
            position:absolute;
            top:0;
            width:60%;
            height:100%;
            left:50%;
            transform:rotate(0turn);
            transform-origin: center left;
            transition:transform 0.7s linear;
            margin:0;
            padding:0;
        }
        >.div_1{
            background-color: inherit;
            z-index: -2;
        }
        >.div_2{
            opacity: 0;
            z-index: -1;
            background-color: yellowgreen;
        }
    }
</style>