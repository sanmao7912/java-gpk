<template lang="pug">
.topics
  h3 专题
  article.news-item(data-track-category="topics" data-track-item="a.topic-card")
    a.topic-card(v-for="topic, index in data.topics", :key="topic.id", :href="`/topic/${topic.id}`", :target="$store.state.target")
      img(v-lazy="topic.app_banner_url" w=241 h=155)
      .mask(v-if="!topic.title_visible")
        h4.multiline-text-overflow {{topic.title}}
        p {{topic.post_count}}篇文章
</template>

<script>
import api from 'store/api'

export default {
  data () {
    return {
      loading: true,
      data: {
        topics: [],
      }
    }
  },
  methods: {
    fetch () {
      this.loading = true;
      api.get(`topics?per=3`).then((result) => {
        this.data = result.data;
        this.loading = false;
      }).catch((err) => {
        console.log(err);
        // this.$message.error(err.toString())
      })
    },
  },
  beforeMount () {
    this.fetch()
  }
}
</script>

<style lang="stylus" scoped>
.topics
  border 1px solid #E4E4E4
  padding 16px
  margin-bottom 50px
  h3
    font-size 22px
    margin 0
  .news-item
    .topic-card
      margin 20px 0
      position relative
      overflow hidden
      display block
      img
        height 155px
        // display block
        // min-height 160px
      .mask
        display flex
        flex-direction column
        justify-content center
        align-items center
        position absolute
        top 0
        left 0
        background-color: rgba(0,0,0,.4);
        transition all .5s
        width 100%
        height 100%
        color #fff
        h4
          margin 0
          margin-bottom 10px
          padding 0px 20px
          line-height 1.5
          font-size 18px
          font-weight 500
        p
          margin 0
          font-size 14px
</style>
