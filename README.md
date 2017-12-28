# vue-preview
The component is used to preview pic with big size, you can scale the image and move it.

# component requirements
Hammer.js(You can search on GITHUB)

# pros

SRC(the image source)
@clickit(control the mode status)

# example

<big-img v-if="showImg" @clickit="viewImg" :imgSrc="imgSrc"></big-img>

Methods

clickImg(e) {
    this.showImg = true;
    // 获取当前图片地址
    this.imgSrc = e.currentTarget.src;
},
viewImg(){
    this.showImg = false;
},

