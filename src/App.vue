<script setup>
import { onMounted, ref } from "vue";
import BangBangMP3 from "./assets/NancySinatra-BangBang.mp3";
import Pastlives from "./assets/Sapientdream - Pastlives.mp3";
import FILV from "./assets/FILV - autumn.mp3";

const current = ref("");
const index = ref(0);
const player = new Audio();
const isPlaying = ref(false);

const songs = ref([
  {
    title: "Bang Bang",
    artist: "Nancy Sinatra",
    src: BangBangMP3,
  },
  {
    title: "Past Lives",
    artist: "Sapient Dream",
    src: Pastlives,
  },
  {
    title: "FILV",
    artist: "autumn",
    src: FILV,
  },
]);
current.value = songs.value[index.value];
console.log(current.value);

// player.src = current.value.src;

function play(song) {
  console.log(song);
  if (typeof song.src != "undefined") {
    current.value = song;
    player.src = current.value.src;
  }
  player.play();
  player.addEventListener(
    "ended",
    function () {
      index.value++;
      if (index.value > songs.value.length) {
        index.value = 0;
      }
      pause();
      current.value = songs.value[index.value];
      play(current.value);
    }.bind()
  );
  isPlaying.value = true;
}
function pause() {
  player.pause();
  isPlaying.value = false;
}

function prev() {
  index.value--;
  if (index.value < 0) {
    index.value = songs.value.length - 1;
  }
  current.value = songs.value[index.value];
  pause();
  play(current.value);
}
function next() {
  index.value++;
  if (index.value > songs.value.length) {
    index.value = 0;
  }
  pause();
  current.value = songs.value[index.value];
  play(current.value);
}
</script>

<template>
  <main class="min-h-screen">
    <header
      class="flex justify-center bg-blue-600 text-white text-4xl font-bold py-10"
    >
      Music Player
    </header>
    <div class="justify-center text-center">
      <p class="text-4xl mt-4 font-bold">
        {{ current.title }} - {{ current.artist }}
      </p>
      <div class="mt-10 space-x-10">
        <button
          class="bg-gray-500 py-2 px-5 rounded-md text-white font-bold"
          @click="prev"
        >
          Prev
        </button>
        <button
          v-if="!isPlaying"
          class="bg-blue-500 py-3 px-8 text-2xl font-bold text-white rounded-md"
          @click="play(current)"
        >
          Play
        </button>
        <button
          v-else
          class="bg-red-500 py-3 px-8 text-2xl font-bold text-white rounded-md"
          @click="pause"
        >
          Stop
        </button>
        <button
          class="bg-gray-500 py-2 px-5 rounded-md text-white font-bold"
          @click="next"
        >
          Next
        </button>
      </div>
    </div>
    <div class="mt-5">
      <hr class="border-2" />
    </div>
    <div
      class="flex flex-col py-10 flex-1 text-center justify-center mt-4 max-w-3xl mx-auto bg-gray-700 rounded-md text-white"
    >
      <h2 class="text-4xl font-bold">Playlist</h2>
      <div class="flex flex-col justify-center text-center">
        <button
          class="mt-4 text-lg w-fit mx-auto px-2 py-1 rounded-md"
          v-for="song in songs"
          :key="song"
          @click="play(song)"
          :class="song.src == current.src ? 'bg-red-500' : ' bg-gray-500'"
        >
          {{ song.title }} - {{ song.artist }}
        </button>
      </div>
    </div>
  </main>
</template>
