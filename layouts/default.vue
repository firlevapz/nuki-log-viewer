<template>
  <div>
    <div class="columns pt-4 pl-2 ml-1 is-centered">
      <h1 class="title">Absteige Logs</h1>
    </div>
    <section class="main-content columns">
      <div class="container column is-10">
        <nuxt keep-alive :keep-alive-props="{ max: 10 }" />
      </div>
    </section>
  </div>
</template>


