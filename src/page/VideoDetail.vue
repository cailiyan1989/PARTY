<template>
  <div class="LearnDetail">
    <BackBar></BackBar>
    <div class="DetailBox">
      <Row>
        <Col span="24">
          <h1>{{Detail.fname}}</h1>
          <h3>拍摄党支部：{{Detail.psdparty}}</h3>
          <p>拍摄时间：{{Detail.psdate}}</p>
          <Card :bordered="false" dis-hover>
              <div style="text-align:center">
                  <!-- <video id="VideoContain" src="https://zgeqscjdglj.org/upload/upload/files\20171226\Free-Converter.com-nk3_cloud-50070812.mp4
"   enable-danmu danmu-btn controls :poster="Detail.fimage" preload>您的浏览器不支持改视频！</video>  -->
<video id="my-video" class="video-js vjs-big-play-centered" controls preload
      :poster="Detail.fimage" data-setup="{}" :src="Detail.video1">
      <source :src="Detail.video1" type="video/mp4">
      <p class="vjs-no-js">
        您的浏览器不支持改视频！
      </p>
      </video>

<!-- <video-player  class="vjs-custom-skin"
                 ref="videoPlayer"
                 :options="playerOptions"
                 :playsinline="true"
                 >
  </video-player> -->

                  <div v-html="Detail.stud"></div>
              </div>
          </Card>
        </Col>
      </Row>
    </div>
     <Spin v-if="ifLoading"></Spin>
  </div> 
</template>
<script>
import Vue from 'vue'
import axios from 'axios'
import BackBar from '../components/BackBar'
import Spin from '../components/Spin'
import {timestampToFormatTime} from '../util/utils'
import Video from '../util/video.min.js'
// require('videojs-flash')
  export default{
    data: function () {
      return {
        Detail:'',
        ifLoading:false,
        Source:''
        // playerOptions: {
        //   // videojs options
        //   muted: false,
        //   language: 'zh-CN',
        //   playbackRates: [0.7, 1.0, 1.5, 2.0],
        //   techOrder: ['flash'],
        //   // techOrder: ['flash', 'html5'], // 兼容顺序
        //   // sourceOrder: true,
        //   // flash: { hls: { withCredentials: false } },
        //   // html5: { hls: { withCredentials: false } },
        //   sources:'',
        //   poster: "",
        //   controlBar: { // 配置控制栏
        //   timeDivider: true, // 时间分割线
        //   durationDisplay: true, // 总时间
        //   progressControl: true, // 进度条
        //   customControlSpacer: true, // 未知
        //   fullscreenToggle: true // 全屏
        // },
        // }
      }
    },
    mounted: function () {
      
    },
    created: function () {
      let _this = this
      let ID = this.$router.history.current.params.id
      axios.get(R_PRE_URL+'/selectvideonr.do?id='+ID
      ).then((res)=> {
        let reg = /[\\\/]/g
        let temp = res.data[0]
        temp.video1 = temp.video1.replace(reg, "/")
        temp.psdate = timestampToFormatTime(temp.psdate)
        this.Detail = temp
        this.$store.state.VideoSource = temp.video1

        // this.playerOptions.poster = temp.fimage
        // this.playerOptions.sources = [{
        //     type: "",
        //     src: temp.video1//"static/Free-Converter.com-nk3_cloud-69092250.swf"//
        //   }]
        var myPlayer = videojs('my-video');
        
        videojs("my-video").ready(function(){
          var myPlayer = this;
          myPlayer.on("progress", function(){
              //_this.ifLoading = false
          });
          myPlayer.play();
          

        })
        // Video.videojs("example_video_1", {}, function(){
          
        // });

        
      }).catch((error)=> {
        console.log(error)
      })
      
    },
    computed: {
      player() {
        return this.$refs.videoPlayer.player
      },
      VideoSource(){
        return this.$store.state.VideoSource
      }
      
    },
    watch: {
      VideoSource(){
        return this.$store.state.VideoSource
      }
      
    },
    components: {
      BackBar,
      Spin
      

    },
    methods: {
    }
  }
</script>
<style lang="scss">
.LearnDetail{
  .DetailBox{
    margin: 70px auto 0 auto;
    #VideoContain{
      width: 100%;
      height: auto;
    }
    .ivu-card{
      width: 100%;
      overflow: hidden;
    }
  }
  .video-js{
    width: 100% !important;
  }
}

</style>
