<template>
  <b-navbar toggleable="xl" type="dark" variant="dark" id="adminNavbar">
    <b-navbar-brand>
      <b-link to="/" class="text-decoration-none mr-5">
        <b-img
          src="https://i.ibb.co/8K8jcMr/alex-logo-2.png"
          alt="Alex Rentals"
          width="110px"
        />
      </b-link>
    </b-navbar-brand>

    <b-navbar-toggle target="admin-collapse" />

    <b-collapse id="admin-collapse" is-nav>
      <div class="d-flex flex-wrap">
        <b-link to="/rents" class="text-decoration-none mr-5 mb-1">
          <h3 class="text-danger">
            Оренди
          </h3>
        </b-link>
        <b-link to="/cars" class="text-decoration-none mr-5">
          <h3 class="text-danger">
            Автомобілі
          </h3>
        </b-link>
        <b-link to="/models" class="text-decoration-none mr-5">
          <h3 class="text-danger">
            Моделі
          </h3>
        </b-link>
        <b-link to="/specifications" class="text-decoration-none mr-5">
          <h3 class="text-danger">
            Специфікації
          </h3>
        </b-link>
        <b-link to="/rent_centers" class="text-decoration-none mr-5">
          <h3 class="text-danger">
            Центри оренди
          </h3>
        </b-link>
        <b-link to="/classes" class="text-decoration-none mr-5">
          <h3 class="text-danger">
            Класи
          </h3>
        </b-link>
        <b-link to="/discounts" class="text-decoration-none mr-5">
          <h3 class="text-danger">
            Знижки
          </h3>
        </b-link>
        <b-link to="/clients" class="text-decoration-none mr-5">
          <h3 class="text-danger">
            Клієнти
          </h3>
        </b-link>
      </div>
    </b-collapse>
  </b-navbar>
</template>

<script>
export default {
  name: "AdminNavbar",
};
</script>

<style>
#adminNavbar {
  border-bottom-left-radius: 20px;
  border-bottom-right-radius: 20px;
}
</style>
