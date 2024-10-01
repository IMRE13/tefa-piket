<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-6 offset-md-3">
        <form @submit.prevent="kirimData">
          <div class="mb-3">
            <select v-model="form.nama" class="form-control form-control-lg form-select rounded-3">
              <option value="">Cari Nama</option>
              <option v-for="(item, i) in objectives" :key="i" :value="item.id">{{ item.nama }}</option>
            </select>
          </div>
          <div>
          <div class="mb-3">
            <input v-model="form.tugas" type="text" class="form-control form-control-lg" placeholder="Tugas piket">
          </div>
          <div></div>
            <button type="submit" class="btn btn-secondary btn-lg rounded-3 px-3">KIRIM</button>
          </div>
        </form>
      </div>
    </div>
  </div>
  <div class="row">
    <div class="col-lg-12 d-flex justify-content-center mt-5">
      <NuxtLink to="/siswa">
        <button type="submit" class="btn btn-success btn-lg rounded-5 px-5">kembali</button>
      </NuxtLink>
    </div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient()

const objectives = ref([])


const form = ref({
  nama: "",
  tugas: "",
})

const kirimData = async () => {
  const { error } = await supabase.from('Piket').insert([form.value])
  if (!error) {
    navigateTo('/Riwayat')
  }
  else {
    console.log(error)
  }
}
const getSiswa= async () => {
  const { data, error } = await supabase.from('Siswa').select('*')
  if (data) objectives.value = data
}

onMounted(() => {
  getSiswa()
})
</script>