<template lang='pug'>
  li.item(@mouseover='focus = true' @mouseleave='focus = false')
    span.item--name {{ item.name }}
    .item--icons
      svg.item--icon.trash(
        v-if='!isFavourite'
        v-show='focus'
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 14 18"
        width="14"
        height="18"
        @click='removeItem(item)'
      ): path(fill="#A6A6A6" fill-rule="evenodd" d="M11.765 6.067H1.58a.81.81 0 0 0-.807.877l.865 10.355c.033.397.365.701.762.701h8.546a.765.765 0 0 0 .762-.7l.864-10.356a.809.809 0 0 0-.807-.877zm.821-1.413H.766a.756.756 0 0 1-.732-.984l.361-1.368a.729.729 0 0 1 .587-.515c.291-.058 2.533-.259 3.077-.292a.512.512 0 0 0 .437-.339c.04-.12.294-.68.453-.881C5.022.183 5.344 0 6.673 0c1.328 0 1.657.183 1.73.275.159.201.415.76.454.881.038.121.19.326.437.339.543.033 2.786.234 3.076.292.291.057.504.238.587.515l.362 1.368a.756.756 0 0 1-.733.984z")

      svg.item--icon.star(
        v-show='item.favourite || focus'
        xmlns="http://www.w3.org/2000/svg"
        viewBox="0 0 21 20"
        width="21"
        height="20"
        @click='toggleFavourites(item)'
      ): path(:class="isFavourite && 'in-fav-list'" :fill="getSVGFill" fill-rule="evenodd" :d="getSVGPath")
</template>

<script>
import bus from '@/bus'

export default {
  name: 'country-item',
  data () {
    return {
      focus: false
    }
  },
  props: {
    item: {
      type: Object,
      required: true
    },
    index: Number,
    isFavourite: Boolean
  },
  computed: {
    getSVGPath () {
      return this.item.favourite
        ? 'M11.443.682l1.705 5.23c.188.58.73.971 1.34.971h5.519c.96 0 1.36 1.224.582 1.787l-4.465 3.232c-.494.358-.7.992-.51 1.57l1.704 5.231c.297.91-.748 1.666-1.524 1.104l-4.465-3.233a1.413 1.413 0 0 0-1.657 0l-4.465 3.233c-.776.562-1.822-.194-1.525-1.104l1.705-5.23a1.403 1.403 0 0 0-.512-1.57L.41 8.67c-.776-.563-.377-1.787.583-1.787h5.519c.61 0 1.151-.391 1.34-.97L9.558.683c.297-.91 1.588-.91 1.885 0'
        : 'M14.951 10.282a3.389 3.389 0 0 0-1.239 3.81l.741 2.275-1.95-1.412a3.402 3.402 0 0 0-4.004 0l-1.952 1.412.741-2.273a3.392 3.392 0 0 0-1.24-3.812L4.116 8.883h2.396a3.4 3.4 0 0 0 3.241-2.35l.747-2.29.746 2.29a3.4 3.4 0 0 0 3.242 2.35h2.395l-1.932 1.4zm5.056-3.399h-5.519a1.41 1.41 0 0 1-1.34-.97L11.442.682c-.297-.91-1.589-.91-1.886 0l-1.705 5.23c-.189.58-.73.97-1.34.97H.993c-.96 0-1.359 1.224-.583 1.786l4.465 3.233c.494.358.701.992.513 1.57l-1.707 5.231c-.296.91.75 1.666 1.526 1.104l4.465-3.233a1.41 1.41 0 0 1 1.656 0l4.466 3.233c.776.562 1.821-.194 1.524-1.104l-1.705-5.23a1.4 1.4 0 0 1 .511-1.57l4.465-3.234c.778-.562.378-1.786-.582-1.786z'
    },
    getSVGFill () {
      return this.item.favourite
        ? '#FAC92F'
        : '#A6A6A6'
    }
  },
  methods: {
    removeItem (item) {
      bus.$emit('itemToRemove', this.item)
      console.log(`Item removed: ${item.name}`)
    },
    toggleFavourites (item) {
      this.$set(item, 'favourite', !item.favourite)
      if (item.favourite) {
        console.log(`Item added to favourites: ${item.name}`)
        return
      }
      console.log(`Item excluded from favourites: ${item.name}`)
    }
  },
  beforeCreate () {
    console.log('CountryItem.vue has started loading!')
    console.time('CountryItem.vue loaded!')
  },
  mounted () {
    this.$nextTick(() => {
      console.timeEnd('CountryItem.vue loaded!')
    })
  }
}
</script>

<style lang='styl'>
.item
  display flex
  align-items center
  justify-content space-between
  background-color inherit
  transition background-color 400ms
  padding 0 24px
  height 36px

  &:hover
    background-color rgba(0, 0, 0, 0.05)

    & .in-fav-list
      fill #a6a6a6

  .item--name
    font-size 18px
    line-height 1.11
    color #4a4a4a

  .item--icons
    display flex
    align-items center

  .item--icon
    cursor pointer

    &:nth-child(2)
      margin-left 16.6px

  .trash
    width 13.4px
    height 18px

  .star
    width 21px
    height 20px

</style>
