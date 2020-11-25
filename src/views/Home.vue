<script>
import firebase from 'firebase'

export default {
  name: 'Home',
  data() {
    return {
      hideNav: true,
      hoveringV: [false, false, false, false],
      dotAppear: false,
      currentModalImage: null,
      modalContext: null,
      passcode: null,
      name: null,
      modalActive: false,
      text: {
        about: null,
        about2: null
      },
      enter: false,
      waveOffset: "120vw",
      waveLeft: true,
      options: {
        normalScrollElements: '.grid, .modal, .modal-active',
        loopHorizontal: false,
        scrollingSpeed: 1500,
        slidesNavigation: false,
        controlArrows: false,
        navigation: true,
        fadingEffect: true,
        navigationPosition: 'left',
        navigationTooltips: ['Home','About', 'Modular Pods', 'Customization','Showroom','Contact'],
        showActiveTooltip: true,
        easingcss3: 'cubic-bezier(0.65, 0, 0.35, 1)', //swoopy
        anchors: ['home','about', 'modular-pods', 'customization','showroom','contact'],
        onLeave: (origin, destination, direction) => {
          this.handleLeave(origin, destination, direction);
        },
        onSlideLeave: (origin, destination, direction) => {
          this.handleSlideLeave(origin, destination, direction);
        }
      },
      triggerUp: false,
      triggerDown: false,
      fix: false,
      activeSection: 0,
      activeSlide: 1,
      hover: false,
      currentExtra: '',
      currentPreview: '',
      currentProjectText: '',
      currentProjectTitle: '',
      scrollOverflow: false,
      context: 0,
      currentImg: null,
      imagesAbout: [
        'assets/renders/compressed/1.png',
        'assets/renders/compressed/2.png',
        'assets/renders/compressed/3.png',
        'assets/renders/compressed/4.png',
        'assets/renders/compressed/5.png',
        'assets/renders/compressed/6.png',
        'assets/renders/compressed/7.png',
        'assets/renders/compressed/8.png',
        'assets/renders/compressed/10.png',
        'assets/renders/compressed/11.png',
        'assets/renders/compressed/12.png',
        'assets/renders/compressed/13.png',
        'assets/renders/compressed/14.png',
        'assets/renders/compressed/15.png',
        'assets/renders/compressed/16.png',
        'assets/renders/compressed/17.png',
        'assets/renders/compressed/18.png',
      ],
      galleryImages: [
        'https://firebasestorage.googleapis.com/v0/b/podular-f5648.appspot.com/o/1948920_orig.jpg?alt=media&token=1fcb20aa-5605-49bc-8810-79d4f1a55ae2',
        'https://firebasestorage.googleapis.com/v0/b/podular-f5648.appspot.com/o/20151001-130856_orig.jpg?alt=media&token=a8109253-cad9-4129-a2ae-c828e5aa2ca0',
        'https://firebasestorage.googleapis.com/v0/b/podular-f5648.appspot.com/o/2063890_orig.jpg?alt=media&token=69fdf29d-7674-440c-9278-4a42c388ddb5',
        'https://firebasestorage.googleapis.com/v0/b/podular-f5648.appspot.com/o/277290_orig.jpg?alt=media&token=83887fe8-26b4-4c0a-bf12-652fcfff1ae0',
        'https://firebasestorage.googleapis.com/v0/b/podular-f5648.appspot.com/o/3670961_orig.jpg?alt=media&token=16660392-e667-47b2-a5d4-d3db48ebff9a',
        'https://firebasestorage.googleapis.com/v0/b/podular-f5648.appspot.com/o/420186_orig.jpg?alt=media&token=b455e9d8-f75f-4b78-814e-73e7f4fcfbe6',
        'https://firebasestorage.googleapis.com/v0/b/podular-f5648.appspot.com/o/4728590_orig.jpg?alt=media&token=4ea42be1-0ff7-4529-8cc3-1556370d6eda',
        'https://firebasestorage.googleapis.com/v0/b/podular-f5648.appspot.com/o/5149153_orig.jpg?alt=media&token=5d6e3bbc-ed59-4618-8dfe-491e55614d07',
        'https://firebasestorage.googleapis.com/v0/b/podular-f5648.appspot.com/o/8327397_orig.jpg?alt=media&token=4d63903f-117f-451b-9b51-0d123b10c8a9',
        'https://firebasestorage.googleapis.com/v0/b/podular-f5648.appspot.com/o/8618218_orig.jpg?alt=media&token=2f4e81fa-f19a-49e3-b057-b1505846011b',
        'https://firebasestorage.googleapis.com/v0/b/podular-f5648.appspot.com/o/9826820_orig.jpg?alt=media&token=2bb4c44b-f172-4128-8672-4bcd6fc965aa',
        'https://firebasestorage.googleapis.com/v0/b/podular-f5648.appspot.com/o/9960622_orig.jpg?alt=media&token=0084eb3a-85fc-40dc-8566-dd7b19b36725',
        'https://firebasestorage.googleapis.com/v0/b/podular-f5648.appspot.com/o/cherryfilledpeach-1_1_orig.jpg?alt=media&token=9db6ec7d-3052-433c-a7f0-c3f9e26bd356',
        'https://firebasestorage.googleapis.com/v0/b/podular-f5648.appspot.com/o/img-8283_1_orig.jpg?alt=media&token=2a28d0a4-189b-4e04-97bd-db366eb1697c',
        'https://firebasestorage.googleapis.com/v0/b/podular-f5648.appspot.com/o/short-ribs-3_orig.jpg?alt=media&token=a8564112-93ee-4fc7-8fb9-c0e0f028b189'
      ]
    }
  },
  props: {
    dataRef: Object
  },
  components: {

  },
  created() {

  },
  mounted() {
    this.fetchTestData();
    this.fetchText();
    this.hideNav = true; //hide nav on landing page?
    var i = 0;

    setInterval(() => {
      this.currentImg = i;
      var aboutPath = this.imagesAbout[i];
      document.getElementById('big-image').style.backgroundImage = 'url(' +  require('@/' + aboutPath) + ')';
      console.log(aboutPath);
      if(i == this.imagesAbout.length - 1) {
        i = 0;
      } else {
        i++;
      }
    }, 5000);
  },
  computed: {

  },
  methods: {
    handleLeave(origin, destination, direction) {
      console.log('origin: ', origin);
      console.log('destination: ', destination);
      console.log('direction: ', direction);
  
      this.activeSection = destination.index;

      var video = document.getElementById('second-custom');
      //var nav = document.getElementById('fp-nav').stye.display = ( this.hideNav ? 'none' : 'block' );

      if(this.activeSection == 3) {
        console.clear();
        console.log(video);
        video.play();
        console.log('play vid');
        this.dotAppear = true;
      } else {

        this.dotAppear = false;

        /*setTimeout(() => {
          video.pause();
          video.currentTime = 0;
          video.load();
        }, 1500);*/
      }

      console.log('activeSection: ', this.activeSection);

      if(direction == 'up') {
        this.triggerUp = true;
        this.triggerDown = false;
        console.log('going up');
      }
      else {
        this.triggerDown = true;
        this.triggerUp = false;
        console.log('going down');
      }

      if(destination.index == 0) {
        console.log('on first slide');
      }
    },
    handleSlideLeave(origin, destination, direction) {
      console.clear();
      console.log('origin: ', origin);
      console.log('destination: ', destination);
      console.log('direction: ', direction);
  
      this.activeSlide = destination.index;

      console.log('activeSlide: ', this.activeSlide);

      if(direction == 'up') {
        this.triggerUp = true;
        this.triggerDown = false;
      }
      else {
        this.triggerDown = true;
        this.triggerUp = false;
      }
    },
    handleMouseEnter(i, index) {
      console.clear();
      console.log('enter' , index);
      this.hover = true;
      this.currentPreview = i.thumb;
      this.currentProjectText = i.description;
      this.currentProjectTitle = i.title;
      this.context = index;

      console.log(this.currentPreview);
    },
    handleMouseLeave(i, index) {
      console.log('left');
      console.log('leave', index);

      this.hover = false;

      /*if(!this.modalActive) {}*/
    },
    handleScroll() {
      //console.log(scrollY);
      //this.bannerOffset = scrollY;
    },
    move(section) {
      this.$refs.fullpage.api.moveTo(section)
    },
    moveRight() {
      this.$refs.fullpage.api.moveSlideRight();
    },
    moveLeft() {
      this.$refs.fullpage.api.moveSlideLeft();
    },
    goDown() {
      this.move(2);
    },
    enterOn(section, context) {
      if(context == 'default') {
        return ( this.activeSection == (section - 1) ? 'enter' : 'stage-in' ) // TODO: adjust for up and down transitions
      } else if(context == 'delay') {
        return ( this.activeSection == (section - 1) ? 'delay-enter' : 'delay-stage' )
      } else if(context == 'big-image') {
        return ( this.activeSection == (section - 1) ? 'big-image-enter' : 'big-image-stage' )
      } else if(context == 'big-image2') {
        return ( this.activeSection == (section - 1) ? 'big-image2-enter' : 'big-image2-stage' )
      } else if(context == 'big-image3') {
        return ( this.activeSection == (section - 1) ? 'big-image3-enter' : 'big-image3-stage' )
      } else if(context == 'wave') {
        return ( this.activeSection == (section - 1) ? 'wave-enter' : 'wave-stage' )
      } else if(context == 'arrows') {
        return ( this.activeSection == (section - 1) ? 'arrows-enter' : 'arrows-stage' )
      } else if(context == 'custom-video') {
        return ( this.activeSection == (section - 1) ? 'custom-vid-enter' : 'custom-vid-stage' )
      } else {
        console.log('invalid context');
      }
    },
    enterOnSlide(slide, context) {
      if(context == 'default') {
        return ( this.activeSlide == slide ? 'enter' : 'stage-in' )
      }
    },
    dothething() {
      console.log('doing the thing');
    },
    fetchTestData() {
      var fireRef = firebase.firestore();
      fireRef.collection("test-data").where("age","==", 23).get().then((docs) => {
        docs.forEach((doc) => {
          this.name = doc.data().name;
        });
      });
    },
    fetchText() {
      var fireRef = firebase.firestore();
      fireRef.collection("text").get().then((docs) => {
        docs.forEach((doc) => {
          this.text.about = doc.data().about;
          this.text.about2 = doc.data().about2
        });
        //console.clear();
        console.log(this.text.about);
      });
    },
    submitPasscode() {
      console.log('pascode submitted');
      if(this.passcode == 'caf3b31Laz!') {
        console.log('passcode correct!');
        this.modalContext = 'showroom';
        this.modalActive = true;
        console.log(this.modalContext);
      } else {
        alert('Passcode is incorrect');
      }
    },
    handleGalleryItemClick(context) {
      this.modalContext = 'gallery';
      this.modalActive = true;
      this.currentModalImage = context;
    },
    hovering(context) {
      console.log('hovering', context);
      this.hoveringV[context] = true;
      console.log(this.hoveringV);
      console.log(this.hoveringV[context]);
      this.$forceUpdate();
    },
    notHovering(context) {
      this.hoveringV[context] = false;
    }
  }
}
</script>

