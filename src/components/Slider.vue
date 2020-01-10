<template>
  <div class="slider-wrapper">
	
	<button v-on:click='prevSlide'>Prev</button>
	<button v-on:click='nextSlide'>Next</button>
	<button v-on:click='openSlide(3)'>Open slide 3</button>
	
	<div class='slider js-slider' ref="slider">
		<div class="slider__body" :style='{left: sliderOffsetLeft + "px"}'>
			<div class="slider__slide js-slide" v-for='(slide, index) in sliderList' :key="index" :style='"background-image: url(" + slide.img + ")"' ref="slide"></div>
		</div>
	</div>
	
</div>
</template>

<script>
export default {
  data () {
		return {
			// Всего слайдов
      sliderAllCount: 0,
      // Номер активного слайда
      sliderActive: 1,
      // Отступ тела со слайдами в контейнере
      sliderOffsetLeft: 0,
      // Шаг одного слайда = его длина
      sliderOffsetStep: 0,
      // Список изображений
      sliderList: [
        {img: 'https://dummyimage.com/wsvga'},
        {img: 'https://dummyimage.com/wsvga'},
        {img: 'https://dummyimage.com/wsvga'},
        {img: 'https://dummyimage.com/wsvga'}      
			]
		}
	},
	
	methods: {
    // Иницилизация слайдера
    initSlider: function () {
			// Получаем элементы сладера и его слайдов
      let sliderBody = this.$refs.slider
      let sliderSlidies = this.$refs.slide
			// Записываем длину одного слайда для перелистывания
      this.sliderOffsetStep = sliderBody.clientWidth
			// Общее количество слайдов для стопов
      this.sliderAllCount = sliderSlidies.length
    },

		// Открыть слайд по номеру
    openSlide: function (id) {
      if (id > 0 && id <= this.sliderAllCount) {
        this.sliderActive = id
				// Сдвигаем элемент со слайдами
        this.sliderOffsetLeft = -(this.sliderActive * this.sliderOffsetStep - this.sliderOffsetStep)
      }
    },

    // Следующий слайд
    nextSlide: function () {
      if (this.sliderActive < this.sliderAllCount) {
        this.sliderActive += 1
				this.openSlide(this.sliderActive)
      }
    },

    // Предыдущий слайд
    prevSlide: function () {
      if (this.sliderActive > 1) {
        this.sliderActive -= 1
				this.openSlide(this.sliderActive)
      }
    }
  },

  mounted () {
    this.initSlider()

    // Перенастройка слайдера при ресайзе окна
    window.addEventListener('resize', () => {
      this.initSlider()
      this.openSlide(this.sliderActive)
    })
  }
}
</script>

<style lang="scss" scoped>
$slider-height: 400px;
$slide-width: 100%;

.slider {
	width: 100%;
	height: $slider-height;
	position: relative;
	overflow: hidden;
	
	&__body {
		min-width: auto;
		height: $slider-height;
		display: flex;
		position: relative;
		align-items: stretch;
		transition: all .5s ease;
	}
	
	&__slide {
		min-width: $slide-width;
		height: $slider-height;
		background-size: cover;
		background-position: center;
		flex: 1 100%;
	}
}
</style>