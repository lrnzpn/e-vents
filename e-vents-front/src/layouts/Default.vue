<template>
  <v-app>
    <v-app-bar app>
        <v-toolbar-title>
            <g-link to="/">{{$static.metadata.siteName}}</g-link>
        </v-toolbar-title>
        <v-text-field 
            placeholder="Search"
            class="ml-8"
            outlined
            rounded
            dense
            hide-details
            style="max-width: 350px"
            prepend-inner-icon="mdi-magnify"
            v-model="searchText"
            clearable
            @click:clear="searchText = ''"
        >
        </v-text-field>
        <v-spacer></v-spacer>
        <v-btn>Create Event</v-btn>
    </v-app-bar>
    <v-main>
        <v-container>
            <v-row>
                <v-col sm="6" offset-sm="3">
                    <!-- pass data to slot with v-bind -->
                    <slot :searchText="searchText" />
                </v-col>
            </v-row>
        </v-container>
    </v-main>
  </v-app>
</template>

<static-query>
query {
  metadata {
    siteName
  }
}
</static-query>

<script>
export default {
    data() {
        return {
            searchText: '',
        }
    }
}
</script>

<style lang="scss">
.v-toolbar__title a {
    text-decoration: none;
    color: black !important;
}
</style>