<template>
  <div class="home">

    <div :class="( modalActive ? 'modal-active' : 'modal-inactive' )" class="modal-container">
      <div @click="() => { modalActive = false; }" class="exit-button hoverable">X</div>
      <div class="modal">
        <iframe class="iframe" v-if="modalContext == 'showroom'" src="https://wix.viar.live/embed/tour/tdodwm" width="400px" height="400px"></iframe>
        <img v-if="modalContext == 'gallery'" :src="currentModalImage" style="border-radius: 12px" height="700px"/>
      </div>
    </div>

    <!-- optional top layer for modals & such -->
    <!--div class="top-layer">
      <div :class="( smallEnter ? 'enter' : 'stage-left' )" class="modal-small">This is some text</div>
      <div :class="( bigEnter ? 'enter' : 'stage-left' )" class="modal-big"></div>
    </div-->

    <!-- social media buttons -->
    <div class="soc-container">
      <a href="https://www.facebook.com/Caf%C3%A9Bellas-LLC-1664700527089434/" target="_blank"><div class="hoverable soc-button fb"></div></a>
      <a href="https://www.instagram.com/cafebellas/" target="_blank"><div class="hoverable soc-button insta"></div></a>
      <a href="https://twitter.com/cafebellas" target="_blank"><div class="hoverable soc-button tw"></div></a>
      <a href="https://www.linkedin.com/company/cafebellas-of-illinois/" target="_blank"><div class="hoverable soc-button li"></div></a>
    </div>

    <!-- optional wave -->
    <!--div :style="'left:' + waveOffset" :class="(waveLeft ? 'wave-left' : 'wave-right' )" class="wave-panel">
        <div class="wave"></div>
    </div-->

    <!-- navigation -->
    <div id="nav" class="hoverable">
      <div class="link-list hoverable">

      </div>
    </div>

    <!-- main (fullpage.js; TODO: Configuure locomotive scroll) -->
    <full-page ref="fullpage" :options="options" id="fullpage">

      <!-- Section 1 (landing page) -->
      <section class="section landing">
        <div class="landing-container">
          <div class="video-container">
            <video loop muted data-keepplaying id="landing-video">
              <source src="../assets/videos/landing-compressed.mp4" type="video/mp4">
              <!--source src="../assets/videos/landing-reel.ogg" type="video/ogg"-->
            </video>
          </div>
          <div class="logo" :class="enterOn(1, 'default')"></div>
          <!--div class="button-container">
            <div @click="move(2)" class="landing-button">Learn More</div>
            <div @click="move(3)" class="landing-button secondary">Donate</div>
          </div-->
          <div @click="goDown" class="arrows hoverable"></div>
        </div>
      </section>
      
      <!-- About -->
      <section style="background: black" class="section">
        <div class="slide">
          <div class="page-container">
            <div class="about-container">
              <div class="about-text">
                <div class="about-text-inner">
                  <p :class="enterOn(2, 'default')">{{ text.about }}</p>
                  <div class="button-container">
                    <div @click="move(3)" class="arrows hoverable" :class="enterOn(2, 'arrows')"></div>
                    <div class="slide-button hoverable" @click="moveRight()"><div class="arrows arrows2 hoverable"></div></div>
                  </div>
                </div>
              </div>
              <div id="big-image" class="image-slides full-image" :class="enterOn(2, 'big-image')">
                <div class="wave-container"><div class="lil-wave" :class="enterOn(2, 'wave')"></div></div>
              </div>
            </div>
          </div>        
        </div>
        <div style="background: black" class="slide">
          <div class="page-container">
            <!-- GRID -->

            <div class="slide-button back hoverable" @click="moveLeft()"><div class="arrows arrows2 hoverable"></div></div>

            <div :class="( modalActive ? 'modal-active' : 'modal-inactive' )" class="gallery-modal-container">
              <div class="gallery-modal">Image not found</div>
            </div>

            <div class="grid"><!-- animates first 11 items rendered -->
              <div 
                v-for="(i, index) in galleryImages" 
                :key="i.index"
                :id="'galler-img' + index" 
                :style="'background-image: url(' + i + ')'"  
                class="gal-item hoverable"
                :class="enterOnSlide(0, 'default')"
                @click="handleGalleryItemClick(i)"
              ></div>
            </div>

          </div>        
        </div>
      </section>

      <!-- Modular pods -->
      <section style="background: black" class="section">
        <div class="slide">
          <div class="page-container">
            <div class="about-container">
              <div class="about-text">
                <div class="about-text-inner">
                  <p :class="enterOn(3, 'delay')">{{ text.about2 }}</p>
                  <div @click="move(4)" class="arrows hoverable" :class="enterOn(3, 'arrows')"></div>
                </div>
              </div>
              <div id="big-image2" class="image-slides full-image" :class="enterOn(3, 'big-image2')">
                <div class="wave-container"><div class="lil-wave" :class="enterOn(3, 'wave')"></div></div>
              </div>
            </div>
          </div>        
        </div>
      </section>

      <!-- Customization -->
      <section style="background: black" class="section">
        <div style="background: black" class="slide">
          <div class="page-container">
            <div class="clickable-container">
              <div v-if="dotAppear" class="dot hoverable dot1" @mouseover="hovering(0)" @mouseleave="hoveringV[0] = false; $forceUpdate();">
                <div v-if="hoveringV[0]" class="hover-box">
                  <h3>Outer shell</h3>
                  <p>Can come in multiple finishes, including white, black, or gun-metal grey, and any RGB color in a glossy or matte coat.</p>
                </div>
              </div>
              <div v-if="dotAppear" class="dot hoverable dot2" @mouseover="hovering(1)" @mouseleave="hoveringV[1] = false; $forceUpdate();">
                <div v-if="hoveringV[1]" class="hover-box">
                  <h3>LED Light Shelf</h3>
                  <p>Built-in multi-color and remote controlable LEDs bright enough to light up your Podular Unit at night and make your services shine even in the day!</p>
                </div>
              </div>
              <div v-if="dotAppear" class="dot hoverable dot3" @mouseover="hovering(2)" @mouseleave="hoveringV[2] = false; $forceUpdate();">
                <div v-if="hoveringV[2]" class="hover-box">
                  <h3>Pod Structural Framing</h3>
                  <p>Specially designed and engineered to handle all of your service loads and provide a sturdy, stable working enviroment!</p>
                </div>
              </div>
              <div v-if="dotAppear" class="dot hoverable dot4" @mouseover="hovering(3)" @mouseleave="hoveringV[3] = false; $forceUpdate();">
                <div v-if="hoveringV[3]" class="hover-box">
                  <h3>Interior Shelf</h3>
                  <p>Simple and customizable shelving that can be coated in a number of material finishes to accomodate your needs!</p>
                </div>
              </div>
            </div>
            <div class="video-container" :class="enterOn(4, 'custom-video')">
              <video muted class="custom-video" id="second-custom">
                <source src="../assets/videos/custom.mp4" type="video/mp4">
              </video>
            </div>
          </div>        
        </div>
      </section>

      <!-- Showroom -->
      <section class="section">
        <div style="background: black" class="slide">
          <div class="page-container">
            <div class="passcode-input-container" :class="enterOn(5, 'default')">
              <div class="showroom-text"></div>
              <div class="input-container">
                <input type="password" placeholder="Input passcode" v-model="passcode">
                <div class="submit hoverable" @click="submitPasscode()" @keyup.enter="submitPasscode()"><b>></b></div>
              </div>
            </div>
          </div>        
        </div>
      </section>

      <!-- Contact -->
      <section style="background: black" class="section">
        <div class="page-container contact">
          <div class="bio-img"></div>
          <h1>Jasna Ostojich</h1>
          <P>Founder & Executive President</P>
          <p>info@cafebellas.com</p>
          <p>847.922.0061</p>
        </div>
      </section>

    </full-page>
  </div>
