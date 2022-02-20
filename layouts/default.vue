<template>
  <div>
    <Header />
    <Nuxt />
    <Footer />
  </div>
</template>

<script>
import Header from "~/components/header/header.vue"
import Footer from "~/components/footer/footer.vue"
export default {
    components: {Header, Footer}
}
</script>