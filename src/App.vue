<template>
  <v-app>
    <v-content>
      <BookFinder/>
    </v-content>
  </v-app>
</template>

<script>
import BookFinder from './components/BookFinder'

export default {
  name: 'App',
  components: {
    BookFinder
  },
  data () {
    return {
      //
    }
  }
}
</script>