</template>

<style lang="scss" scoped>
@import '../assets/styles/global';

.contact {
  flex-direction: column;

  p, h1 {
    line-height: 0 !important;
    text-align: left !important;
  }
}

.bio-img {
  background-size: contain;
  background-position: center;
  background-repeat: no-repeat;
  background-image: url('../assets/contact.png');
  border-radius: 100%;
  height: 120px;
  width: 120px;
  margin-bottom: 24px;
}

.hover-box {
  background: white;
 //height: 100px;
  width: 300px;
  border-radius: 12px;
  margin-top: 36px;
  animation: flyin ease forwards 300ms;
  color: black;
  text-align: left;
  padding: 12px 24px 12px 24px;

  p {
    line-height: 1.5;
  }


  h3 {
    margin-bottom: 0px;
    font-size: 18px;
  }
}

@keyframes flyin {
  from {
    opacity: 0;
    transform: scale(0.8);
  } to {
    opacity: 1;
    transform: scale(1);
  }
}

.dot1 {
  top: 25vh;
  left: 45vh;
  animation: dot-enter 1s ease forwards 3s;
}

.dot2 {
  top: 49vh;
  left: 72vh;
  animation: dot-enter 1s ease forwards 3.2s;
}

.dot3 {
  top: 26vh;
  left: 78vw;
  animation: dot-enter 1s ease forwards 3.4s;
}

