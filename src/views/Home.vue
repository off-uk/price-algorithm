<template>
  <div class="home">
    <PriceAlgorithm/>
  </div>
</template>

<script>
// @ is an alias to /src
import PriceAlgorithm from '@/components/PriceAlgorithm.vue'

export default {
  name: 'Home',
  components: {
    PriceAlgorithm,
  }
}
</script>
