<template>
  <div>
    <b-nav tabs>
      <b-nav-item disabled>Fancy Quiz App</b-nav-item>
      <b-nav-item disabled>Counter: {{correct}} / {{total}} </b-nav-item>
    </b-nav>
  </div>
</template>

<script>
  export default {
    props:[
      'correct',
      'total'
    ]
  }
</script>