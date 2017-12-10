<template>
  <div id="app">
    <div class="body">
      <div class="easel-top"></div>
      <div class="easel">
        <canvas class="canvas" id="canvas" width="510" height="330" v-on:mousedown="handleMouseDown" v-on:mousemove="handleMouseMove" v-on:mouseup="handleMouseUp"></canvas>
      </div>
      <div class="easel-bottom"></div>
      <div class="easel-foot">
        <div class="easel-feet easel-feet-1"></div>
        <div class="easel-feet easel-feet-2"></div>
      </div>
      <div class="buttons">
        <div class="btn reset" v-on:click="reset">Reset</div>
        <div class="btn save">Save</div>
        <!-- <div style="color:#888">X:{{currentMousePos.x}}, Y:{{currentMousePos.y}}</div> -->
      </div>
    </div>
    <div class="tools">
      <div class="brushes">
        <div class="brush marker"><img src="https://78.media.tumblr.com/744458020f539c5c4811f7f37629335e/tumblr_ozz4qdyzbN1wbgtmko1_1280.png" alt="marker" v-on:click="get_brush_marker" v-bind:class="{b_selected:brushes[0].selected}"></div>
        <div class="brush painting"><img src="https://78.media.tumblr.com/b6653d359596807edcca0d73f408a0e3/tumblr_ozz66tq8sr1wbgtmko1_1280.png" alt="painting" v-on:click="get_brush_paintB" v-bind:class="{b_selected:brushes[1].selected}"></div>
      </div>
      <div class="colors">
        <div class="color-block" v-for="(color,id) in colors" v-bind:style="get_color(id)" v-on:click="current_color(id)" v-bind:class="{selected:color.selected}"></div>
      </div>
      <div class="lineWidth">
        <div class="lineWidth_block" v-for="(i,id) in lineWidth" v-bind:style="get_lineWidth(id)" v-on:click="current_lineWidth(id)" v-bind:class="{selected:i.selected}"></div>
      </div>
    </div>
  </div>
</div>
</template>

<script>

