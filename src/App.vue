<template>
  <div id="app">
    <div v-if="step3" :class="(unveil ? 'swoop' : '' )" class="logo-loader"></div>
    <div :class="(unveil ? 'loading-veil unveil' : 'loading-veil')">
      <svg v-if="!step2 && !step3" width="200" height="200" viewBox="0 0 200 200" fill="none" xmlns="http://www.w3.org/2000/svg">
        <circle id="initial-logo" cx="100" cy="100" r="72.5" stroke="white" stroke-width="55"/>
      </svg>
      <div v-if="step2 && !step3" class="chunk-logo">
        <div class="chunk-big">
          <svg width="200" height="200" viewBox="0 0 200 200" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path fill-rule="evenodd" clip-rule="evenodd" d="M100 0C44.7715 0 0 44.7715 0 100C0 155.228 44.7715 200 100 200C155.228 200 200 155.228 200 100C200 44.7715 155.228 0 100 0ZM100 55.2504C75.2855 55.2504 55.2504 75.2855 55.2504 100C55.2504 124.715 75.2855 144.75 100 144.75C124.715 144.75 144.75 124.715 144.75 100C144.75 75.2855 124.715 55.2504 100 55.2504ZM200 99.8385H144.749C144.749 99.8923 144.75 99.9461 144.75 100C144.75 124.715 124.715 144.75 100 144.75C99.9461 144.75 99.8923 144.749 99.8385 144.749V200C99.8923 200 99.9461 200 100 200C155.228 200 200 155.228 200 100C200 99.9461 200 99.8923 200 99.8385Z" fill="white"/>
          </svg>
        </div>
        <div class="chunk-small">
          <svg width="100" height="100" viewBox="0 0 100 100" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path fill-rule="evenodd" clip-rule="evenodd" d="M99.9999 2.41118e-06L44.8384 0C44.8386 0.0537423 44.8387 0.107507 44.8387 0.161294C44.8387 24.836 24.8359 44.8387 0.16129 44.8387C0.107505 44.8387 0.0537411 44.8386 0 44.8384V99.9999C0.0537535 100 0.107517 100 0.16129 100C55.3007 100 100 55.3007 100 0.161294C100 0.10752 100 0.0537562 99.9999 2.41118e-06Z" fill="#FFFFFF"/>
          </svg>
        </div>
      </div>
      <!--div v-if="step3" class="emblem-logo"></div-->
    </div>
    <div class="cc" :class="[ 'g-cursor', { 'g-cursor_hover': hover }, {'g-cursor_hide': hideCursor} ]">
      <div :style="cursorCircle" class="g-cursor__circle"></div>
      <div class="g-cursor__point" ref="point" :style="cursorPoint"></div>
    </div>
    <router-view :dataRef='dataRef' />
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {
    //CustomCursor
  },
  data() {
    return {
      step3: false,
      step2: false,
      dataRef: {},
      xChild: 0,
      yChild: 0,
      xParent: 0,
      yParent: 0,
      hover: false,
      hideCursor: false,
      unveil: null
    }
  },
  computed: {
    cursorCircle() {
      return `transform: translateX(${this.xParent}px) translateY(${this.yParent}px) translateZ(0) translate3d(0, 0, 0);`
    },
    cursorPoint() {
      return `transform: translateX(${this.xChild - 3}px) translateY(${this.yChild - 3}px) translateZ(0) translate3d(0, 0, 0);`
    }
  },
  mounted() {
    console.log('stroke length: ', document.getElementById('initial-logo').getTotalLength());
    setTimeout(() => {
      this.changeLogo('step3');
    }, 5000);
    setTimeout(() => {
      this.changeLogo('step2');
    }, 3000);
    setTimeout(() => {
      this.unveil = true;
      //this.waveOffset = 700;
      console.log('unveiled');
      setTimeout(() => {
        document.getElementById('landing-video').play(); //delay landing page video
        document.getElementById('second-custom').play(); //delay landing page video
      }, 500);
    }, 5100);
    document.addEventListener("mousemove", this.moveCursor);
    document.addEventListener('mouseleave', (e) => {
      this.hideCursor = true;
      console.log('left: ', e.target);
    });
    document.addEventListener('mouseenter', (e) => {
      this.hideCursor = false;
      console.log('entered: ', e.target);
    });
  },
  methods: {
    handleNav(index) {
      console.clear();
      console.log(index);
      console.log('ROUTE HOME');
      this.$refs.fullpage.api.moveTo(1);
    },
    moveCursor(e) {
      var self = this;

      //console.log(e.target.classList);

      if(e.target.classList.contains('hoverable') || e.target.classList.contains('fp-tooltip') || e.target.tagName == "SPAN") {
        console.log(e.target.tagName);
        //console.log('HOVERABLE:)');
        self.hover = true;
      } else {
        //console.log('NOT HOVERABLE!');
        self.hover = false;
      }

      this.xChild = e.clientX;
      this.yChild = e.clientY;
      setTimeout(() => {
        this.xParent = e.clientX - 20;
        this.yParent = e.clientY - 20;
      }, 100);
    },
    changeLogo(context) {
      if(context == 'step2') {
        this.step2 = true;
      } else if(context == 'step3') {
        this.step3 = true;
        this.step2 = false;
      }
    }
  }
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Nunito&display=swap');

