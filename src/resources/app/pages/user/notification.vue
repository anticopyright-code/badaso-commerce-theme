<template>
  <div>
    <Head :title="$page.props.name" />

    <div class="pt-8 container hidden sm:grid grid-cols-profile">
      <div class="pr-8">
        <div class="h-22 flex items-center border-b">
          <div class="h-16 w-16 items-center justify-center clip-circle">
            <img :src="user.avatar" alt="User's avatar" class="object-cover h-16 w-16">
          </div>
          <div class="inline-block flex-wrap pl-2">
            <div class="text-sm font-medium w-full line-clamp-1 text-gray-700">{{ user.name }}</div>
            <Link :href="route('badaso.commerce-theme.profile')" class="text-sm text-gray-500 w-full flex items-center gap-1 cursor-pointer">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4" viewBox="0 0 20 20" fill="currentColor">
                <path d="M13.586 3.586a2 2 0 112.828 2.828l-.793.793-2.828-2.828.793-.793zM11.379 5.793L3 14.172V17h2.828l8.38-8.379-2.83-2.828z" />
              </svg>
              Ubah Profil
            </Link>
          </div>
        </div>
        <div class="mt-4 flex items-center gap-y-3 flex-wrap">
          <Link :href="route('badaso.commerce-theme.profile')" class="w-full inline-flex items-center group">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" />
            </svg>
            <span class="text-gray-700 font-semibold group-hover:text-primary transition-colors text-sm pl-2">Akun Saya</span>
          </Link>
          <Link :href="route('badaso.commerce-theme.order')" class="w-full inline-flex items-center group">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 text-gray-500" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-3 7h3m-3 4h3m-6-4h.01M9 16h.01" />
            </svg>
            <span class="text-gray-700 font-semibold cursor-pointer group-hover:text-primary transition-colors text-sm pl-2">Pesanan Saya</span>
          </Link>
          <Link :href="route('badaso.commerce-theme.notification')" class="w-full inline-flex items-center group text-primary">
            <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor">
              <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9" />
            </svg>
            <span class="font-semibold cursor-pointer transition-colors text-sm pl-2">Notifikasi</span>
          </Link>
        </div>
      </div>
      <div class="bg-white shadow-sm rounded-xl flex flex-wrap">
        <div class="flex flex-col w-full px-6 justify-center items-end border-b">
          <div class="text-sm text-gray-400 cursor-pointer py-2" @click="readAll">Tandai sebagai sudah dibaca</div>
        </div>
        <div class="flex flex-wrap w-full">
          <template v-if="notifications.data.length > 0">
            <div class="w-full flex p-6 space-x-4 items-start last:rounded-b-xl hover:bg-gray-50 cursor-pointer" :class="[notification.isRead == 0 ? 'bg-primary bg-opacity-10' : 'bg-white']" @click="notificationDetail(notification)" v-for="notification, index in notifications.data" :key="index">
              <div class="w-1/12">
                <img :src="logoTheme" class="w-full h-auto" alt="">
              </div>
              <div class="flex flex-col w-6/12">
                <div class="text-gray-700 mb-2">{{ notification.title }}</div>
                <div class="text-sm text-gray-400">{{ notification.content }}</div>
                <div class="text-sm text-gray-400 mt-2">{{ $moment(notification.createdAt).format('DD-MM-YYYY H:mm:ss') }}</div>
              </div>
              <div class="w-5/12">
                <button 
                  class="py-2 px-8 float-right bg-white hover:border-primary hover:text-primary text-xs border rounded-md" 
                  v-if="notification.type === 'orderNotification'"
                >Tampilkan Rincian Pesanan</button>
              </div>
            </div>
          </template>
          <template v-else>
            <div class="w-full h-full flex-col flex p-6 items-center justify-center text-gray-500">
              <svg xmlns="http://www.w3.org/2000/svg" class="h-16 w-16" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="1" d="M6 18L18 6M6 6l12 12" />
              </svg>
              <span>There is no notification</span>
            </div>
          </template>
        </div>
      </div>
    </div>

    <div class="sm:hidden">
      <div class="flex flex-col w-full divide-y">
        <div class="flex justify-between px-3 py-2 bg-white">
          <span class="text-sm font-medium text-gray-700">Notification</span>
          <div class="text-sm font-medium text-primary" @click="readAll">Baca Semua</div>
        </div>
        <div class="w-full flex gap-2 p-3" :class="[notification.isRead == 0 ? 'bg-primary bg-opacity-10' : 'bg-white']" v-for="notification, index in notifications.data" :key="index" @click="notificationDetail(notification)">
          <div class="w-1/5">
            <img :src="logoTheme" class="w-full h-auto object-contain object-center" alt="">
          </div>
          <div class="w-4/5 gap-2">
            <div class="text-gray-700 mb-1">{{ notification.title }}</div>
            <div class="text-sm text-gray-400">{{ notification.content }}</div>
            <div class="text-xs text-gray-400 mt-1">{{ $moment(notification.createdAt).format('DD-MM-YYYY H:mm:ss') }}</div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'
import appLayout from '../../layouts/app.vue'
import profileLayout from '../../layouts/profile.vue'
import { Link, Head } from '@inertiajs/inertia-vue'

export default {
  layout: [appLayout, profileLayout],
  components: {
    Link,
    Head,
  },
  data() {
    return {
      notifications: {
        data: []
      }
    }
  },
  computed: {
    ...mapState({
      isAuthenticated(state) {
        return state.isAuthenticated
      },
      user(state) {
        return state.user
      },
      logoTheme(state) {
        return this.$_.find(state.themeConfigurations, { key: "logoTheme" }).value
      },
    }),
  },
  mounted() {
    if (!this.isAuthenticated) {
      this.$inertia.visit(this.route('badaso.commerce-theme.login'))
    }

    this.fetchNotifications()
  },
  methods: {
    fetchNotifications() {
      this.$api.badasoNotification
        .browse()
        .then(res => {
          this.notifications = res.data.notifications
        })
        .catch(err => {
          this.$helper.displayErrors(err)
        })
    },
    getOrderId(message) {
      if (message.split("#").length < 2) return null
      return message.split("#")[1].split(" ")[0]
    },
    notificationDetail(notification) {
      if (notification.isRead === 0) {
        this.$api.badasoNotification
          .read({
            id: notification.id
          })
          .then(res => {
            this.$inertia.visit(this.route('badaso.commerce-theme.order-detail', this.getOrderId(notification.content)))
          })
          .catch(err => {
            this.$helper.displayErrors(err)
          })
      } else {
        this.$inertia.visit(this.route('badaso.commerce-theme.order-detail', this.getOrderId(notification.content)))
      }
    },
    readAll() {
      this.$api.badasoNotification
        .readAll()
        .then(res => {
          this.fetchNotifications()
        })
        .catch(err => {
          this.$helper.displayErrors(err)
        })
    }
  }
}
</script>