.dot4 {
  top: 76vh;
  left: 68.5vw;
  transition: 300ms;
  animation: dot-enter 1s ease forwards 3.6s;
}

@keyframes flux {
  from {
    transform: scale(0.8);
  } to {
    transform: scale(1);
  }
}

@keyframes dot-enter {
  from {
    opacity: 0;
    transform: scale(0.2);
  } to {
    opacity: 0.3;
    transform: scale(1);
  }
}

.dot {
 position: absolute;
 height: 24px;
 width: 24px;
 background: white;
 border-radius: 100%;
 opacity: 0;
 transition: 300ms;
 cursor: default !important;
 //animation: flux 2s ease forwards infinite;

 &:hover {
   opacity: 1 !important;
 }
}

.clickable-container {
  //background:rgba(red, 0.3);
  position: absolute;
  height: 100vh;
  width: 100vw;
  z-index: 9999;
}

#big-image2 {
  background-image: url('../assets/renders/animations/sketch.gif');
  background-size: cover;
  background-position: center;
  background-repeat: no-repeat; 
}

.custom-video {
  height: 100vh;
  width: 100vw;
  z-index: -1 !important;
  transform: scale(1.15);
  filter: brightness(0.5);
  border-radius: 48px !important;
}

.iframe {
  border: none;
  height: 70vh;
  width: 70vw;
  border-radius: 12px;
}

