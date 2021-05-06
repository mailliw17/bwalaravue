<template>
  <div class="home">
    <Header />
    <Hero/>
    <Women/>
    <Insta/>
    <PartnerLogo/>
    <Footer/>
  </div>
</template>

<script>
// @ is an alias to /src
import Header from '@/components/Header.vue'
import Hero from '@/components/Hero.vue'
import Women from '@/components/Women.vue'
import Insta from '@/components/Insta.vue'
import PartnerLogo from '@/components/PartnerLogo.vue'
import Footer from '@/components/Footer.vue'

export default {
  name: 'Home',
  components: {
    Header,
    Hero,
    Women,
    Insta,
    PartnerLogo,
    Footer
  }
}
</script>
