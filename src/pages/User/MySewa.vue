<template>
  <q-page padding>
    <div class="row q-mb-md col-gutter-md">
      <div class="col-md-12 col-xs-12 col-lg-12">
        <div class="row">
          <div class="col-auto">
            <div class="left blue"></div>
          </div>
          <div class="col">
            <q-banner inline-actions class="text-white bg-red">
              <div class="text-h6">Data Transaksi</div>
              <div>Data Transaksi Anda</div>
            </q-banner>
          </div>
        </div>
      </div>
    </div>
    <q-card flat>
          <q-table
            :data="data"
            flat
            :columns="columns"
            row-key="name"
          >
            <template v-slot:body="props">
              <q-tr :props="props">
                <q-td key="lapangan" :props="props">
                  {{ props.row.dataLapangan[0].lapangan }}
                </q-td>
                <q-td key="lapangan" :props="props">
                  {{ props.row.dataLapangan[0].lokasi }}
                </q-td>
                <q-td key="harga" :props="props">
                  {{ props.row.harga }}
                </q-td>
                <q-td key="durasi" :props="props">
                  {{ props.row.durasi }}
                </q-td>
                <q-td key="total" :props="props">
                  {{ props.row.total }}
                </q-td>
                <q-td key="mulai" :props="props">
                  {{ props.row.mulai }}
                </q-td>
                <q-td key="selesai" :props="props">
                  {{ props.row.selesai }}
                </q-td>
                <q-td key="status" :props="props">
                  <q-badge v-if="props.row.status === 1" color="warning" class="q-pa-sm">
                    Belum Dikonfirmasi
                  </q-badge>
                  <q-badge v-else-if="props.row.status === 2" color="blue" class="q-pa-sm">
                    Sudah DiKonfirmasi (Belum Membayar)
                  </q-badge>
                  <q-badge v-else color="green" class="q-pa-sm">
                    Sudah Membayar
                  </q-badge>
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
        { name: 'lokasi', align: 'left', label: 'Lokasi', field: 'lokasi', sortable: true },
        { name: 'harga', align: 'left', label: 'Harga', field: 'harga', sortable: true },
        { name: 'durasi', align: 'left', label: 'Durasi Sewa', field: 'durasi', sortable: true },
        { name: 'total', align: 'left', label: 'Total Harga', field: 'total', sortable: true },
        { name: 'mulai', align: 'left', label: 'Mulai', field: 'mulai', sortable: true },
        { name: 'selesai', align: 'left', label: 'Selesai', field: 'selesai', sortable: true },
        { name: 'status', align: 'left', label: 'Status', field: 'status', sortable: true }
      ],
      data: [],
      detail: false,
      activeData: null
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      this.$axios.get(`sewa/getsewabyUser/${this.$q.localStorage.getItem('dataUser')._id}`)
        .then((res) => {
          if (res.data.sukses) {
            this.data = res.data.data
          }
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