.exit-button {
  position: absolute;
  top: 64px;
  right: 64px;
  color: white;
  height: 48px;
  width: 48px;
  line-height: 48px;
  text-align: center;
  font-weight: bold;
  transition: 300ms;
  font-size: 24px;
  border-radius: 100%;

  &:hover {
    background: white;
    color: black;
  }
}

.modal div {
  border-radius: 12px;
  background-repeat: no-repeat;
  background-size: cover;
  background-position: center;
}

.modal-inactive {
  opacity: 0;

  .modal {
    transform: scale(0.8);
  }
}

.modal-active {
  opacity: 1;
  border-radius: 12px;

  .modal {
    transform: scale(1);
  }
}

.modal-container {
  //background: red;
  height: 100vh;
  width: 100%;
  position: absolute;
  top: 0px;
  z-index: 9999;
  display: flex;
  justify-content: center;
  align-items: center;

  .modal {
    background: gray;
    border-radius: 12px;
  }
}

.input-container {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 24px;
}

.submit {
  background: white;
  border-radius: 0px 12px 12px 0px;
  padding-top: 12px;
  padding-bottom: 12px;
  width: 60px;
  color: black;
  border: 2px solid white;
  font-size: 14px;
  cursor: pointer;
  transition: 300ms;

  &:hover {
    filter: brightness(0.4);
  }
}

