<template>
  <div id="app">
    <!--主体-->
    <mm-header />
    <router-view />
    <!--播放器-->
    <audio ref="mmPlayer"></audio>
  </div>
</template>

<script>
import { mapMutations, mapActions } from 'vuex'
import { getPlaylistDetail } from 'api'
import { MMPLAYER_CONFIG } from '@/config'
import MmHeader from 'components/mm-header/mm-header'

// 移除了 MmDialog 组件的引入

const VERSION_INFO = `<div class="mm-dialog-text text-left">
版本号：${VERSION}（${process.env.VUE_APP_UPDATE_TIME}）<br/>
1、 采用新版图标<br>
2、 修复音乐搜索<br>
3、 优化滚动条样式
</div>`

export default {
  name: 'App',
  components: {
    MmHeader,
    // 移除了 MmDialog 组件注册
  },
  created() {
    // 移除了版本信息设置相关代码
    // 移除了弹窗显示逻辑

    // 获取正在播放列表
    getPlaylistDetail(MMPLAYER_CONFIG.PLAYLIST_ID).then((playlist) => {
      const list = playlist.tracks.slice(0, 100)
      this.setPlaylist({ list })
    })

    // 设置title
    let OriginTitile = document.title
    let titleTime
    document.addEventListener('visibilitychange', function () {
      if (document.hidden) {
        document.title = 'Music'
        clearTimeout(titleTime)
      } else {
        document.title = '嗨，又见面了!'
        titleTime = setTimeout(function () {
          document.title = OriginTitile
        }, 2000)
      }
    })

    // 设置audio元素
    this.$nextTick(() => {
      this.setAudioele(this.$refs.mmPlayer)
    })

    // 移除了版本检测和弹窗显示逻辑
  },
  methods: {
    ...mapMutations({
      setAudioele: 'SET_AUDIOELE',
    }),
    ...mapActions(['setPlaylist']),
  },
}
</script>

<style lang="less">
/* 样式保持不变 */
</style>
