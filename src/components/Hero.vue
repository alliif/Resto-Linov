<template>
  <div class="row mt-5">
      <div class="col">
          <div class="d-flex h-100">
              <div class="justify-content-center align-self-center">
                  <h2><strong>Delicious Food Menu, </strong><br>in Your Gadget</h2>
                  <p>Ayo segera pilih dan pesan <br>makanan dan minuman favoritmu</p>
                  <router-link class="btn btn-success" to="/food">ORDER <b-icon-arrow-right></b-icon-arrow-right></router-link>
              </div>
          </div>
      </div>
      <div class="col">
          <img src="../assets/img/Hero.png" width="100%" alt="Background">
      </div>
  </div>
</template>

<script>
export default {
    name: 'Hero'
}
</script>

<style>

</style>