.passcode-input-container {
  flex-direction: column;

  input {
    background: rgba(white, 0.1);
    border-radius: 12px 0px 0px 12px;
    border: none;
    padding: 12px 12px 12px 24px;
    text-align: center;
    color: white;
    border: 2px solid rgba(white, 0.1);
    transition: 300ms;
    font-size: 14px;

    &:hover {
      border: 2px white solid;
    }
  }
}

//caf3b31Laz!

.showroom-text {
  background-image: url('../assets/showroom-text.png');
  background-position: center;
  background-size: contain;
  background-repeat: no-repeat;
  height: 80px;
  width: 500px;
}

.grid {
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  grid-gap: 24px;
  height: 85vh;
  width: 60vw;
  //background: red;
  position: absolute;
  overflow: auto;
  bottom: 0px;
  padding-right: 24px;
}

.gal-item {
  height: 300px;
  background: gray;
  background-size: 200%;
  background-position: center;
  background-repeat: no-repeat;
  border-radius: 8px;
  cursor: zoom;
  transition: 2s;

  &:nth-child(1) {
    transition-delay: 1.4s;  
  }

  &:nth-child(2) {
    transition-delay: 1.2s;  
  }

  &:nth-child(3) {
    transition-delay: 1s;  
  }

  &:nth-child(4) {
    transition-delay: 1.6s;  
  }

  &:nth-child(5) {
    transition-delay: 1.4s;  
  }

  &:nth-child(6) {
    transition-delay: 1.2s;  
  }

  &:nth-child(7) {
    transition-delay: 1.8s;  
  }

  &:nth-child(8) {
    transition-delay: 1.6s;  
  }

  &:nth-child(9) {
    transition-delay: 1.4s;  
  }

  &:hover {
    //background-size: 240%;
  }
}

.back {
  right: 100px !important;
  transform: rotate(180deg) !important;
  background: black !important;
}

.slide-button {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  //margin: auto;
  height: 80px;
  width: 80px;
  background: rgba(0,0,0,0.6);
  border-radius: 100%;
  z-index: 9999;
  cursor: pointer;
  bottom: 36px;
  right: 103vw;
  transition: 300ms;

  &:hover {
    transform: scale(0.8);
    background: rgba(0,0,0,1);
  }
}

.arrow-button {
  background: black;
  position: absolute;
  //transform: rotate(270deg) scale(1) !important;
  cursor: pointer !important;
  z-index: 999;
  height: 80px;
  width: 80px;
  padding: 12px;
  border-radius: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  //transform: rotate(90deg) !important;
}

.top-layer {
  background: #127894;
  width: 100vw;
  height: 100vh;
  margin: 0px;
  padding: 0px;
  position: absolute;
  top: 0px;
  z-index:0;
  cursor: pointer;
}

.arrows2 {
  margin-right: 12px !important;
  margin-top: -8px !important;
  transform: rotate(270deg) scale(0.4) !important;
}

.lil-wave {
  background-image: $wave;
  background-size: cover;
  //background-position: right;
  //background-position-y: 170% !important;
  background-repeat: repeat-y;
  height: 100vh;
  width: 180px !important;
  transform: scale(2) translateX(33px);
}

.lil-wave2 {
  background-image: $wave2;
  background-size: cover;
  //background-position: right;
  //background-position-y: 170% !important;
  background-repeat: repeat-y;
  height: 100vh;
  width: 180px !important;
  transform: scale(2) translateX(33px);
}

.new-wave {
  background-image: $wave;
  //background: red;
  height: 100vh;
  width: 100px;
  //justify-self: flex-end;
  //background: orange;
  transform: $flip scale(6);
  background-size: contain;
  background-position-y: 160%;
  background-repeat: repeat-y;
  z-index: 3 !important;
  transition: $drag;
}

#landing-video {
  position: absolute;
  width: 100%;
  height: 100vh;
  top: 0px;
  left: 0px;
  margin: 0px;
  z-index: 0;
  filter: brightness(0.6);
  transform: scale(1.3);
}

#big-image {
  transition: 3s !important;
}

