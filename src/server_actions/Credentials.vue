<template>
  <div>
    <h1 class="mb-3 mr-2">Connect Subsonic apps</h1>
    <div>
      <p>
        You can use the following settings to connect any Subsonic-compatible app to this media server.
      </p>
      <ul>
        <li class="mb-1"> <span style="width: 6em; display: inline-block">Server:</span> <a href="{{ window.location.origin }}">{{ window.location.origin }}</a></li>
        <li class="mb-1"> <span style="width: 6em; display: inline-block">Username:</span> <span class="text-monospace bg-dark p-1"> {{ auth.username }} </span></li>
        <li class="mb-1"> <span style="width: 6em; display: inline-block">Password:</span> <span class="text-monospace bg-dark p-1"> {{ auth.cached_password }} </span></li>
      </ul>
    </div>
  </div>
</template>
<script lang="ts">
  import { computed, defineComponent, ref } from 'vue'
  import { useAuth } from '@/auth/service'

  export default defineComponent({
    setup() {
      return {
        auth: useAuth(),
      }
    },
    data() {
      return {
        window: window
      }
    }
  })
</script>
