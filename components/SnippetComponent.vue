<template>
  <v-expansion-panel>
    <v-expansion-panel-header class="monospaced" style="font-size: 1.2em">
      <div>
        <v-btn icon class="ml-n3" @click="$emit('remove')">
          <v-icon>mdi-close</v-icon>
        </v-btn>
        {{ _snippet.name }}
      </div>
    </v-expansion-panel-header>
    <v-expansion-panel-content>
      <v-row>
        <v-col cols="12" sm="6">
          <v-text-field
            v-model="_snippet.name"
            label="Name"
            class="monospaced"
          />
        </v-col>
        <v-col cols="12" sm="6">
          <v-text-field
            v-model="_snippet.trigger"
            label="Trigger"
            class="monospaced"
          />
        </v-col>
        <v-col>
          <v-textarea
            v-model="_snippet.body"
            label="Body"
            class="monospaced"
            rows="10"
          />
        </v-col>
      </v-row>
    </v-expansion-panel-content>
  </v-expansion-panel>
</template>

<script lang="ts">
import { Vue, Component, PropSync } from 'nuxt-property-decorator'
import { Snippet } from '@/types'

@Component({})
class SnippetComponent extends Vue {
  @PropSync('snippet', { type: Object })
  _snippet!: Snippet
}
export default SnippetComponent
</script>
