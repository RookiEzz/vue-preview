<template>
    <!-- 过渡动画 -->
    <transition name="fade">
        <div class="img-view" @click="bigImg" >
            <!-- 遮罩层 -->
            <div class="img-layer"></div>
            <div class="img">
                <img :src="imgSrc" ref="imgHammer" style="margin-left:0;margin-top:0;">
            </div>
        </div>
    </transition>
</template>
<script>
export default {
    props: ['imgSrc'],
    methods: {
        bigImg() {
            // 发送事件
            this.$emit('clickit')
        }
    },
    mounted () {
        var myElement = this.$refs.imgHammer;
        console.log(myElement)
        var hammertime = new Hammer(myElement);
        hammertime.get('pinch').set({ enable: true });
        var scale;
        
        hammertime.on("pinchmove",e => {
            scale = scale || 1;
            myElement.style.transform = "scale("+(scale*e.scale)+")";
        });
        hammertime.on("pinchend",e => {
            
            scale = scale*e.scale;
        });
        
        var left,top;
        hammertime.on('panstart',function(event){
            console.log(left,top)
            left = parseFloat(myElement.style.marginLeft);
            top = parseFloat(myElement.style.marginTop);
        });
        hammertime.on("panmove",e => {
            myElement.style.marginLeft = left + e.deltaX + 'px';
            myElement.style.marginTop = top + e.deltaY + 'px';
        });
        hammertime.on("panend",e => {
            console.log('移动结束')
        })
    }
}
</script>
<style scoped>
/*动画*/
.fade-enter-active,
.fade-leave-active {
    transition: all .2s linear;
    transform: translate3D(0, 0, 0);
}

.fade-enter,
.fade-leave-active {
    transform: translate3D(100%, 0, 0);
}


/* bigimg */

.img-view {
    position: relative;
    width: 100%;
    height: 100%;
}

/*遮罩层样式*/
.img-view .img-layer {
    position: fixed;
    z-index: 999;
    top: 0;
    left: 0;
    background: rgba(0, 0, 0, 0.7);
    width: 100%;
    height: 100%;
    overflow: hidden;
}

/*不限制图片大小，实现居中*/
.img-view .img{
    display: -webkit-flex;
    display:flex;
    -webkit-align-items:center;
    align-items:center;
    height: 100%;
    -webkit-justify-content:center;
    justify-content:center;
}
.img-view .img img {
    max-width: 80%;
    display: block;
    position: absolute;
    margin: auto;
    z-index: 1000;
}
</style>