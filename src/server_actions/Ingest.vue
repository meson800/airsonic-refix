<template>
  <div>
    <h1 class="mb-3 mr-2">Add music</h1>
    <form class="mb-3">
      <div class="form-group">
        <label for="spotifyLink">Spotify link</label>
        <input type="text" class="form-control" id="spotifyLink" v-model="url" placeholder="Enter an https://open.spotify.com URL">
        <small id="spotifyHelp" class="form-text text-muted">You can add a single song, or an album, or anything listed <a href="https://spotdl.github.io/spotify-downloader/usage/">here</a>.</small>
      </div>
      <div class="form-check">
        <input type="checkbox" class="form-check-input" v-model="includeAlbum" id="includeAlbum">
        <label class="form-check-label" for="includeAlbum">Download other songs in the same album(s)</label>
      </div>
      <button type="submit" class="btn btn-primary mt-1" @click="enqueue">Submit</button>
    </form>
    <h3 class="mb-3">Download queue</h3>
    <ul class="list-group">
      <li v-for="(entry, index) in queueEntries" :key="index" class="list-group-item">
        <span v-bind:class="badgeClass(entry)"> {{ entry.stage }}</span>
        <span class="ml-2"><a href="{{ entry.url }}">{{ entry.url }}</a>{{ entry.include_albums ? " (including album)" : "" }}</span>
      </li>
    </ul>
  </div>
</template>
<script lang="ts">
  import { computed, defineComponent, ref } from 'vue'

  export default defineComponent({
    mounted: function() {
      this.timer = setInterval(() => { this.fetchQueue() }, 1500)
    },
    beforeDestroy() {
      clearInterval(this.timer)
    },
    data() {
      return {
        queueEntries: [],
        url: '',
        includeAlbum: false,
        timer: 0
      }
    },
    methods: {
      async enqueue(event: any) {
        event.preventDefault()
        if (this.url.length === 0) {
          return
        }
        const url = '/api/queue'
        await fetch(url, {
          method: 'post',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ url: this.url, include_album: this.includeAlbum })
        }).then(
          response => response.ok ? '' : Promise.reject(new Error(response.statusText))
        )
        this.url = ''
      },
      async fetchQueue() {
        const url = '/api/queue'
        this.queueEntries = await fetch(url).then(
          response => response.ok ? response.json() : Promise.reject(new Error(response.statusText))
        )
      },
      badgeColor(entry: any) {
        if (!entry.completed) {
          return 'primary'
        }
        if (entry.stage.startsWith('done')) {
          return 'success'
        }
        return 'danger'
      },
      badgeClass(entry: any) {
        return `badge text-white bg-${this.badgeColor(entry)}`
      }
    }
  })
</script>
