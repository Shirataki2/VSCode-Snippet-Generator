<template>
  <v-row justify="center" align="center">
    <v-col col="12" lg="10" xl="8" offset-lg="1" offset-xl="2">
      <Controller
        @generate="generate"
        @save="save"
        @upload="upload"
        @load="load"
        @truncate="truncate"
      />
      <v-dialog v-model="dialog" max-width="1000">
        <v-card>
          <v-card-title>
            Generated JSON
            <v-spacer />
            <v-btn
              large
              color="primary"
              @click="onCopy"
            >
              Copy to clipboard
            </v-btn>
          </v-card-title>
          <v-card-text>
            <v-textarea
              v-model="generated"
              outlined
              class="monospaced"
              readonly
              rows="15"
            />
          </v-card-text>
        </v-card>
      </v-dialog>
      <v-expansion-panels v-model="panel" class="mt-2" accordion>
        <SnippetComponent
          v-for="(snippet, index) in snippets"
          :key="index"
          :snippet.sync="snippet"
          @remove="onSnippetRemove(index)"
        />
      </v-expansion-panels>
      <input id="file-input" type="file" style="display: none" accept="application/JSON, .code-snippets" @change="onFileLoad">
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
  defaultSnippet = { name: '', trigger: '', body: '' }
  snippets: Array<Snippet> = [this.defaultSnippet]

  snackbarEnabled = false
  snackbarContent = ''
  snackbarColor = 'success'

  dialog = false
  generated = ''

  onSnippetRemove (index: number) {
    if (!confirm(`Are you sure you want to remove this snippet?:\n${this.snippets[index].name}`)) { return }
    this.snippets.splice(index, 1)
  }

  appendSnippet () {
    this.snippets.push({ name: '', trigger: '', body: '' })
  }

  generate () {
    const data: any = {}
    for (const snippet of this.snippets) {
      const body = snippet.body.split('\n').map(
        line => line.replace('\t', '    ').replace(/^(.*?)/, function (_, p1) {
          return `${p1}`
        })
      )
      data[snippet.name] = {
        prefix: snippet.trigger,
        description: snippet.name,
        body
      }
    }
    this.generated = JSON.stringify(data, null, 2)
    this.dialog = true
  }

  onCopy () {
    const container = document.querySelector('.v-dialog')!
    this.$copyText(this.generated, container).then(() => {
      this.snackbarColor = 'primary'
      this.snackbarContent = 'Copied!'
      this.snackbarEnabled = true
    }).catch(() => {
      this.snackbarColor = 'error'
      this.snackbarContent = 'Failed to copy to clipboard'
      this.snackbarEnabled = true
    })
  }

  save () {
    const payload = JSON.stringify(this.snippets)
    localStorage.setItem('data', payload)
    this.snackbarColor = 'primary'
    this.snackbarContent = 'Saved!'
    this.snackbarEnabled = true
  }

  upload () {
    const dom = document.getElementById('file-input')!
    dom.click()
  }

  onFileLoad ({ target }: Event) {
    if (target && target instanceof HTMLInputElement && target.files) {
      const file = target.files[0]
      const reader = new FileReader()
      reader.onload = (e) => {
        const result = e.target?.result
        if (typeof result === 'string') {
          const payload: Object = JSON.parse(result)
          try {
            const snippets = Object.entries(payload).map(([k, v]) => {
              const name = k
              const trigger = v.prefix
              const body = (v.body as Array<string>).join('\n')
              return { name, trigger, body } as Snippet
            })
            this.snippets = snippets
            this.snackbarColor = 'primary'
            this.snackbarContent = 'Loaded'
            this.snackbarEnabled = true
          } catch {
            this.snackbarColor = 'error'
            this.snackbarContent = 'File parse failed'
            this.snackbarEnabled = true
          }
        } else {
          this.snackbarColor = 'error'
          this.snackbarContent = 'File type is invalid (accept only json file)'
          this.snackbarEnabled = true
        }
      }
      reader.readAsText(file)
    } else {
      this.snackbarColor = 'error'
      this.snackbarContent = 'File load failed'
      this.snackbarEnabled = true
    }
  }

  mounted () {
    const payload = localStorage.getItem('data')
    if (payload) {
      try {
        const data = JSON.parse(payload)
        this.snippets = data
      } catch {
      }
    }
  }

  load () {
    const payload = localStorage.getItem('data')
    if (payload) {
      try {
        const oldPayload = JSON.stringify(this.snippets)
        const defaultPayload = JSON.stringify(this.defaultSnippet)
        if (
          oldPayload !== defaultPayload &&
          oldPayload !== payload &&
          !confirm('Unsave data will be lost.\nAre you sure you want to continue?')
        ) { return }
        const data = JSON.parse(payload)
        this.snippets = data
      } catch {
        this.snackbarColor = 'error'
        this.snackbarContent = 'Parse failed'
        this.snackbarEnabled = true
      }
    } else {
      this.snackbarColor = 'secondary'
      this.snackbarContent = 'No saved snippets'
      this.snackbarEnabled = true
    }
  }

  truncate () {
    if (!confirm('All snippet data will be deleted.\nAre you sure you want to continue?')) { return }
    this.snippets = [{ name: '', trigger: '', body: '' }]
    localStorage.removeItem('data')
  }
}
export default Index
</script>
