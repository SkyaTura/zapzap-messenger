<template lang="pug">
.fill-height
  v-app-bar(color='primary', app)
    v-toolbar-title ZapZap Messenger
    v-spacer
    v-btn(icon)
      v-icon search
    v-btn(icon)
      v-icon more_vert
    template(#extension)
      v-tabs.tabs(grow, color="accent" v-model="tab")
        v-tab
          v-icon photo_camera
        v-tab Chats
        v-tab Status
        v-tab Calls
  v-tabs-items.fill-height(v-model='tab')
    v-tab-item
      v-container
        span Camera
    v-tab-item
      v-btn(fixed fab bottom right color="accent")
        v-icon message
      v-list.pt-0(two-line)
        template(v-for="(chat, index) in chats")
          v-divider(v-if="index" :key="index")
          v-list-item(:key="chat.id" @click="")
            v-list-item-avatar(size="48")
              v-img(:src="chat.contact.picture")
            v-list-item-content
              v-list-item-title
                v-row.ma-0
                  span {{ chat.contact.name }}
                  v-spacer
                  .text-caption.message-time {{ getTime(chat.sentAt) }}

              v-list-item-subtitle {{ chat.lastMessage }}

    v-tab-item
      v-container status
    v-tab-item
      v-container calls
</template>

<script>
import faker from 'faker'

export default {
  data: () => ({
    tab: 1,
    chats: Array(25)
      .fill(null)
      .map((_, id) => ({
        id,
        contact: {
          name: faker.name.findName(),
          picture: `https://i.pravatar.cc/150?img=${id}`,
        },
        lastMessage: faker.lorem.words(),
        sentAt: faker.date.recent(),
      }))
      .sort((a, b) => {
        if (a.sentAt > b.sentAt) return -1
        if (a.sentAt < b.sentAt) return 1
        return 0
      }),
  }),
  methods: {
    getTime(time) {
      const date = this.$moment(time)
      return date.isSame(new Date(), 'day')
        ? date.format('HH:mm')
        : date.format('DD/MM/YYYY')
    },
  },
}
</script>

<style lang="sass" scoped>
.message-time
  opacity: 0.5
.fill-height
  height: 100%
.tabs
  &::v-deep
    .v-slide-group__content
      & > .v-tab:nth-child(2)
        flex: 0 0 48px
        min-width: 48px
        padding: 0
</style>
