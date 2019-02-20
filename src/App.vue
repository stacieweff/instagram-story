<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png"> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <nav>
      <div v-for="(color, index) in colors">
        <div></div>
      </div>
    </nav>
  
    <section v-for="(color, index) in colors" :key="index" :style="{ background: color }" v-if="current == index"></section>
  </div>
</template>

<script>

// import HelloWorld from './components/HelloWorld.vue'
import anime from 'animejs/lib/anime.es.js'

export default {
  name: 'app',
  components: {
    // HelloWorld
  },
  data() {
    return {
      colors: ['#D53738', '#638867', '#FAF429'],
      current: 0
    }
  },
  mounted() {
    let timeline = anime.timeline({
      autoplay: true,
      duration: 10000,
      easing: 'linear',
      loop: true
    })
    
    this.colors.forEach((color, index) => {
      timeline.add({
        targets: document.querySelectorAll('nav > div')[index].children[0],
        width: '100%',
        changeBegin: (a) => {
          this.current = index
        }
      })
    })

        let hammertime = new Hammer(document.querySelector('#app'))
    
    hammertime.on('press', (e) => {
      timeline.pause()
    })

    hammertime.on('pressup', (e) => {
      timeline.play()
    })
    
    hammertime.on('tap', (e) => {
      if (e.center.x > window.innerWidth / 2) {
        if (this.current < this.colors.length - 1) {
          this.current += 1
          
          timeline.pause()
          timeline.seek(this.current * 10000)
          timeline.play()
        }
      } else {
        if (this.current > 0) {
          this.current -= 1
          
          timeline.pause()
          timeline.seek(this.current * 10000)
          timeline.play()
        }
      }
    })
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

html, body, main, section{
  height: 100vh;
  width: 100%;
}

nav{
  box-sizing: border-box;
  display: grid;
  grid-column-gap: 1em;
  grid-template-columns: repeat(3, 1fr);
  height: 0.5em;
  padding: 0 1em;
  position: fixed;
  top: 1em;
  width: 100%;
}

nav > div{
  background: rgba(0,0,0,0.25);
  height: 100%;
}
nav > div > div{
  background: black;
  height: 100%;
  width: 0%;
}
</style>
