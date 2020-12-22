<template>
  <v-snackbar
    v-model="_enable"
    :color="_color"
  >
    {{ _content }}
    <template v-slot:action="{ attrs }">
      <v-btn
        text
        v-bind="attrs"
        @click="_enable = false"
      >
        Close
      </v-btn>
    </template>
  </v-snackbar>
</template>

<script lang="ts">
import { Vue, Component, PropSync } from 'nuxt-property-decorator'

@Component({})
class Snackbar extends Vue {
  @PropSync('enable', { type: Boolean }) _enable!: boolean;
  @PropSync('content', { type: String }) _content!: string;
  @PropSync('color', { type: String }) _color!: string;
}
export default Snackbar
</script>
