# Create Nuxt app
`npx create-nuxt-app <proj name>`

# API: https://developers.themoviedb.org/3/getting-started/introduction

# Nota bene
- When changing `nuxt.config.js`, you need to reset the dev browser (also in Yarn?)


# SASS
- install : `npm install --save-dev sass sass-loader@10 fibers` MERGE DOAR PE YARN
- configure: 
    - create 
        '@/assets/css/main.css',
        '@/assets/css/main.scss'
    - add them to the nuxt instalation

# App Transitions
// app transitions
.page-enter-active,
.page-leave-active {
  transition: opacity 0.5s;
}
.page-enter,
.page-leave-to {
  opacity: 0;
}


# Vue vs Nuxy
- In Vue the `App.vue` e ca un `base.twig.html` of Vue.JS; Poti specifica componente care vor fi incluse in toate celelalte
- In NUXT: 
    - this OG file is found at `.nuxt\layouts\default.vue`
    - but, to modify it, we need to create our own `@/layouts` folder


# Data Fetching
- In Vue, fetching data is activated by calling the function in side a lifecycle hook (mounted, created etc)
- In Nuxt, we use the `fetch` custom app hook


- `$fetchState` param gives acces to:
  - `pending` bool
  - `error`
  - `timestamp` of the last fetch


# API w/ images 
- building imgs required 3 pieces of data
  - `base_url`
  - `size`
  - `file_path`


# Input binding modifiers
  By default, v-model syncs the input with the data after each input event (with the exception of IME composition, as stated above). You can add the lazy modifier to instead sync after change events:

<!-- synced after "change" instead of "input" -->
<input v-model.lazy="msg">

# Callin FETCH inside <template>
<input @keyup.enter="$fetch" />
async fetch() {
    if (this.searchInput === '') {
      await this.getMovies()
    }
    if (this.searchInput !== '') {
      await this.searchMovies()
    }
  },


# Focus on load
- need to be called in `mounted() {}` hook


# LOADING COMPONENT
-> using the `$fetchState.pending` to show the `<Loading />` component while waiting for the `data` to charge
  - `v-if` on the `<Loading />`
  - `v-else` on the _CONTENT_

- Use `fetchDelay` to avoid "too quick" flashing loads; 
  - add it as an option inside `export default {}` obj


# Keep-alive DIRECTIVE
  ## Caching
  INSIDE `layout`

  You can use keep-alive directive in <nuxt/> and <nuxt-child/> component to save fetch calls on pages you already visited:
  layouts/default.vue

  <template>
    <nuxt keep-alive />
  </template>


# (PAGE TRANSITIONS)[https://nuxtjs.org/docs/features/transitions/#global-settings]
- add transitions in your global .scss


# SEO OPTIMISATION
- Nuxt *Head() method*

- add a dynamic Title in inside [movies/_id.vue]

# Deploy with HEROKU
- deploy from GitHub repo
- deploy w/ Heroku CLI



*=====================================================*
*EXTRAS*
# Change css and Bootstrap-ize the project

# Add Color Switcher

# Move NuxtLink directly on the movie posters (index.vue)

# 

