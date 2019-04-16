<template>
  <div id="mySwiper">
    <div class="swiper-container">
      <div class="swiper-wrapper" ref="wrapper">
        <div class="swiper-slide"><div class="slide-1">1</div></div>
        <div class="swiper-slide"><div class="slide-2">2</div></div>
        <div class="swiper-slide"><div class="slide-3">3</div></div>
        <div class="swiper-slide"><div class="slide-4">4</div></div>
        <div class="swiper-slide"><div class="slide-5">5</div></div>
      </div>
    </div>
    <div class="swiper-pagination"></div>
  </div>
</template>

<script>
import Swiper from 'swiper'
import 'swiper/dist/css/swiper.css'

export default {
  data() {
    return {
      m: null
    }
  },
  mounted() {
    let slideNodes = this.$refs.wrapper.children
    let count = slideNodes.length
    let slideW = slideNodes[0].offsetWidth

    new Swiper('#mySwiper .swiper-container', {
      watchSlidesProgress: true,
      slidesPerView: 'auto',
      // centeredSlides: true,
      loop: true,
      loopedSlides: count,
      autoplay: {
        disableOnInteraction: false,
      },
      pagination: {
        el: '.swiper-pagination',
      },
      on: {
        progress: function (progress) {
          // 每个slide水平偏移参考宽度
          var w = slideW * 0.92

          for (var i = 0; i < this.slides.length; i++) {
            var slide = this.slides.eq(i);
            var slideProgress = this.slides[i].progress;
            var sp = Math.abs(slideProgress)
            var scale = 1 - sp / 9.5;
            var translateX = slideProgress * w + 'px';
            var zIndex = 999 - Math.abs(Math.round(10 * slideProgress));
            // console.log('sp: ', Math.round(slideProgress))
            if (Math.round(slideProgress) > 0) {
              slide.transform('translateX(-' + slideW + 'px)');
            } else {
              slide.transform('translateX(' + translateX + ') scale(' + scale + ')');
            }
            // slide.transform('translateX(' + translateX + ') scale(' + scale + ')');
            slide.css('zIndex', zIndex);
            if (Math.round(slideProgress) === 0) {
              slide.css('opacity', 1);
            } else {
              slide.css('opacity', 1 - sp / (sp + 1));
            }
            // slide.css('opacity', 1 - sp / (sp + 1));

            if (count > 4) {
              if (Math.round(sp) >= Math.ceil(count / 2)) {
                slide.css('opacity', 0);
              }
            } else {
              if (Math.round(sp) > Math.ceil(count / 2)) {
                slide.css('opacity', 0);
              }
            }
          }
        },
        setTransition: function (transition) {
          for (var i = 0; i < this.slides.length; i++) {
            var slide = this.slides.eq(i)
            slide.transition(transition);
          }
        }
      }
    })
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
#mySwiper {
	position: relative;
  /* margin: 60px auto; */
  /* border: 1px solid; */
}

#mySwiper .swiper-container {
	padding-bottom: 60px;
}

#mySwiper  .swiper-slide {
	width: 94%;
	height: 200px;
	background: #fff;
    /* border:1px solid #c2c2c2; */
}

#mySwiper .swiper-pagination {
	width: 100%;
	bottom: 20px;
}

#mySwiper .swiper-pagination-bullets .swiper-pagination-bullet {
	margin: 0 5px;
	border: 3px solid #fff;
	background-color: #d5d5d5;
	width: 10px;
	height: 10px;
	opacity: 1;
}

#mySwiper .swiper-pagination-bullets .swiper-pagination-bullet-active {
	border: 3px solid #00aadc;
	background-color: #fff;
}

.swiper-slide > div {
  height: 100%;
  text-align: right;
  color: #fff;
  font-weight: bold;
  border-radius: 6px;
}
.slide-1 {
  background: red;
}
.slide-2 {
  background: #816130;
}
.slide-3 {
  background: rgb(91, 173, 146);
}
.slide-4 {
  background: #cd3;
}
.slide-5 {
  background: #daf;
}
</style>
