<template>
  <div class="player">
    <!-- 歌曲名字 + 歌手名字-->
    <div class="song-title">
      <div class="song-name">{{ songInfo.name }}</div>
      <div class="song-sginer">{{ songInfo.singer }}</div>
    </div>
    <div class="song-content" @click="lyricSwitch">
      <div v-show="!showLyric">
        <!-- 歌曲logo+转动效果（点击显示歌词） -->
        <div class="song-logo">
          <van-image round width="150" height="150" :src="songInfo.logo" />
        </div>
        <!-- 收藏等操作按钮 -->
        <div class="song-btn"></div>
      </div>
      <!-- 歌词 -->
      <div v-show="showLyric">
        {{ songInfo.lyric }}
      </div>
    </div>
    <div class="song-operation">
      <!-- 滚动条+开始结束时间  -->
      <div class="song-slider"></div>
      <!-- 播放器 -->
      <div class="song-switch"></div>
    </div>
  </div>
</template>
<script lang="ts">
// 搜索展示组件
import { onMounted, reactive, ref, defineComponent } from 'vue'
import API from '@/api/api'
import { useRoute } from 'vue-router'

export default defineComponent({
  name: 'Player',
  components: {
  },
  setup () {
    const route = useRoute()
    const songInfo = reactive({
      songId: '',
      name: '',
      singer: '',
      lyric: '',
      logo: ''
    })
    const showLyric = ref(false)

    onMounted(() => {
      songInfo.songId = <string>route.query.songId
      methods.getMusicDetail()
    })

    const methods = {
      getMusicDetail () {
        API.musicDetail({ ids: songInfo.songId }).then((res: any) => {
          if (res.data.code === 200) {
            songInfo.name = res.data.songs[0].name
            songInfo.singer = res.data.songs[0].ar[0].name
            songInfo.logo = res.data.songs[0].al.picUrl
          }
        })
      },
      lyricSwitch () {
        showLyric.value = !showLyric.value
      },
      playMusic () {
        alert('播放功能待开放')
      }
    }
    return {
      ...methods,
      songInfo,
      showLyric
    }
  }
})
</script>
<style lang="less" scoped>
.player {
  width:100%;
  height:100%;
  background: rgba(0,0,0,0.6);
  .song-title {
    color: #fff;
    padding-top: .2rem;
    .song-sginer {
      font-size: .1rem;
      color: #999;
    }
  }
  .song-content {
    height: 80%;
    overflow: auto;
    display: flex;
    align-items: center;
    justify-content: center;
    .song-logo {
      animation: myRotate 5s linear infinite;
      transform: translate3d(0,0,0);
      box-shadow: 0 0 4rem #f4f4f4;
      border-radius: 50%;
    }
    @keyframes myRotate {
      0% {
        transform: rotate(0deg);
      }
      100% {
        transform: rotate(360deg);
      }
    }
  }
}
</style>
