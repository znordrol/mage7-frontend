<template>
  <b-navbar class="navbar">
    <b-navbar-brand class="text-white" style="width: 350px;">
      <img
        class="logo"
        src="/img/mage-white.png"
        style="height:30px; width:30px;margin-left:0px; margin-top:0px;"
      />
    </b-navbar-brand>

    <b-container @click="performToggle()">
      <i class="fas fa-bars text-white"></i>
    </b-container>
    <b-collapse id="nav-text-collapse" is-nav> </b-collapse>
    <!--a class="text-white" href="" @click="logout()">
      <i class="fas fa-arrow-left mr-2"></i>
      Keluar
    </a-->
  </b-navbar>
</template>

<script>
export default {
  name: 'Header',
  data() {
    return {
      textBold: 'text-bold',
      location: '',
    };
  },
  computed: {
    sidebar() {
      return this.$store.state.ui.sidebar;
    },
  },
  methods: {
    performToggle() {
      this.$store.dispatch('ui/changeSidebarComponent');
    },
    setLocation() {
      if (window.location.href.includes('session')) this.location = 'Sesi';
      else if (window.location.href.includes('participant')) { this.location = 'Peserta'; } else if (window.location.href.includes('candidate')) { this.location = 'Kandidat'; } else if (window.location.href.includes('setting')) { this.location = 'Pengaturan'; } else this.location = 'Beranda';
    },
    logout() {
      this.$store.commit('setAuthentication', false);
      this.$router.push('/');
    },
  },
  watch: {
    $route() {
      this.setLocation();
    },
  },
  created() {
    this.setLocation();
  },
};
</script>
<style scoped>
.navbar {
  background-color: #111111;
  width: calc(100%);
  height: 70px;
  position: fixed;
  top: 0;
  left: 0;
  z-index: 999999999999999;
}
.text-bold {
  font-weight: bold;
}
.text-normal {
  font-weight: normal;
}
.inline {
  display: inline;
}
.sticky-header {
  position: fixed;
  z-index: 5;
}
</style>
