<script setup lang="ts">
import { onMounted, ref } from 'vue'
import type { Song } from '@/types'
import { getSongs, addSongs } from '@/firebase/songs'
import SongsList from '@/components/SongsList.vue'
import SongsPlayer from '@/components/SongsPlayer.vue'

const songs = ref<Song[]>([])
const isLoading = ref(false)
const addNotificationShow = ref(false)

const playingSongs: Song[] = [
  {
    id: '1',
    title: 'So What',
    artist: 'Miles Davis',
    year: 1959,
  },
  {
    id: '2',
    title: 'Smells Like Teen Spirit',
    artist: 'Nirvana',
    year: 1991,
  },
  {
    id: '3',
    title: 'What a Wonderful World',
    artist: 'Louis  Armstrong',
    year: 1967,
  },
  {
    id: '4',
    title: 'The Look',
    artist: 'Roxette',
    year: 1988,
  },
]

onMounted(async () => {
  getSongs(songs, isLoading)
})

const addToFavorites = (id: string) => {
  const song = playingSongs.find((s) => s.id === id)

  if (song) {
    addSongs(song)
    addNotificationShow.value = true
  }
}
</script>

<template>
  <main>
    <SongsPlayer @add-to-favorite="addToFavorites" :songs="playingSongs" />
    <SongsList :is-loading="isLoading" :songs="songs" />

    <v-snackbar
      :timeout="2000"
      color="primary"
      variant="tonal"
      v-model="addNotificationShow"
    >
      Song add to favorites!
    </v-snackbar>
  </main>
</template>

<style scoped></style>
