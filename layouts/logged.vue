<template>
    <div>
      <header class="shadow-sm" style="background-color:#B28C49;">
        <nav class="container mx-auto p-4 flex justify-end">
          <ul class="flex gap-4">
            <li><NuxtLink to="/profile">Profile</NuxtLink></li>
            <li class="nuxtLogged"><NuxtLink to="/feed">Feed</NuxtLink></li>
            <li class="nuxtLogged"><NuxtLink to="/logout">Log out</NuxtLink></li>
            <!-- <li class="nuxtDefault"><NuxtLink to="/login">Login</NuxtLink></li>
            <li class="nuxtDefault"><NuxtLink to="/signup">Sign Up</NuxtLink></li> -->
          </ul>
        </nav>
      </header>
    </div>
  
      <slot />
</template>
<!--   
<script>
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