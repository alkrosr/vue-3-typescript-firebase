<script setup lang="ts">
import type { Song } from '@/types'
import { deleteSong } from '@/firebase/songs'

const props = defineProps<{
  songs: Song[]
  isLoading: boolean
}>()
const delSong = (song: Song) => {
  deleteSong(song.id)
}
</script>

<template>
  <v-card class="mx-auto pa-2" max-width="300">
    <v-list>
      <v-list-subheader>FAVORITE SONGS</v-list-subheader>

      <template v-if="props.isLoading">
        <div class="d-flex justify-center align-center loading-height">
          Loading...
        </div>
      </template>

      <template v-else>
        <v-list-item
          v-for="(song, i) in props.songs"
          :key="i"
          :value="song"
          active-color="primary"
          rounded="shaped"
        >
          <template v-slot:prepend>
            <v-icon icon="mdi-headphones"></v-icon>
          </template>

          <template v-slot:append>
            <v-icon @click="delSong(song)" icon="mdi-close"></v-icon>
          </template>

          <v-list-item-title v-text="song.title"></v-list-item-title>
        </v-list-item>
      </template>
    </v-list>
  </v-card>
</template>
