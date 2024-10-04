<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-6 offset-md-3">
        <form @submit.prevent="kirimData">
          <div class="mb-3">
            <select v-model="schedule" class="form-control form-control-lg form-select rounded-3"
              @change="getStudentsBySchedule">
              <option disabled value="">Cari Hari</option>
              <option v-for="(schedule, i) in schedules" :key="i" :value="schedule.id">{{ schedule.hari }}</option>
            </select>
          </div>

          <div class="mb-3">
            <select v-model="schedule" class="form-control form-control-lg form-select rounded-3"
              @change="getStudentsBySchedule">
              <option disabled value="">Tugas</option>
              <option v-for="(schedule, i) in schedules" :key="i" :value="schedule.id">{{ schedule.nama }}</option>
            </select>
          </div>

          <div class="mb-3" v-if="students.length">
            <div class="row">
              <div class="col">Nama</div>
              <div class="col">Tugas</div>
              <div class="col">Melaksanakan</div>
            </div>
            <div v-for="(item, index) in form" class="row">
              <div class="col">{{ getStudentName(item.siswa) }}</div>
              <div class="col">{{ getStudentDuty(item.nama) }}</div>
              <div class="col">
                <span class="me-3">
                  <input type="radio" :id="'ya' + index" :value="true" v-model="item.melaksanakan" />
                  <label class="ms-1" for="ya">Ya</label>
                </span>
                <span>
                  <input type="radio" :id="'tidak' + index" :value="false" v-model="item.melaksanakan" />
                  <label class="ms-1" for="tidak">Tidak</label>
                </span>
              </div>
            </div>
          </div>
        </form>
        <NuxtLink to="/siswa">
          <div class="vstack gap-2 col-md-5 mx-auto">
  <button type="button" class="btn btn-success">Kirim</button>
  <button type="button" class="btn btn-outline-success">Kembali</button>
</div>
        </NuxtLink>
      </div>
    </div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient()

const schedules = ref([])
const students = ref([])
const schedule = ref('')

const form = ref([{
  siswa: null,
  melaksanakan: null
}])
const kirimData = async () => {
  const { error } = await supabase.from('Piket').insert(form.value)
  if (error) throw error
  else {
    navigateTo('/Riwayat')
  }
}
const getStudentsBySchedule = async () => {
  const { data, error } = await supabase.from(`Siswa`).select(`*, tugas_piket(nama)`).eq('jadwal_piket', schedule.value)
  if (error) throw error
  if (data) {
    students.value = data
    form.value = data.map(data => ({
      siswa: data.id,
      melaksanakan: null
    }))
  }
}
const getSchedules = async () => {
  const { data, error } = await supabase.from('jadwal_piket').select().order('id')
  if (data) schedules.value = data
}
const getStudentName = (id) => {
  const student = students.value.find(s => s.id === id);
  return student ? student.nama : '';
};
const getStudentDuty = (id) => {
  const student = students.value.find(s => s.id === id);
  return student ? student.tugas_piket?.nama : '';
};

const setDate = () => {
  const today = new Date().toISOString().split('T')(0);
  form.value.tanggal = today;
}
onMounted(() => {
  getSchedules()
})

</script>