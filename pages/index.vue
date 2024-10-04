<template>
  <div class="container-fluid d-flex justify-content-center align-items.center" style="background-color: aliceblue;">
    <div class="row">
    <div class="col-2 p-2 m-5">
      <div class="card shadow p-3 rounded-4">
        <h2 class="text-center">Login</h2>
        <form @submit.prevent="login">
          <div class="mb-3">
            <label>email</label>
          <input v-model="email" type="text" class="rounded-3" >
          </div>
          <div class="mb-3">
            <label>password</label>
          <input v-model="password" type="password" class="rounded-3"><br>
          </div>
          <div class="text-center m-3">
          <button type="submit" class="m-1 col-7 rounded-3">login</button>
        </div>
      </form>
      
    </div>
  </div>
  </div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient()
const email = ref('')
const password = ref('')

async function login() {
  const { data, error } = await supabase.auth.signInWithPassword({
    email: email.value,
    password: password.value,
  })
  if(data) navigateTo('/siswa')
}
</script>


<style scoped>
.container-fluid{
  height: 100vh;
}

.card{
  height: 300px;
  width: 300px;
  background-color: #6EAF72;
}
</style>
