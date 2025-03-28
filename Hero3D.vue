<template>
    <div class="hero" :style="{ '--bgImage': `url(${imageUrl})` }">
      <div class="text">
        <h1>
          {{ title1 }}
          <span class="titleSpan">
            {{ titleSpan }}
          </span> 
          {{ title2 }}
        </h1>
        <div class="line">
          <span>
            {{ spanText }}
          </span>
          <div class="buttons">
            <Button2
              :text="buttonAttributes.text"
              :icon=" buttonAttributes.icon"
              :hoverColor=" buttonAttributes.hoverColor"
              :backgroundColor=" buttonAttributes.backgroundColor"
              :link=" buttonAttributes.link"
            />
          </div>
      </div>
      </div>
      <spline-viewer :url="splineUrl" v-if="splineUrl"></spline-viewer>
    </div>
</template>

<script>
import Button2 from './Button2.vue';
export default {
  name: 'Hero3D',
  props: {
    title1: String,
    titleSpan: String,
    title2: String,
    spanText: String,
    backgroundData: {type: Object, required: false},
    buttonAttributes: { type: Object, required: true },
    splineUrl: {type: String, required: false}
  },
  components: {
    Button2
  },
  data(){
    return {
      buttonHoverColor: this.buttonAttributes.hoverColor,
      imageUrl: (this.backgroundData && this.backgroundData.imageUrl) || '',
      backgroundPositionX: (this.backgroundData && this.backgroundData.backgroundPositionX) || 'center',
    }
  }
}
  </script>

<style lang="scss">
    @import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Josefin+Sans:ital,wght@0,100..700;1,100..700&family=Roboto:wght@900&display=swap');
    
    .hero{
        width: 100%;
        height: 100vh; 
        overflow: hidden;
        font-family: #{'Bebas Neue'};
        position: relative;

        &::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: var(--bgImage) no-repeat;
            background-position: v-bind(backgroundPositionX) center;
            background-attachment: fixed; 
            background-size: cover;
            filter: saturate(1.25) brightness(0.7); 
            z-index: 0; 
            pointer-events: none; 
        }

        @media screen and (min-width: 768px) {
            .text h1{
              transform: scale(1.25);
              .titleSpan{
                font-size: inherit !important;
              }
            }
        }

        @media screen and (min-width: 1700px) {
            height: 80vh;

            &::before {
              background-position-x: 70%;
              background-position-y: -1vh;
            }
        }

        @media screen and (min-width: 1900px) {
            height: 60vh;

            &::before {
              background-position-x: 70%;
              background-position-y: -10vh;
            }
        }

        @media screen and (min-width: 2200px) {
            height: 40vh;

            &::before {
              background-position-x: 70%;
              background-position-y: -15vh;
            }
        }

        @media screen and (min-width: 2500px) {
            height: 30vh;

            &::before {
              background-position-x: 70%;
              background-position-y: -15vh;
            }
        }

        &::after {
            content: '';
            position: absolute;
            width: 100%;
            height: 100%;
            top: 0;
            left: 0;
            box-shadow: inset 5px 5px 50px v-bind(buttonHoverColor), inset -5px -5px 50px v-bind(buttonHoverColor);
            filter: blur(10px);
            z-index: 1; /* On top of the background image */
            pointer-events: none; /* Ensure it's not interactive */
        }
    }
    spline-viewer{
        width: 100%;
        height: 100%;
        position: absolute;
        top: 0;
        left: 0;
        overflow: hidden;
        transform: scale(1.35);
        z-index: 1;
        opacity: 0.5;
    }

    .text{
      width: min(90%, 90em);
      color: white;
      z-index: 10;
      display: flex;
      position: relative;
      flex-direction: column; 
      justify-content: center;
      text-align: center;
      position: absolute;
      bottom: 0;
      left: 50%;
      transform: translate(-50%, 0);
      padding: clamp(1.25em, 10vw, 5em);

      h1 {
        font-size: clamp(3em, 5.5vw + 2em, 10em);
        word-wrap: break-word;
        line-height: 1em;

        .titleSpan{
          color: v-bind(buttonHoverColor);
          font-size: clamp(1.25em, 5vw + 1em, 5em);
        }
      }

      .line{
        span{
          font-size: clamp(1.25em, 10vw - 3em, 1.5em);
        }

        .buttons{
          display: flex;
          justify-content: center;
          align-items: center;
          gap: 1em;
          margin-top: 1em;
        }
      }
    }
</style>