.insta {
  background-image: url("../assets/i.png");
  background-size: 125% !important;
  filter: invert(1);
}

.fb {
  background-image: url("../assets/f.png");
  filter: invert(1);
  background-size: 120% !important;
}

.li {
  background-image: url("../assets/li.png");
  filter: invert(1);
  background-size: 105% !important;
  transform: scale(1.2);
}

.tw {
  background-image: url("../assets/t.png");
  filter: invert(1);
}

.about-container {
  display: flex;
  width: 100%;
}

.clock {
  background-image: $clock;
  margin-bottom: 24px;
}

.about-text {
  width: 100%;
  text-align: left;
  line-height: 2;
  display: flex;
  align-items: center;
  justify-content: center;
  //background-image: url('../assets/wave.svg');
  background-position: center;
  background-size: cover;
  background-repeat: no-repeat;
  //background: #444;

  .about-text-inner {
    width: 310px;
    z-index: 4;
    margin-left: 155px;
  }
}


.image-slides {
  height: 100vh;
  background-image: url('../assets/renders/BAR.png');
  z-index: -1;
}

.donation-container {
  border-radius: $rad;
  background: rgba(white, 0.05);
  width: 60%;
  height: 60%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  color: white;
}

.secondary {
  background: transparent !important;
  color: white !important;
}

.button-container {
  display: flex;
}

.subtitle {
  width: 350px;
  line-height: 2;
  opacity: 0.4;
}

.logo {
  background-image: $logoText;
  background-position: center;
  background-size: contain;
  background-repeat: no-repeat;
  width: 500px;
  height: 100px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  padding: 0px;
}

$buttonHeight: 50px;

.landing-button {
  color: black;
  background: white;
  border-radius: $buttonHeight;
  height: $buttonHeight;;
  width: 120px;
  text-align: center;
  padding: 0px;
  line-height: $buttonHeight;
  border: 2px solid white;
  font-weight: bold;
  margin-top: 12px;
  transition: 200ms;
  cursor: pointer;
  margin-left: 8px;
  margin-right: 8px;

  &:hover {
    transform: scale(0.9);
  }
}

.page-container {
  height: 100vh;
  width: 100%;
  margin: 0px;
  padding: 0px;
  display: flex;
  justify-content: center;
  align-items: center;
}

.arrows {
  position: relative;
  width: 30px;
  height: 30px;
  transform: scale(0.5);
  margin-top: 36px;
}

.arrows:before {
  content: '';
  position: absolute;
  width: 100%;
  height: 100%;
  border-left: 26.66667px solid rgba(0, 0, 0, 0.7);
  border-bottom: 26.66667px solid rgba(0, 0, 0, 0.7);
  transform: translate(26.66667px, 106.66667px) rotate(-45deg);
  animation: arrows 3s linear infinite;
}

.arrows:after {
  content: '';
  position: absolute;
  width: 100%;
  height: 100%;
  border-left: 26.66667px solid rgba(0, 0, 0, 0.7);
  border-bottom: 26.66667px solid rgba(0, 0, 0, 0.7);
  transform: translate(53.33333px, 0px) rotate(-45deg);
  animation: arrows 3s linear infinite -1.5s;
}

@keyframes arrows {
  0% {
    border-left: 10px solid transparent;
    border-bottom: 10px solid transparent;
    transform: translate(-13.33333px, -53.33333px) rotate(-45deg);
  }
  10%, 90% {
    border-left: 10px solid transparent;
    border-bottom: 10px solid transparent;
  }
  50% {
    border-left: 10px solid rgba(255, 255, 255, 0.7);
    border-bottom: 10px solid rgba(255, 255, 255, 0.7);
    transform: translate(-13.33333px, 0px) rotate(-45deg);
  }
  100% {
    border-left: 26.66667px solid transparent;
    border-bottom: 26.66667px solid transparent;
    transform: translate(-13.33333px, 53.33333px) rotate(-45deg);
  }
}

.top-layer {
  position: fixed;
  z-index: 99999;
  height: 100vh;
  width: 100%;
  display: flex;
  pointer-events: none;
  justify-content: center;
  align-items: center;
}

.soc-container {
  position: fixed;
  bottom: $pad;
  left: $pad;
  margin: auto;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 9999;
  //background: red;
}

.landing-container {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  position: absolute;
  margin: auto;
  left: 0px;
  right: 0px;
  top: 0px;
  bottom: 0px;

  h1 {
    font-size: 48px;
  }
}

