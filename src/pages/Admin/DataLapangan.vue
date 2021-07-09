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
              <div class="text-h6">Data Lapangan</div>
              <div>Data Katalog Lapangan</div>
            </q-banner>
          </div>
        </div>
      </div>
    </div>
    <q-card flat>
          <q-table
            :data="data"
            :columns="columns"
            row-key="name"
          >
            <template v-slot:body="props">
              <q-tr :props="props">
                <q-td key="lapangan" :props="props">
                  {{ props.row.lapangan }}
                </q-td>
                <q-td key="harga" :props="props">
                  Rp{{ props.row.harga }},-
                </q-td>
                <q-td key="lokasi" :props="props">
                  {{ props.row.lokasi }}
                </q-td>
                <q-td key="alas" :props="props">
                  {{ props.row.alas }}
                </q-td>
                <q-td key="deskripsi" :props="props">
                  <div class="ellipsis" style="max-width: 300px">
                    {{ props.row.deskripsi }}
                  </div>
                </q-td>
                <q-td key="gambar" :props="props">
                  <q-img
                    :src="`${$baseImageURL}/${props.row.image}`"
                    spinner-color="white"
                    />
                </q-td>
                <q-td key="aksi" :props="props">
                <div class="row q-gutter-md">
                <q-btn :to="{ name: 'formEditLapangan', params: { id: props.row._id }}" label="Edit" icon="edit" color="warning" unelevated/>
                <q-btn @click="deleteLapangan(props.row._id)" label="Hapus" icon="delete" color="negative" unelevated/>
                </div>
                </q-td>
              </q-tr>
            </template>
          </q-table>
    </q-card>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      columns: [
        { name: 'lapangan', align: 'left', label: 'Lapangan', field: 'lapangan', sortable: true },
        { name: 'harga', align: 'left', label: 'Harga PerJam', field: 'harga', sortable: true },
        { name: 'lokasi', align: 'left', label: 'Lokasi', field: 'lokasi', sortable: true },
        { name: 'alas', align: 'left', label: 'Jenis Lantai', field: 'alas', sortable: true },
        { name: 'deskripsi', align: 'left', label: 'Deskripsi', field: 'deskripsi', sortable: true },
        { name: 'gambar', align: 'left', label: 'Gambar', field: 'gambar' },
        { name: 'aksi', align: 'center', label: 'Aksi', field: 'gambar' }
      ],
      data: []
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      this.$axios.get('lapangan/getall')
        .then((res) => {
          if (res.data.sukses) {
            this.data = res.data.data
          } else {
            this.showNotif(res.data.pesan, 'negative')
          }
        })
    },
    deleteLapangan (id) {
      this.$q.dialog({
        title: 'Konfirmasi',
        message: 'Apakah Anda Yakin?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.$axios.delete(`lapangan/delete/${id}`)
          .then(res => {
            if (res.data.sukses) {
              this.$showNotif(res.data.pesan, 'positive')
              this.getData()
            } else {
              this.$showNotif(res.data.pesan, 'negative')
            }
          })
      })
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
