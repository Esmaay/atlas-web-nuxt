<script setup lang="ts">
const { $orpc } = useNuxtApp()
import { useQuery } from '@tanstack/vue-query'

const TITLE_TEXT = `
 ██████╗ ███████╗████████╗████████╗███████╗██████╗
 ██╔══██╗██╔════╝╚══██╔══╝╚══██╔══╝██╔════╝██╔══██╗
 ██████╔╝█████╗     ██║      ██║   █████╗  ██████╔╝
 ██╔══██╗██╔══╝     ██║      ██║   ██╔══╝  ██╔══██╗
 ██████╔╝███████╗   ██║      ██║   ███████╗██║  ██║
 ╚═════╝ ╚══════╝   ╚═╝      ╚═╝   ╚══════╝╚═╝  ╚═╝

 ████████╗    ███████╗████████╗ █████╗  ██████╗██╗  ██╗
 ╚══██╔══╝    ██╔════╝╚══██╔══╝██╔══██╗██╔════╝██║ ██╔╝
    ██║       ███████╗   ██║   ███████║██║     █████╔╝
    ██║       ╚════██║   ██║   ██╔══██║██║     ██╔═██╗
    ██║       ███████║   ██║   ██║  ██║╚██████╗██║  ██╗
    ╚═╝       ╚══════╝   ╚═╝   ╚═╝  ╚═╝ ╚═════╝╚═╝  ╚═╝
 `;

const healthCheck = useQuery($orpc.healthCheck.queryOptions())
</script>

<template>
  <div class="container mx-auto max-w-3xl px-4 py-2">
    <pre class="overflow-x-auto font-mono text-sm whitespace-pre-wrap">{{ TITLE_TEXT }}</pre>
    <div class="grid gap-6 mt-4">
      <section class="rounded-lg border p-4">
        <h2 class="mb-2 font-medium">API Status</h2>
        <div class="flex items-center gap-2">
            <div class="flex items-center gap-2">
              <div
                class="w-2 h-2 rounded-full"
                :class="{
                  'bg-yellow-500 animate-pulse': healthCheck.status.value === 'pending',
                  'bg-green-500': healthCheck.status.value === 'success',
                  'bg-red-500': healthCheck.status.value === 'error',
                  'bg-gray-400': healthCheck.status.value !== 'pending' &&
                                  healthCheck.status.value !== 'success' &&
                                  healthCheck.status.value !== 'error'
                }"
              ></div>
              <span class="text-sm text-muted-foreground">
                <template v-if="healthCheck.status.value === 'pending'">
                  Checking...
                </template>
                <template v-else-if="healthCheck.status.value === 'success'">
                  Connected ({{ healthCheck.data.value }})
                </template>
                <template v-else-if="healthCheck.status.value === 'error'">
                  Error: {{ healthCheck.error.value?.message || 'Failed to connect' }}
                </template>
                 <template v-else>
                  Idle
                </template>
              </span>
            </div>
          </div>
      </section>
    </div>
  </div>
</template>
