<template>
    <q-page padding>
        <div class="row q-mb-md col-gutter-md">
      <div class="col-md-12 col-xs-12 col-lg-12">
        <div class="row">
          <div class="col-auto">
            <div class="left blue"></div>
          </div>
          <div class="col">
            <q-banner inline-actions class="text-white bg-grey-7">
              <div class="text-h6">Input Data</div>
              <div>Input Data Lapangan</div>
            </q-banner>
          </div>
        </div>
      </div>
    </div>
    <q-card flat>
      <q-card-section class="row">
          <q-form
            @submit="onSubmit()"
            @reset="onReset()"
        class="q-gutter-md col-md-6 col-xs-12"
        >
        <q-input
            filled
            v-model="form.lapangan"
            label="Lapangan"
            :rules="[ val => val && val.length > 0 || 'Mohon Isi  Nama Lapangan']"
        />

        <q-input
            filled
            v-model="form.harga"
            type="number"
            label="Harga perJam"
            :rules="[ val =>  val > 0 || 'Mohon Isi Harga']"
        />

        <q-input
            filled
            v-model="form.lokasi"
            label="Lokasi"
            :rules="[ val => val && val.length > 0 || 'Mohon Isi Lokasi']"
        />

        <q-select
        filled
        v-model="form.alas"
        :options="alasLapangan"
        label="Plilh Jenis lantai"
        />

        <div class="flex">
          Pilih Rating
            <q-rating
            v-model="form.rating"
            size="2em"
            :max="5"
            class="q-ml-md"
            color="primary"
          />
        </div>
            <q-input
            v-model="form.deskripsi"
            filled
            type="textarea"
            label="Deskripsi"
          />

            <q-file accept=".jpg, image/*" color="teal" filled v-model="image" label="Upload Gambar">
            <template v-slot:prepend>
            <q-icon name="cloud_upload" />
            </template>
        </q-file>

        <div>
            <q-btn label="Submit" type="submit" color="primary"/>
            <q-btn label="Reset" type="reset" color="primary" flat class="q-ml-sm" />
        </div>
        </q-form>
      </q-card-section>
    </q-card>

    </q-page>
</template>
<script>
export default {
  data () {
    return {
      form: {
        lapangan: null,
        harga: 0,
        lokasi: null,
        alas: null,
        rating: 0,
        deskripsi: null
      },
      alasLapangan: [
        'Rumput Sintetis',
        'Lantai Vinyl'
      ],
      image: null
    }
  },
  methods: {
    onSubmit () {
      const formData = new FormData()
      formData.append('image', this.image)
      formData.append('data', JSON.stringify(this.form))
      this.$axios.post('/lapangan/insert', formData)
        .then(res => {
          if (res.data.sukses) {
            this.$showNotif(res.data.pesan, 'positive')
            this.$router.push({ name: 'DataLapangan' })
          } else {
            this.$showNotif(res.data.pesan, 'negative')
          }
        })
    },
    onReset () {
      this.form = {
        judulFilm: null,
        harga: 0,
        tahun: null,
        genre: null,
        rating: 0,
        deskripsi: null
      }
      this.alasLapangan = [
        'Rumput Sintetis',
        'Lantai Vinyl']
      this.image = null
    }
  }
}
</script>
<style scoped>
.left{
  width : 5px;
  height: 100%;
  background-color: black;
}
</style>