.emblem-logo {
  //background: blue;
  width: 200px;
  height: 200px;
  background-image: url('assets/podular-white-emblem.svg');
  background-size: contain;
  background-position: center;
  background-repeat: no-repeat;
  transform: translate(12px, 12px) scale(1.13);
}

.chunk-logo {
  //background: red;
  width: 200px;
  height: 200px;

  .chunk-big {
    position: absolute;
  }

  .chunk-small {
    position: absolute;
    transform: translate(100px,100px);
    animation: pop 2s cubic-bezier(0.65, 0, 0.35, 1) forwards;
  }
}

#initial-logo {
  height: 200px;
  width:200px;
  stroke-dasharray: 455;
  stroke-dashoffset: 455;
  animation: 3s cubic-bezier(0.65, 0, 0.35, 1) forwards circle-draw;
}

@keyframes pop {
  to {
    transform: translate(125px, 125px);
  }
}

@keyframes circle-draw {
  to {
    stroke-dashoffset: 0;
  }
}

html {
  transition: filter 1s;
}

#fullpage {
  transition-delay: 1s;
}

.fp-slidesNav {
  z-index: 1 !important;

  ul li a {
    margin-top: -36px;

    span {
      background: white !important;
    }
  }
}

.fp-controlArrow {
  top: 91.6vh !important;
  transition: 300ms;

  &:hover {
    opacity: 0.3;
  }
}

.fp-next {
  border: none;
  height: 24px !important;
  width: 24px !important;
  margin: 0px;
  background-image: url('assets/icons/arrow.svg');
  background-size: contain;
  background-position: center;
  background-repeat: no-repeat;
  right: 100px !important;
  bottom: 24px !important;
}

.fp-prev {
  border: none;
  height: 24px !important;
  width: 24px !important;
  margin: 0px;
  background-image: url('assets/icons/arrow.svg');
  transform: rotate(180deg);
  background-size: contain;
  background-position: center;
  background-repeat: no-repeat;
  left: 100px !important;
  bottom: 24px !important;
}

.swoop {
  transform: translate(-22px, -22px) scale(0.4) !important;
  //background: green !important;
  transition: 1s cubic-bezier(0.65, 0, 0.35, 1);
}

.logo-loader {
  margin-top: 0px !important;
  position: absolute;
  top: 0px;
  left: 0px;
  transform: translate(calc((50vw - 88px)), calc((50vh - 88px))) scale(1.13); //  transform: translate(12px, 12px) scale(1.13);
  margin: auto;
  z-index: 999999;
  width: 200px;
  height: 200px;
  //background: pink;  
  transition: 2s cubic-bezier(0.65, 0, 0.35, 1);
  background-image: url('assets/podular-white-emblem.svg');
  background-position: center;
  background-size: contain;
  background-repeat: no-repeat;
}

