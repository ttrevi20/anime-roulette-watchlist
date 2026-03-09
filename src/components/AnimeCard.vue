<script setup>
import { computed, ref, watch } from 'vue'

const props = defineProps({
  anime: {
    type: Object,
    default: null,
  },
  loading: {
    type: Boolean,
    default: false,
  },
  error: {
    type: String,
    default: '',
  },
  inWatchlist: {
    type: Boolean,
    default: false,
  },
})

const emit = defineEmits(['add'])
</script>

<template>
  <section
    class="rounded-3xl border border-violet-800/70 bg-violet-900/60 p-5 shadow-2xl shadow-indigo-950/30 backdrop-blur-sm"
  >
    <div
      v-if="props.loading"
      class="space-y-4"
    >
      <div class="flex items-center gap-3 text-pink-300">
        <div
          class="h-5 w-5 animate-spin rounded-full border-2 border-pink-400 border-t-transparent"
        ></div>
        <p class="font-semibold tracking-wide">Shuffling anime reels...</p>
      </div>

      <div class="grid grid-cols-3 gap-2">
        <div class="h-3 animate-pulse rounded bg-indigo-700"></div>
        <div class="h-3 animate-pulse rounded bg-indigo-700"></div>
        <div class="h-3 animate-pulse rounded bg-indigo-700"></div>
      </div>
      <div class="h-72 animate-pulse rounded-2xl bg-indigo-800"></div>
    </div>

    <div v-else-if="props.error" class="space-y-3">
      <div class="rounded-xl border border-pink-500/50 bg-pink-500/10 px-4 py-3">
        <p class="text-sm font-semibold text-pink-200">{{ props.error }}</p>
      </div>
    </div>

    <div v-else-if="props.anime" class="space-y-4">
      <div class="aspect-video w-full overflow-hidden rounded-2xl bg-black-800">
        <img
          :src="props.anime.images?.jpg?.large_image_url || props.anime.images?.jpg?.image_url"
          :alt="props.anime.title"
          class="h-full w-full object-cover"
        />
      </div>

      <div class="space-y-2">
        <h3 class="text-2xl font-bold text-white">{{ props.anime.title }}</h3>
        
        <div class="grid grid-cols-2 gap-3 text-sm">
          <div>
            <p class="text-green-400">Score</p>
            <p class="text-lg font-semibold text-red-300">{{ props.anime.score || 'N/A' }}</p>
          </div>
          <div>
            <p class="text-green-400">Episodes</p>
            <p class="text-lg font-semibold text-red-300">{{ props.anime.episodes || '?' }}</p>
          </div>
        </div>

        <div v-if="props.anime.rating" class="rounded-lg bg-blue-950/50 px-3 py-2">
          <p class="text-xs font-semibold text-blue-400 uppercase tracking-wider">Rating: {{ props.anime.rating }}</p>
        </div>

        <p v-if="props.anime.synopsis" class="line-clamp-3 text-sm text-blue-300">
          {{ props.anime.synopsis }}
        </p>
      </div>

      <div class="flex gap-3">
        <a
          :href="props.anime.url"
          target="_blank"
          rel="noopener noreferrer"
          class="flex-1 rounded-lg border border-emerald-800 bg-emerald-800/50 px-4 py-2 text-center text-sm font-semibold text-slate-200 hover:bg-cyan-700/50"
        >
          View on MAL
        </a>
        <button
          v-if="!props.inWatchlist"
          type="button"
          @click="emit('add', props.anime)"
          class="flex-1 rounded-lg border border-cyan-300/50 bg-cyan-500/20 px-4 py-2 text-sm font-semibold text-cyan-200 hover:bg-emerald-500/30"
        >
          Add to Watchlist
        </button>
        <div
          v-else
          class="flex-1 rounded-lg border border-fuchsia-800/50 bg-fuchsia-500/20 px-4 py-2 text-center text-sm font-semibold text-fuchsia-200"
        >
          ✓ In Watchlist
        </div>
      </div>
    </div>

    <div v-else class="text-center space-y-3 py-12">
      <p class="text-lime-600">No anime selected. Click the SPIN button to get started!</p>
    </div>
  </section>
</template>