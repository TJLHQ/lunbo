<template>
  <div class="box"
       @touchstart.prevent="start"
       @touchmove.prevent="move"
       @touchend="end"
  >
    <ul>
      <li ref="li1">我是1</li>
      <li ref="li2">我是2</li>
    </ul>
    <ol>
      <li :class="{'active':currentShow=='aa'}">1</li>
      <li :class="{'active':currentShow=='bb'}">2</li>
    </ol>
  </div>
</template>
<script>
  export default{
    data(){
      return {
        currentShow: 'aa'
      }
    },
    created(){
      this.touch = {}
    },
    methods: {
      start(e){
        this.touch.initat = true;
        this.touch.startX = e.touches[0].pageX;
        this.touch.startY=e.touches[0].pageY;
        this.touch.moved=false;
      },
      move(e){
        if (!this.touch.initat) {
          return
        }
        const detalX = e.touches[0].pageX - this.touch.startX;
        const detalY = e.touches[0].pageX - this.touch.startX;
        console.log(detalX,detalY)
        if(Math.abs(detalY)>Math.abs(detalX)){
            return
        }
        if(!this.touch.moved){
            this.touch.moved=true;
        }
        const width = this.currentShow == 'aa' ? 0 : -window.innerWidth;
        const leftWidth = Math.min(0, Math.max(-window.innerWidth, width + detalX));
        this.touch.prevent = Math.abs(leftWidth / window.innerWidth);
        const opacity = 1 - this.touch.prevent;
        this.$refs.li1.style.opacity = opacity;
        this.$refs.li2.style.transitionDuration =0
        this.$refs.li2.style.transform = `translate3d(${leftWidth}px,0,0)`
      },
      end(){
          if(!this.touch.moved){
              return
          }
          let offsetWidth,opacity;
        if (this.currentShow == 'aa') {
            if(this.touch.prevent>0.1){
                offsetWidth=-window.innerWidth;
                opacity=0;
                this.currentShow='bb'
            }else{
              offsetWidth=0;
              opacity=1;
            }
        }else{
            if(this.touch.prevent<0.9){
              offsetWidth=0;
              opacity=1;
              this.currentShow='aa'
            }else{
              offsetWidth=-window.innerWidth;
              opacity=0;
            }
        }
        const time=300;
        this.$refs.li1.style.opacity = opacity;
        this.$refs.li2.style.transitionDuration = `${time}ms`
        this.$refs.li2.style.transform = `translate3d(${offsetWidth}px,0,0)`
        this.touch.initat=false;
        console.log(this.touch.initat)
        console.log(this.touch.moved)
      }
    }
  }
</script>
<style>
  * {
    padding: 0;
    margin: 0;
  }

  ul {
    list-style: none;
  }

  .box {
    height: 300px;
    overflow: hidden;
    position: relative;
  }

  ul {
    width: 200%;
    display: flex;
    flex-flow: row nowrap;
    justify-content: flex-start;
  }

  ul li:nth-of-type(1) {
    width: 100%;
    height: 300px;
    background: red;
  }

  ul li:nth-of-type(2) {
    width: 100%;
    height: 300px;
    background: deeppink;
  }

  ol {
    position: absolute;
    bottom: 20px;
    left: 0;
    width: 100%;
    text-align: center;
    height: 20px;
    line-height: 20px;
  }

  ol li {
    width: 20px;
    height: 20px;
    border-radius: 50%;
    background: yellow;
    display: inline-block;
    margin-right: 10px;
  }

  .active {
    width: 40px;
    background: blue;
  }
</style>

