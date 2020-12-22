<template>
  <v-card>
    <v-card-actions>
      <span class="monospaced ml-3" style="font-size: 1.5em">
        for VSCode
      </span>
      <v-spacer />
      <v-tooltip bottom>
        <template #activator="{ on, attrs }">
          <v-btn icon large v-bind="attrs" v-on="on" @click="$emit('generate')">
            <v-icon>
              mdi-content-save
            </v-icon>
          </v-btn>
        </template>
        <span>Generate snippets</span>
      </v-tooltip>
      <v-tooltip bottom>
        <template #activator="{ on, attrs }">
          <v-btn icon large v-bind="attrs" v-on="on" @click="$emit('upload')">
            <v-icon>
              mdi-upload
            </v-icon>
          </v-btn>
        </template>
        <span>Import VSCode snippets</span>
      </v-tooltip>
      <v-tooltip bottom>
        <template #activator="{ on, attrs }">
          <v-btn icon large v-bind="attrs" v-on="on" @click="$emit('save')">
            <v-icon>
              mdi-cloud-upload
            </v-icon>
          </v-btn>
        </template>
        <span>Save snippets data on browser storage</span>
      </v-tooltip>
      <v-tooltip bottom>
        <template #activator="{ on, attrs }">
          <v-btn icon large v-bind="attrs" v-on="on" @click="$emit('load')">
            <v-icon>
              mdi-sync
            </v-icon>
          </v-btn>
        </template>
        <span>Load snippets data from browser storage</span>
      </v-tooltip>
      <v-tooltip bottom>
        <template #activator="{ on, attrs }">
          <v-btn icon large v-bind="attrs" v-on="on" @click="$emit('truncate')">
            <v-icon>
              mdi-delete
            </v-icon>
          </v-btn>
        </template>
        <span>Delete all snippets</span>
      </v-tooltip>
    </v-card-actions>
  </v-card>
</template>

<script lang="ts">
import { Vue, Component } from 'nuxt-property-decorator'

@Component({})
class Controller extends Vue {
}
export default Controller
</script>
