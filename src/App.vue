<template>
  <div id="app">
    <header>
      <h1>My Music</h1>
    </header>
    <main>
      <section class="player">

        <h2 class="song-title">{{ current.title }} - <span>{{ current.artist }}</span></h2>

        <img :src="current.img" class="album-cover" />

        <div class="controls">
          <button ><i class="fas fa-backward"></i></button>
          <button class="prev" @click="prev">Prev</button>
          <button class="play" v-if="!isPlaying" @click="play"></button>
          <button class="pause" v-else @click="pause"></button>
          <button class="next" @click="next">Next</button>
        </div>

      </section>


      <section class="playlist">

        <h3>The Playlist</h3>

        <button v-for="song in songs" :key="song.src" @click="play(song)"
          :class="(song.src == current.src) ? 'song playing' : 'song'">
          {{ song.title }} - {{ song.artist }}
        </button>

      </section>

    </main>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: 'KIF KEEM',
          artist: 'Farabi',
          src: require('./assets/Farabi-KIF-KEEM.mp3'),
          img: require('./assets/farabi.png')
        },
        {
          title: 'Midnight Love',
          artist: 'Testa Rossa',
          src: require('./assets/TestaRossa-Midnight_Love.mp3'),
          img: require('./assets/love.png')
        },
        {
          title: 'BBY',
          artist: 'Two Feet',
          src: require('./assets/TwoFeet_BBY.mp3'),
          img: require('./assets/bby.png')
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener('ended', function () {
        this.index++;
        if (this.index > this.songs.length - 1) {
          this.index = 0;
        }
        this.current = this.songs[this.index];
        this.play(this.current);
      }.bind(this));
      this.isPlaying = true;
    },
    pause() {
      this.player.pause();
      this.isPlaying = false;
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
  }
}
</script>
