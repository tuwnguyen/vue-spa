<template>
    <nav 
    class="navbar navbar-expand-lg"
    :class="computedClasses"
    >
      <div class="collapse navbar-collapse" id="navbarNav">
        <ul class="navbar-nav">
            <nav-bar-link
              v-for="(page, index) in pages" class="navItem" :key="index"
              :page="page"
              :index="index">
            </nav-bar-link>
            <li>
              <router-link
                :to="`/pages`" 
                class="nav-link"
                active-class="active"
                aria-current="page"
              >
                Pages
              </router-link>
          </li>
        </ul>
      </div>
      <form class="d-flex">
        <button
        class="btn btn-primary"
        @click.prevent="changeTheme()"
        >Toggle</button>
      </form>
    </nav>
</template>

<script setup>
  import { ref, computed, inject } from 'vue'
  import { useStore } from 'vuex'
  import NavBarLink from './NavBarLink.vue'

  const store = useStore()
  const $bus = inject('$bus')
  const theme = ref('dark')
  const pages = ref([])
  const pagesGetter = computed(() => store.getters['pages/getPublishedPages']).value
  pages.value = pagesGetter
  const computedClasses = computed(() => ({
    'navbar': true,
    'navbar-expand-lg': true,
    [`navbar-${theme.value}`]: true,
    [`bg-${theme.value}`]: true,
  }))

  getThemeSetting()

  $bus.$on('page-updated', () => {
    pages.value = store.getters['pages/getPublishedPages']
  })

  $bus.$on('page-created', () => {
    pages.value = store.getters['pages/getPublishedPages']
  })

  $bus.$on('page-deleted', () => {
    pages.value = store.getters['pages/getPublishedPages']
  })

  function changeTheme() {
    let _theme = 'dark'
    if(_theme === theme.value) {
      _theme = 'light'
    }
    theme.value = _theme
    storeThemeSetting()
  }

  function storeThemeSetting() {
    localStorage.setItem('theme', theme.value)
  }

  function getThemeSetting() {
    let _theme = localStorage.getItem('theme')
    if(_theme) {
      theme.value = _theme
    }
  }
</script>

<style>
  
</style>