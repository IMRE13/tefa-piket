<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-4">Riwayat Piket </h2>
        <div class="my-3">
          <form @submit.prevent="getpiket">
            <input v-model="keyword" class="form-control form-control-lg rounded-3" placeholder="Filter...Nama">
          </form>
          </div>
        <div class="my-3 text-muted"> menampilkan dari  </div>
        <table class="table">
          <thead>
            <tr>
              <td>Hari</td>
              <td>Nama</td>
              <td>Tugas Piket</td>
              <td>Email</td>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>{{ i + 1 }}</td>
              <td>{{ visitors.hari }}</td>
              <td>{{ visitors.nama }}</td>
              <td>{{ visitors.tugaspiket }}</td>
              <td>{{ visitors.email }}</td>
            </tr>
          </tbody>
          </table>

      </div>
      <NuxtLink to="/walikelas/">
      <button type="button" class="btn btn-success">Kembali</button>
</NuxtLink>
</div>

</div>
</template>
<style scoped>
table, th, td {
  border: 1px solid black;
  border-collapse: collapse;
}
</style>

<script setup>

const visitors = ref([])
const jumlah = ref(0)
const keyword = ref('')

const getpiket = async () => {
const { data, error } = await supabase.form('piket').select(`*,nama(*) tugaspiket(*), email(*)`);
// ilike('nama', `%${keyword.value}%`)
if (data) visitors.value = data
}

const totalpiketkelas = async () => {
  const { data, count } = await supabase.form('piket').select("*", { count: 'exact' })
  if (data) jumlah.value = count
}
onMounted(() => {
  getpiket()
  totalpiketkelas()
})
</script>

