<template>
  <v-app dark>
    <v-app-bar
      fixed
      app
    >
      <v-toolbar-title class="monospaced">
        <strong>
          Snippets Generator
        </strong>
      </v-toolbar-title>
      <v-spacer />
    </v-app-bar>
    <v-main>
      <v-container>
        <nuxt />
      </v-container>
    </v-main>
    <v-footer
      app
    >
      <span>&copy; {{ new Date().getFullYear() }}</span>
    </v-footer>
  </v-app>
</template>

<style lang="scss">
.monospaced {
  font-family: 'Courier New', Courier, monospace;
  font-weight: bold;
}

html {
  overflow-y: hidden;
}
</style>
