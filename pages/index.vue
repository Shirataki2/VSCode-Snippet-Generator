<template>
  <v-row justify="center" align="center">
    <v-col col="12">
      <Controller
        @generate="generate"
        @save="save"
        @upload="upload"
        @load="load"
        @truncate="truncate"
      />
      <v-expansion-panels v-model="panel" class="mt-2" accordion>
        <SnippetComponent
          v-for="(snippet, index) in snippets"
          :key="index"
          :snippet.sync="snippet"
          @remove="onSnippetRemove(index)"
        />
      </v-expansion-panels>
      <AddButton
        @add="appendSnippet"
      />
    </v-col>
    <Snackbar
      :enable.sync="snackbarEnabled"
      :content.sync="snackbarContent"
      :color.sync="snackbarColor"
    />
  </v-row>
</template>

<script lang="ts">
import { Vue, Component } from 'nuxt-property-decorator'
import Controller from '@/components/Controller.vue'
import AddButton from '@/components/AddButton.vue'
import SnippetComponent from '@/components/SnippetComponent.vue'
import { Snippet } from '@/types'

@Component({
  components: {
    Controller,
    AddButton,
    SnippetComponent
  }
})
class Index extends Vue {
  panel = 0
  snippets: Array<Snippet> = [
    { name: '', trigger: '', body: '' }
  ]

  snackbarEnabled = false
  snackbarContent = ''
  snackbarColor = 'success'

  onSnippetRemove (index: number) {
    if (!confirm(`Are you sure you want to remove this snippet?:\n${this.snippets[index].name}`)) { return }
    this.snippets.splice(index, 1)
  }

  appendSnippet () {
    this.snippets.push({ name: '', trigger: '', body: '' })
  }

  generate () {}

  save () {
    this.snackbarColor = 'primary'
    this.snackbarContent = 'Saved'
    this.snackbarEnabled = true
  }

  upload () {}

  load () {}

  truncate () {}
}
export default Index
</script>
