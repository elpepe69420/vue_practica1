<template>
  <section class="hero">
    <div class="container">
      <h2>Informacion de usuarios</h2>
      <p>Control de usuario, agregado, editado y eliminacion.</p>
      <a href="/users" class="btn">Empezar</a>
    </div>
  </section>

  <section id="beneficios" class="beneficios">
    <div class="container">
      <h3>¿Que puede hacer?</h3>
      <ul>
        <li>✔️ Agregar usuario</li>
        <li>✔️ Editar usuario</li>
        <li>✔️ Eliminar usuario</li>
      </ul>
    </div>
  </section>
</template>

<script>
// @ is an alias to /src

export default {
  name: "HomeView",
  components: {},
};
</script>

<style>
.hero {
  background: #eee;
  text-align: center;
  padding: 4rem 1rem;
}

.hero h2 {
  font-size: 2rem;
  margin-bottom: 1rem;
}

.hero .btn {
  display: inline-block;
  background: #000;
  color: #fff;
  padding: 0.8rem 1.5rem;
  text-decoration: none;
  margin-top: 1rem;
  border-radius: 5px;
}

.beneficios ul {
  list-style: none;
  padding: 1rem 0;
}

.beneficios li {
  margin-bottom: 0.5rem;
}
</style>
