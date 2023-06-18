<template>
  <div class="layout">
    <header class="shadow-sm transparent-header">
      <nav class="container mx-auto p-4 flex justify-end">
        <ul class="flex gap-4">
          <li><NuxtLink to="/">Home</NuxtLink></li>
          <!-- <li class="nuxtLogged"><NuxtLink to="/feed">Feed</NuxtLink></li> -->
          <!-- <li class="nuxtLogged"><NuxtLink to="/logout">Log out</NuxtLink></li> -->
          <li class="nuxtDefault"><NuxtLink to="/login">Login</NuxtLink></li>
          <li class="nuxtDefault"><NuxtLink to="/signup">Sign Up</NuxtLink></li>
        </ul>
      </nav>
    </header>

    <!-- <div class="container mx-auto p-4"> -->
      <slot />
    <!-- </div> -->
  </div>
</template>

<style>
.layout {
  background: transparent;
}

.transparent-header {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  background: transparent;
  z-index: 999;
}
</style>

<!-- --------------------------------- -->
<!-- <template>
    <div class="layout">
      <header class="shadow-sm">
        <nav class="container mx-auto p-4 flex justify-end">
          <ul class="flex gap-4">
            <li><NuxtLink to="/">Home</NuxtLink></li>
            <li class="nuxtDefault"><NuxtLink to="/login">Login</NuxtLink></li>
            <li class="nuxtDefault"><NuxtLink to="/signup">Sign Up</NuxtLink></li>
          </ul>
        </nav>
      </header>
    </div>
  
    <div class="container mx-auto p-4 mainbody">
      <slot />
    </div>
</template>
<style>
.layout{
  background: transparent;
}
.mainbody{
  width: 100%;
  height: 300px;
  background-image: url('~/assets/Home_Banner.jpg');
  background-size: cover;
  background-position: center;
}
</style> -->
<!-- ------------------------------------------------------------ -->
<!-- <script>
  export default {
    data() {
      return {
        tok: '',
      }
    },
    mounted() {
      this.tok = localStorage.getItem('name')
      const defaultLinks = document.getElementsByClassName('nuxtDefault')
      const loggedLinks = document.getElementsByClassName('nuxtLogged')
      if (this.tok) {
        for (let i = 0; i < defaultLinks.length; i++) {
          defaultLinks[i].style.display = 'none'
        }
        for (let i = 0; i < loggedLinks.length; i++) {
          loggedLinks[i].style.display = 'flex'
        }
      } else {
        for (let i = 0; i < defaultLinks.length; i++) {
          defaultLinks[i].style.display = 'flex'
        }
        for (let i = 0; i < loggedLinks.length; i++) {
          loggedLinks[i].style.display = 'none'
        }
      }
    },
   }
</script> -->
  