.fp-slideNav {
  ul li a span {
    background: red !important;
  }
}

#fp-nav {
  margin-top: -190px !important;
  margin-left: 42px;

  li {
    margin-bottom: 36px !important;
  }

  .fp-tooltip {
    font-size: 14px !important;
    padding-left: 32px;
    line-height: 36px;
  }

  .active span {
    opacity: 1 !important;
    background: white !important;
    border: 2px solid white;
    //transform: translate(4px,4px);

    &:hover {
      //transform: translate(-1px,-1px) !important;
      //transform: scale(1.5);
    }
  }

  span {
    background: white !important;
    opacity: 0.2;
    //background: rgba(0,0,0,0) !important;
    height: 24px !important;
    width: 24px !important;
    //transform: translate(-1px,-1px);
    transform: none !important;

    &:hover {
      //transform: translate(-1px,-1px);
      //transform: scale(1.05);
    }
  }
}

.unveil {
  opacity: 0 !important;
  transition: 6s;
  pointer-events: none;
}

.loading-veil {
  //display: none;
  background: black;
  opacity: 1;
  width: 100%;
  height: 100vh;
  position: fixed;
  margin: 0px;
  padding: 0px;
  pointer-events: none;
  z-index: 99999;
  display: flex;
  justify-content: center;
  align-items: center;
  transition: 8s;
}

// ---------- transition shit -----------

//transition animation fade

.fade-enter, .fade-leave-to {
  opacity: 0;
  transform: scale(0.95);
}

.fade-enter-active, .fade-leave-active {
  transition: all 300ms ease;
}

body {
  padding: 0px;
  margin: 0px;
  //cursor: none;
  font-family: 'Nunito', sans-serif;
}

#app {
  font-family: 'Gotham', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: white;
  background: #222; 
  font-weight: normal;
}

// --------- cursor stuff ----------

.g-cursor {
    position: absolute;

    &_hide {
      opacity: 0;
      width: 60px;
      height: 60px;
      transition: 
        width .6s ease,
        height .6s ease,
        opacity .6s ease;
    }

    &__circle {
      pointer-events: none;
      user-select: none;
      top: 2px;
      left: 2px;
      position: fixed;
      width: 36px;
      height: 36px;
      border: 2px solid rgba(white, 0.4);
      mix-blend-mode: difference;
      //background: rgba(white,0.3);
      border-radius: 100%;
      z-index: 5555;
      backface-visibility: hidden;
      transition: 
        margin 0.6s ease,
        opacity 0.6s ease,
        width 0.6s ease,
        height 0.6s ease,
    }

    &__point {
      top: 0;
      left: 0;
      opacity: 0;
      position: fixed;
      width: 10px;
      height: 10px;
      pointer-events: none;
      user-select: none;
      border-radius: 100%;
      background: #fff;
      z-index: 55555555;
      backface-visibility: hidden;
      will-change: transform;
      transition:
        margin .4s ease,
        opacity .4s ease,
        width .4s ease,
        height .4s ease;
    }

    &_hover {
      .g-cursor__point {
          opacity: 0;
          width: 80px;
          height: 80px;
          margin-left: -42px;
          margin-top: -42px;
          border: 2px solid rgba(white, 1);
          //background: white;
          //mix-blend-mode: difference;
          //background: rgba(white, 1);
          //border-color: white;
          transition: 
            margin .4s ease,
            width .4s ease,
            height .4s ease,
            opacity .4s ease,
            transform 0s;
        }

        .g-cursor__circle {
          opacity: 1;
          width: 74px;
          height: 74px;
          margin-left: -22px;
          margin-top: -22px;
          //background: rgba(white, 1);
          //border-color: transparent;
          transition: 
            margin .4s ease,
            width .4s ease,
            height .4s ease,
            opacity .4s ease;
        }
    }
}

@media only screen and (max-width: 900px) {
  .g-cursor {
    display: none !important;
  }
}
</style>
