<template>
  <q-layout view="lHh LpR fFf">
      <q-toolbar unelevated style= "height: 70px;" class="bg-dark text-white">
      <div class="text-h5 q-ml-lg">Penyewaan Lapangan</div>

      <q-space />
      <q-btn flat>
      <q-item-section>
        <q-icon name="more_vert"/>
      </q-item-section>
      <q-menu>
          <q-list style="min-width: 200px">
            <q-separator />
            <q-item clickable v-close-popup active-class="active" v-ripple exact :to="{ name :'homeUser'}">
              <q-item-section>
                <q-item-label>Home</q-item-label>
              </q-item-section>
            </q-item>
            <q-separator />
            <q-item clickable v-close-popup active-class="active" v-ripple exact :to="{ name :'mySewa'}">
              <q-item-section>
                <q-item-label>Transaksi Saya</q-item-label>
              </q-item-section>
            </q-item>
            <q-separator />
            <q-item clickable v-close-popup active-class="active" v-ripple>
              <q-item-section>
                <q-item-label>Alamat</q-item-label>
              </q-item-section>
            </q-item>
            <q-separator />
            <q-item clickable v-close-popup @click="logout()">
              <q-item-section>Logout</q-item-section>
            </q-item>
          </q-list>
        </q-menu>
       </q-btn>
    </q-toolbar>

    <q-page-container class="bg-grey-2">
      <router-view />
    </q-page-container>
  </q-layout>
</template>
<script>
export default {
  data () {
    return {
      drawer: false
    }
  },
  methods: {
    logout () {
      this.$q.localStorage.clear()
      this.$router.push({ name: 'loginPage' })
    }
  }
}
</script>
