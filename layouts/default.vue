<template>
  <div>
    <h1 class="tag is-info is-large">Absteige Nuki Logs</h1>

    <section class="main-content columns">
      <div class="container column is-10">
        <nuxt keep-alive :keep-alive-props="{ max: 10 }" />
      </div>
    </section>
  </div>
</template>


