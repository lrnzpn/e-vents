<template>
  <Layout v-slot="{ searchText}">  <!-- pull out searchText from props -->
    <v-tabs v-model="tab" grow>
        <v-tab>All Events</v-tab>
        <v-tab>Live Music</v-tab>
        <v-tab>Coding Events</v-tab>
    </v-tabs>

    <v-row class="justify-space-around">
        <v-card
        width="280"
        v-for="edge in getEvents(searchText)" :key="edge.node.id"
        class="mt-5"
        >
            <v-img
            class="white--text align-end"
            height="200px"
            :src="`http://localhost:1337${edge.node.thumbnail}`"
            >
            
            </v-img>
            <v-card-title>{{edge.node.title}}</v-card-title>
            <v-card-subtitle class="pb-0">{{ formatDate(edge.node.date) }}</v-card-subtitle>
            <v-card-actions>
            <v-btn
                color="orange"
                text
                @click="$router.push(`/events/${edge.node.id}`)"
            >
                More Info
            </v-btn>
            </v-card-actions>
        </v-card>
    </v-row>
  </Layout>
</template>

<page-query>
query {
    events: allEvent {
        edges {
            node {
                id
                title
                description
                price
                duration
                date
                thumbnail
                image
                category 
            }
        }
    }
}
</page-query>

<script>
import moment from 'moment'

export default {
  metaInfo: {
    title: 'Hello, world!'
  },
  data() {
      return {
          tab: 0,
          events: []
      }
  },
  watch: {
      // watch tab change
      tab(val) {
          this.tab === 0 ? this.showAllEvents() : this.showEventsByType(val)
      }
  },
  methods: {
      showAllEvents() {
          this.events = this.$page.events.edges
      },
      showEventsByType(val) {
          this.events = this.$page.events.edges.filter((edge) => {
              return edge.node.category === val
          })
      },
      formatDate(date) { // convert datetime to readable date using moment
        return moment(date).format('MMMM Do YYYY, h:mm a')
      },
      getEvents(searchText) {
          return this.events.filter((edge) => {
              // filter by lowercase, check if searchbox includes the text
              return edge.node.title.toLowerCase().includes(searchText.toLowerCase())
          })
      }
  },
  mounted() {
      this.events = this.$page.events.edges // load graphql data to events array
  }
}
</script>

<style>
.home-links a {
  margin-right: 1rem;
}
</style>
