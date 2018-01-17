<template>
  <div>
   <todos v-model="todos"></todos>
  </div>
</template>

<script>
    // ici c'est le centre de l'application
    // on import les templates dont on a besoin
    import Todos from './components/Todos'
    export default {
      components: {
        Todos
      }
    }
</script>

