<template>
    <q-page padding>
      <div class="q-mb-xl">
          <q-carousel
      animated
      v-model="slide"
      navigation
      infinite
      autoplay
      swipeable
      arrows
      transition-prev="slide-right"
      transition-next="slide-left"
      @mouseenter="autoplay = false"
      @mouseleave="autoplay = true"
    >
      <q-carousel-slide :name="1" img-src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRJ7dHHjCo1PighhuaVQKg3hX4DMYDhpUNTjw&usqp=CAU" />
      <q-carousel-slide :name="2" img-src="https://s.kaskus.id/r540x540/images/2019/06/25/10507992_20190625112402.jpg" />
      <q-carousel-slide :name="3" img-src="https://www.lantaifutsal.com/wp-content/uploads/2019/10/Lapangan-futsal-interlock-1.jpg" />
      <q-carousel-slide :name="4" img-src="https://thumbs.dreamstime.com/b/empty-badminton-court-competing-32605004.jpg" />
    </q-carousel>
      </div>
      <div class="row q-col-gutter-md">
        <div class="col-md-3 col-xs-12" v-for="(lapangan, i) in data" :key="i">
              <q-card>
                   <q-img
                    :src="`${$baseImageURL}/${lapangan.image}`"
                    :ratio = "1"
                    />

                    <q-card-section>
                      <q-btn
                        fab
                        color="blue-7"
                        icon="place"
                        class="absolute"
                        style="top: 0; right: 15px; transform: translateY(-50%);"
                      />
                        <div class="row no-wrap items-center">
                          <div class="col text-h6 ellipsis">
                              {{ lapangan.lapangan }}
                          </div>
                          <div class="col-auto text-blue text-caption q-pt-md row no-wrap items-center">
                              {{ lapangan.lokasi }}
                          </div>
                        </div>

                        <q-rating v-model="lapangan.rating" readonly color="orange-5" :max="5" size="32px" />
                    </q-card-section>

                    <q-card-section class="q-pt-none">
                      <div class="text-subtitle1">
                         Rp.{{ lapangan.harga }},-
                        </div>
                        <div class="text-subtitle1">
                        {{ lapangan.alas }}
                        </div>
                        <div @click="lapangan.expanded = !lapangan.expanded" class="text-caption text-grey-9 cursor-pointer">
                          Tampilkan Deskripsi
                        </div>
                        <q-slide-transition>
                        <div v-show="lapangan.expanded">
                          <div class="text-grey-9 text-caption">
                            {{ lapangan.deskripsi }}
                          </div>
                        </div>
                      </q-slide-transition>
                    </q-card-section>

                    <q-card-actions>
                        <q-btn unelevated @click="openDetail(lapangan)" class="full-width" color="primary">
                        Order Now
                        </q-btn>
                    </q-card-actions>
                    </q-card>
        </div>
      </div>
      <q-dialog v-model="dialog" v-if="dialog" position="bottom">
      <q-card style="width: 500px">
        <q-card-section>
          <div class="text-h6">Detail Pemesanan</div>
        </q-card-section>
        <q-card-section style="max-height: 50vh;" class="scroll">
          {{ activeData.lapangan }} -- Rp.{{ activeData.harga }},-
          <q-form class="q-mt-md">
            <q-input filled type="number" class="q-mb-md" v-model="durasi" label="Masukan Durasi Sewa (Perjam)"/>
            Total : Rp.{{total}},-
            <q-select
            class="q-mt-md"
            filled
            v-model="mulai"
            :options="optionMulai"
            label="Pilih Jam Mulai Sewa"
            :rules="[ val => val && val.length > 0 || 'Mohon di Isi']"
            />
            <q-select
            class="q-mt-md"
            filled
            v-model="selesai"
            :options="optionSelesai"
            label="Pilih Jam Selesai Sewa"
            :rules="[ val => val && val.length > 0 || 'Mohon di Isi']"
            />
             <q-input filled type="number" class="q-mb-md" v-model="noTelp" label="Masukan Nomor Telepon/HP" :rules="[ val => val && val.length > 0 || 'Mohon di Isi']"/>
            <q-file color="teal" class="q-mt-md" filled v-model="image" label="Kartu Identitas">
            <template v-slot:prepend>
              <q-icon name="cloud_upload" />
            </template>
            </q-file>
          </q-form>
        </q-card-section>
        <q-card-actions align="right">
          <q-btn flat label="Batal" clickable v-close-popup/>
          <q-btn color="primary" @click="prosesTransaksi()" label="Proses"/>
        </q-card-actions>
      </q-card>
    </q-dialog>
    </q-page>
</template>
<script>
export default {
  data () {
    return {
      slide: 1,
      stars: 4,
      dialog: false,
      image: null,
      activeData: null,
      durasi: 1,
      noTelp: null,
      data: [],
      mulai: null,
      selesai: null,
      optionMulai: [
        '08:00',
        '08:30',
        '09:00',
        '09:30',
        '10:00',
        '10:30',
        '12:30',
        '13:00',
        '13:30',
        '14:00',
        '14:30',
        '15:00',
        '15:30',
        '16:30',
        '17:00',
        '17:30',
        '18:00',
        '18:30',
        '19:00',
        '19:30',
        '20:00',
        '20:30',
        '21:00',
        '21:30',
        '22:00',
        '22:30',
        '23:00'
      ],
      optionSelesai: [
        '09:00',
        '09:30',
        '10:00',
        '10:30',
        '11:00',
        '11:30',
        '13:30',
        '14:00',
        '14:30',
        '15:00',
        '15:30',
        '16:00',
        '17:00',
        '17:30',
        '18:00',
        '18:30',
        '19:00',
        '19:30',
        '20:00',
        '20:30',
        '21:00',
        '21:30',
        '22:00',
        '22:30',
        '23:00',
        '23:30',
        '00:00',
        '01:00'
      ]
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      this.$axios.get('lapangan/getall')
        .then(res => {
          if (res.data.sukses) {
            this.data = res.data.data.map(lapangan => {
              return Object.assign(lapangan, {
                expanded: false
              })
            })
          } else {
            this.$showNotif(res.data.pesan, 'negative')
          }
        })
    },
    openDetail (data) {
      this.activeData = data
      this.dialog = true
    },
    prosesTransaksi () {
      const formData = new FormData()
      formData.append('image', this.image)
      formData.append('data', JSON.stringify({
        idUser: this.$q.localStorage.getItem('dataUser')._id,
        idLapangan: this.activeData._id,
        harga: this.activeData.harga,
        durasi: this.durasi,
        total: this.total,
        mulai: this.mulai,
        selesai: this.selesai,
        noTelp: this.noTelp

      }))
      this.$axios.post('/sewa/insert', formData)
        .then(res => {
          if (res.data.sukses) {
            this.$showNotif(res.data.pesan, 'positive')
            this.dialog = false
            this.image = null
          } else {
            this.$showNotif(res.data.pesan, 'negative')
          }
        })
    }
  },
  computed: {
    total () {
      return this.activeData.harga * this.durasi
    }
  }
}
</script>