export default {
  name: 'app',
  data(){
    return {
      colors:[{id:'white',num:'#F2EDE3',selected:false},{id:'yellow',num:'#F0ED78',selected:false},{id:'orange',num:'#F2A036',selected:false},{id:'red',num:'#EB685A',selected:false},{id:'green',num:'#86C13B',selected:false},{id:'blue',num:'#2586C9',selected:false},{id:'purple',num:'#8D61A6',selected:false},{id:'black',num:'#3E3A39',selected:true}],
       lineWidth:[{id:'5',size:5,selected:false} , {id:'10',size:10,selected:true} , {id:'15',size:15,selected:false} , {id:'20',size:20,selected:false}],
        brushes:[{id:'marker',selected:true},
                 {id:'paintBrush',selected:false}],
        cur_color:'#3E3A39',
        cur_lineWidth:10,
        cur_mouse: {
          x:0, y:0
        },
        mouse_down: false
    }
  },
  methods:{
    get_color(id){
      var self=this;
      //console.log(self.colors[id]);
      return {'background-color':self.colors[id].num};
    },
    get_lineWidth(id){
      var self=this;
      //console.log(self.colors[id]);
      return {'width':self.lineWidth[id].size+'px','height':self.lineWidth[id].size+'px'};
    },
    get_brush_marker(){
      this.brushes[0].selected=true;
      this.brushes[1].selected=false;
      console.log(this.brushes);
    },
    get_brush_paintB(){
      this.brushes[1].selected=true;
      this.brushes[0].selected=false;
      console.log(this.brushes);
    },
    current_color(id){
      this.cur_color=this.colors[id].num;
      for (var i=0; i<this.colors.length;i++) {
        if (id==i) {
          this.colors[i].selected=true;
          console.log(this.colors[i].selected);
        } else {
          this.colors[i].selected=false;
        }
      }
      //console.log(this.cur_color);
    },
    current_lineWidth(id){
      this.cur_lineWidth=this.lineWidth[id].size;
      for (var i=0; i<this.lineWidth.length;i++) {
        if (id==i) {
          this.lineWidth[i].selected=true;
          console.log(this.lineWidth[i].selected);
        } else {
          this.lineWidth[i].selected=false;
        }
      }
    },
    handleMouseDown(e){
      this.mouse_down = true;
      var ctx = document.getElementById("canvas").getContext("2d");
      this.cur_mouse= {
        x:e.pageX, y:e.pageY };
        ctx.beginPath();
        if (this.brushes[1].selected) {
          ctx.lineJoin = ctx.lineCap = 'round';
          ctx.shadowBlur = 10;
          ctx.shadowColor = this.cur_color;
        } else {
          ctx.lineJoin = ctx.lineCap = 'square';
          ctx.shadowColor = "transparent";
          ctx.shadowBlur = 0;
        }
        ctx.moveTo(this.currentMousePos.x,this.currentMousePos.y);
      
    },
    handleMouseUp() {
      this.mouse_down = false;
    },
    handleMouseMove(e){
      this.cur_mouse= {
        x:e.pageX, y:e.pageY
      };
      this.draw();
    },
    draw(){
      if (this.mouse_down==true) {
        var ctx = document.getElementById("canvas").getContext("2d");
        //ctx.clearRect(0,0,600,400);
        ctx.strokeStyle=this.cur_color;
        ctx.lineWidth=this.cur_lineWidth;
        ctx.lineTo(this.currentMousePos.x, this.currentMousePos.y);
        ctx.stroke();
      } 
    },
    reset(){
      var ctx = document.getElementById("canvas").getContext("2d");
      ctx.clearRect(0,0,510,330);
    },
    // save: function(){
    //   var _canvas = document.getElementById('canvas');
    //   var url = _canvas.toDataURL(); //利用toDataURL() 把canvas轉成data:image
    //   this.href = url;
    // }
  },
  computed:{
    currentMousePos(){
      var c = document.getElementById("canvas");
      var rect = c.getBoundingClientRect();
      return {
        x: this.cur_mouse.x - rect.left,
        y: this.cur_mouse.y - rect.top
      };
    }
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css?family=Ubuntu+Condensed');
$bg:#A3C8C6;
$easel:#E8C371;
$pink:#DEAAA0;
$white:#F2EDE3;
$brown:#C89970;
$gray:#46939B;
$border:#DBD5CA;
@mixin size($w,$h:$w) {
  width:$w;
  height:$h;
}
* {
  box-sizing: border-box; 
  &:before, &:after{
    content:'';
    position:absolute;
  } 
}
html, body, .tools, .body, .easel, #app,.colors, .lineWidth, .buttons {
  display:flex;
  display:-webkit-flex;
  align-items:center;
  align-items:-webkit-center;
}
html, body {
  @include size(100%);
  background: $bg;
  margin: 0;
  padding: 0;
  justify-content:center;
  justify-content:-webkit-center;
  font-family: 'Ubuntu Condensed', sans-serif;
}
.body {
  @include size(600px, 600px);
  flex-direction:-webkit-column;
  flex-direction:column;
  .easel-top {
    @include size(120px,15px);
    background: $easel;
    position:relative;
    top:7px;
    display:flex;
    justify-content:center;
    justify-content:-webkit-center;
    z-index:2;
    box-shadow: 0px 4px 0.5px rgba(black,.1),inset 0px 3px 0px rgba($white,.3);
    &:before {
      @include size(70px,40px);
      border-radius:3px;
      background: $easel;
      bottom:0;
      box-shadow:inset 0px 3px 0px rgba($white,.3);
    }
    &:after {
      @include size(15px,15px);
      border-radius: 50%;
      background: $brown;
      box-shadow:.5px 2px .5px rgba(black,.3),inset -.5px -2px 0px rgba(black,.1);
      top:-15px;
    }
  }
  .easel {
    @include size(540px, 360px);
    background: $easel;
    border-radius:5px;
    box-shadow:inset 0px 3px 0px rgba($white,.5);
    justify-content:center;
    z-index:1;
    .canvas {
      @include size(510px, 330px);
      background: $white;
      box-shadow:2px 2px .3px rgba($brown,.5);
    }
  }
  .easel-bottom {
    position:relative;
    @include size(600px,25px);
    border-radius:3px;
    background: $easel;
    margin-top: -5px;
    z-index:3;
    box-shadow:inset 0px -3px 0px rgba($brown,.7),inset 0px 3px 0px rgba($white,.3);
  }
  .easel-foot{
    display:flex;
  .easel-feet {
    position:relative;
    @include size(50px, 80px);
    background: $easel;
    display:flex;
    justify-content:center;
    margin-top:-15px;
    &:before {
      @include size(30px, 120px);
      background: $easel;
      top:80px;
    }
    &:after {
      @include size(18px);
      top:40px;
      border-radius:50%;
      background: $brown;
    }
    &.easel-feet-1 {
      transform:rotate(10deg);
      left:-130px;
      box-shadow:inset -4px 0px 0px rgba($brown,.5);
      &:before {
        box-shadow:inset 0px 4px .3px rgba(black,.1),inset -3px 0px 0px rgba($brown,.5);
      }
    }
    &.easel-feet-2 {
      transform:rotate(-10deg);
      right:-130px;
      box-shadow:inset 4px 0px 0px rgba($brown,.5);
      &:before {
        box-shadow:inset 0px 4px .3px rgba(black,.1),inset 3px 0px 0px rgba($brown,.5);
      }
    }
  }
  }
}
.tools {
  @include size(150px,700px);
  justify-content:center;
  flex-direction:column;
  .brushes {
    @include size(130px,260px);
    .brush {
      margin-top:20px;
      @include size(130px,80px);
    }
    img {
      cursor:pointer;
      height:150px;
      transform:rotate(55deg);
      margin-top:-30px;
      margin-left:40px;
      
      &.b_selected {
        border:2px solid $border;
      }
    }
  }
  .colors {
    @include size(100px,180px);
    flex-direction:column;
    flex-wrap:wrap;
    .color-block {
      @include size(30px);
      margin-top:10px;
      cursor:pointer;
    }
  }
  .lineWidth {
    @include size(130px,180px);
    flex-direction:column;
    .lineWidth_block {
      margin-top:20px;
      background: #000;
      cursor:pointer;
    }
  }
  .selected {
    border:3px solid $border;
  }
}
.buttons {
  @include size(170px,100px);
  flex-direction:column;
  .btn {
    @include size(80px);
    border:2px solid rgba($gray,.85);
    margin-bottom:17px;
    font-size: 18px;
    text-align:center;
    padding:10px 5px;
    font-size: 22px;
    color: $gray;
    cursor:pointer;
    transition:.3s;
    &:hover {
      background: $gray;
      color: $pink;
    }
  }
}

</style>