.landing {
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.wave-panel {
  background: #181818;
  position: fixed;
  height: 100vh;
  width: 100%;
  margin: 0px;
  z-index: 9;
  transition: 2s;
  pointer-events: none;

  .wave {
    height: 100vh;
    transform: translateX(0px) scale(4);
    width: 175px;
    position: absolute;
    background-image: url('../assets/wave.svg');
    background-size: contain;
    background-position-y: 50%;
    background-repeat: repeat-y;
    transition: 2s;
  }
}

.wave-left {
  .wave {
    background-position-y: 7% !important;
    //background: rgba(yellow, 0.2);
  }
}

.wave-right {
  .wave {
    background-position-y: 50% !important;
    //background: rgba(aqua, 0.2);
  }
}

.main-title {
  //background:red;
  width: 70px;
  background-image: $logoEmblem;
  background-position: center;
  background-size: contain;
  background-repeat: no-repeat;
  height: 70px;
  margin: 42px;
}

.hoverable {
  cursor: pointer;
}

.project-text {
  font-size: 14px !important;
}

.mwm-expanded {
  background: white;
  width: auto;
  height: 70%;
  border-radius: 18px;
  min-width: 25%;
}

.mwm-inactive {
  //background: blue;
  width: 100vw;
  height: 100vh;
  position: absolute;
  padding: 0px;
  margin: 0px;
  z-index: 999;
  transition: 1s;
  display: flex;
  justify-content: center;
  align-items: center;
  opacity: 0;
  pointer-events: none;
  
  img {
    transition: 600ms;
    transform: scale(0.8);
    opacity: 0;
  }
}

.mvm-active {
  background: rgba(black, 0.8);
  width: 100vw !important;
  height: 100vh !important;
  position: absolute;
  padding: 0px;
  margin: 0px;
  z-index: 999;
  transition: 1s;
  display: flex;
  justify-content: center;
  align-items: center;
  opacity: 1;
  transition-delay: 150ms;

  img {
    transition: 600ms;
    transform: scale(1);
  }
}

.modal-title {
  font-size: 36px;
}

.image-list {
  list-style: none;
  padding: 0px;
  margin: 0px;
}

.modal-text {
  //@extendanimation: flyleft 1s ease forwards 3s;
  width:300px;
  text-align: left;
}

.modal {
  display: flex;
  justify-content: center;
  align-items: center;
  //background: purple;
  transition: 1s;
}

#nav {
  position: fixed;
  z-index: 9999;

  h1 {
    padding-left: 18px;
  }

  ul {
    list-style: none;
    text-align: left;
    font-family: 'Inconsolata', monospace;

    li {
      margin-top: 6px;
      font-size: 18px;

      &:hover {
        span {
          opacity: 1;
          padding-right: 8px;
        }
      }

      span {
        opacity: 0;
        margin-right: 12px;
        transition: 300ms;
        font-weight: bold;
      }
    }
  }

  a {
    color: white;
    text-decoration: none;
    opacity: 0.7;
    //cursor: none;

    &.router-link-exact-active {
      color: white;
      opacity: 1;
      font-weight: bold;
    }
  }
}

//grid flyin animation
.grid-item {
  background: #555;
  height: 250px;
  transition: 1.2s;

  &:hover {
    //transform: scale(0.95);
    //filter:hue-rotate(145deg);
  }

  &:nth-child(1) {
    transition-delay: 1.4s;  
  }

  &:nth-child(2) {
    transition-delay: 1.2s;  
  }

  &:nth-child(3) {
    transition-delay: 1s;  
  }

  &:nth-child(4) {
    transition-delay: 1.6s;  
  }

  &:nth-child(5) {
    transition-delay: 1.4s;  
  }

  &:nth-child(6) {
    transition-delay: 1.2s;  
  }

  &:nth-child(7) {
    transition-delay: 1.8s;  
  }

  &:nth-child(8) {
    transition-delay: 1.6s;  
  }

  &:nth-child(9) {
    transition-delay: 1.4s;  
  }
}

.modal-active {
  background: black;
  width: 100vw;
  height: 100vh;
  position: absolute;
  z-index: 999;
  transition: 600ms;
  transition-delay: 600ms;
  margin: auto;
  left: 0px;
  top: 0px;
  overflow: auto;
}

.modal-inactive {
  background: black;
  opacity: 0;
  width: 100vw;
  height: 100vh;
  position: absolute;
  z-index: 999;
  transition: 600ms;
  margin: auto;
  left: 0px;
  top: 0px;
  overflow: auto;
  pointer-events: none;
}

.blur {
  filter: blur(24px);
}

.clear {
  filter: blur(0px);
}

@media only screen and (max-width: 900px) {

}
</style>