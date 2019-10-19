<template>
  <div class="slider">
    <div class="slides">
      <div
        class="slide"
        v-for="(slide, index) in sliders"
        :key="slide.position"
        :class="[slide.background, 'slide--' + (index + 1)]"
        :ref="index"
      >
        <div class="slide-content" :ref="'content'" v-if="index == 0">
          <div class="slide-content__main">
            <div class="slide-content__product">{{ slide.product }}</div>
            <div class="slide-content__name">{{ slide.name }}</div>
            <div class="slide-content__description">{{ slide.description }}</div>
            <div class="slide-content__cta btn">{{ slide.cta }}</div>
          </div>
        </div>
      </div>
      <div class="slides-current">
        {{ sliders[0].position }}
        <span class="color-grey">/ {{ slidersArr.length }}</span>
      </div>
    </div>
    <div class="slides-control__container slides-control__container--left" @click="prevSlide">
      <div class="btn--slides-control">
        <svg viewBox="0 0 16 16" xml:space="preserve">
          <path
            fill="#3D7AB6"
            d="M3.293 7.707a1 1 0 0 1 1.414-1.414L8 9.586l3.293-3.293a1 1 0 0 1 1.414 1.414L9.414 11a2 2 0 0 1-2.828 0L3.293 7.707z"
          />
        </svg>
      </div>
    </div>
    <div class="slides-control__container slides-control__container--right" @click="nextSlide">
      <div class="btn--slides-control">
        <svg viewBox="0 0 16 16" xml:space="preserve">
          <path
            fill="#3D7AB6"
            d="M3.293 7.707a1 1 0 0 1 1.414-1.414L8 9.586l3.293-3.293a1 1 0 0 1 1.414 1.414L9.414 11a2 2 0 0 1-2.828 0L3.293 7.707z"
          />
        </svg>
      </div>
    </div>
    <div class="slider__dots">
      <span
        class="slider__dot"
        v-for="slide in slidersArr.length"
        :key="slide"
        :style="{'background': sliders[0].position == slide ? '#ffdd2d' : '#dddfe0'}"
        @click="jumpToSlide(slide)"
      ></span>
    </div>
  </div>
</template>

<script>
import { TweenMax, Power3, TimelineMax } from "gsap";

export default {
  data() {
    return {
      slidersArr: [
        {
          product: "Дебетовая карта",
          name: "Tinkoff Black",
          description: "Дебетовая карта с кэшбеком до 30%",
          cta: "Оформить карту",
          background: "white",
          position: 1
        },
        {
          product: "Развлечения",
          name: "В кино дешевле с Тинькофф",
          description: "Кэшбек при покупке билетов",
          cta: "Купить билеты",
          background: "black",
          position: 2
        },
        {
          product: "Кредит наличными",
          name: "Получите кредит прямо сейчас",
          description: "Кредит до 15 млн. рублей",
          cta: "Оформить кредит",
          background: "dark-blue",
          position: 3
        },
        {
          product: "Авто кредит",
          name: "Кредит в залог авто",
          description: "Заём до 5 млн. рублей",
          cta: "Оформить кредит",
          background: "white",
          position: 4
        },
        {
          product: "Лизинг",
          name: "Возьми авто в лизинг",
          description: "Современный фиинансовый инструмент",
          cta: "Оформить лизинг",
          background: "black",
          position: 5
        }
      ]
    };
  },
  updated() {
    TweenMax.fromTo(
      this.$refs.content,
      0.32,
      { opacity: 0 },
      { opacity: 1, ease: Power3.easeOut }
    );
  },
  computed: {
    sliders() {
      return [
        ...this.slidersArr.slice(0, 4),
        this.slidersArr[this.slidersArr.length - 1]
      ];
    }
  },
  methods: {
    nextSlide() {
      const firstSlide = this.$refs[0];
      const secondSlide = this.$refs[1];
      const thirdSlide = this.$refs[2];
      let content = this.$refs.content;
      const updateSlides = this.updateSlides;

      TweenMax.to(content, 0, {
        opacity: 0
      });

      TweenMax.to(firstSlide, 0.8, {
        opacity: 0,
        ease: Power3.easeOut,
        onComplete: () => {
          TweenMax.set(firstSlide, { clearProps: "all", ease: Power3.easeOut });
        }
      });

      TweenMax.to(secondSlide, 0.7, {
        top: 0,
        width: "100%",
        opacity: 1,
        ease: Power3.easeOut,
        delay: 0.1,
        onComplete: function() {
          TweenMax.set(secondSlide, {
            clearProps: "all",
            ease: Power3.easeOut
          });
        }
      });

      TweenMax.to(thirdSlide, 0.8, {
        top: 20,
        width: "96%",
        opacity: 0.8,
        ease: Power3.easeOut,
        onComplete: () => {
          TweenMax.set(thirdSlide, { clearProps: "all", ease: Power3.easeOut });
          updateSlides("next");
        }
      });
    },
    prevSlide() {
      const firstSlide = this.$refs[0];
      const secondSlide = this.$refs[1];
      const thirdSlide = this.$refs[2];
      const fourthSlide = this.$refs[4];
      const updateSlides = this.updateSlides;
      const content = this.$refs.content;
      const t1 = new TimelineMax();
      const t2 = new TimelineMax();
      const t3 = new TimelineMax();
      const t4 = new TimelineMax();

      TweenMax.to(content, 0, {
        opacity: 0
      });

      // t1.to(firstSlide, 0, { "z-index": 3 }).to(firstSlide, 0.8, {
      //   opacity: 0.8,
      //   top: 20,
      //   width: "96%",
      //   ease: Power3.easeOut,
      //   onComplete: () => {
      //     TweenMax.set(firstSlide, 0, {
      //       clearProps: "opacity, top, width",
      //       ease: Power3.easeOut
      //     });
      //   }
      // });

      t1.to(firstSlide, 0, { "z-index": 3 })
        .to(firstSlide, 0.8, {
          opacity: 0.8,
          top: 20,
          width: "96%",
          ease: Power3.easeOut
        })
        .to(firstSlide, 0, {
          clearProps: "all",
          ease: Power3.easeOut
        });

      t2.to(secondSlide, 0, { "z-index": 2 })
        .to(secondSlide, 0.7, {
          width: "92%",
          opacity: 1,
          top: 40,
          ease: Power3.easeOut,
          delay: 0.1
        })
        .to(secondSlide, 0, {
          clearProps: "all",
          ease: Power3.easeOut
        });

      t3.to(fourthSlide, 0, { "z-index": 5 })
        .to(fourthSlide, 0.7, {
          opacity: 1,
          ease: Power3.easeOut,
          delay: 0.1,
          onComplete: () => {
            updateSlides("prev");
          }
        })
        .to(fourthSlide, 0, {
          clearProps: "all",
          ease: Power3.easeOut
        });

      t4.to(thirdSlide, 0.8, {
        ease: Power3.easeOut,
        opacity: 0.8
      }).to(thirdSlide, 0, {
        clearProps: "all",
        ease: Power3.easeOut
      });
    },
    updateSlides(direction) {
      const sliders = [...this.slidersArr];

      if (direction == "next") {
        let firstSlide = sliders[0];
        sliders.shift();
        sliders.push(firstSlide);
      } else {
        let lastSlide = sliders[sliders.length - 1];
        sliders.pop();
        sliders.unshift(lastSlide);
      }

      this.slidersArr = sliders;
    },
    jumpToSlide(slide) {
      const newArr = this.slidersArr.sort((a, b) => a.position - b.position);
      newArr.unshift(...newArr.splice(slide - 1));
      this.slidersArr = newArr;
    }
  }
};
</script>

<style lang="scss">
.slider {
  display: grid;
  grid-template-columns: [button-prev-start] 75px [button-prev-end slides-start] 1fr [slides-end button-next-start] 75px [button-next-end];
  grid-template-rows: 1fr 65px;
  max-width: 110rem;
  margin: 10rem auto;

  &__dots {
    grid-column: 1 / -1;
    justify-self: center;
    align-self: end;
    display: grid;
    grid-auto-flow: column;
    grid-gap: 1rem;
  }

  &__dot {
    border-radius: 50%;
    width: 0.8rem;
    height: 0.8rem;
    background: black;
    cursor: pointer;
  }
}

.slides-control__container {
  align-self: center;
  justify-self: center;
  width: 32px;
  height: 32px;
  border-radius: 50%;
  background: rgba(#eef2f7, 0.7);
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: all 0.2s ease-out;

  &:hover {
    background: #eef2f7;
  }
}

.slides-control__container--left {
  grid-column: button-prev-start / button-prev-end;
  grid-row: 1 / span 1;
  transform: rotate(90deg);
}

.slides-control__container--right {
  grid-column: button-next-start / button-next-end;
  grid-row: 1 / span 1;
  transform: rotate(-90deg);
}

.slides {
  grid-column: slides-start / slides-end;
  position: relative;
  grid-row: 1 / span 1;
  height: 35rem;

  &-current {
    font-size: 1.4rem;
    color: white;
    bottom: 3rem;
    right: 5rem;
    position: absolute;
    border-radius: 20px;
    text-align: center;
    padding: 0.8rem 1rem;
    background-color: rgba(62, 71, 87, 0.7);
    z-index: 10;
  }
}

.slide {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
  width: 100%;
  height: 100%;
  border-radius: 10px;
  margin: 0 auto;

  &--1 {
    z-index: 3;
    left: 0;
  }

  &--2 {
    z-index: 2;
    width: 96%;
    top: 20px;

    opacity: 0.8;
  }

  &--3 {
    z-index: 1;
    width: 92%;
    top: 40px;
  }

  &--4 {
    z-index: 0;
    width: 92%;
    top: 40px;
  }

  &--5 {
    opacity: 0;
    width: 100%;
  }

  &-content {
    padding: 3rem 5rem;
    line-height: 2;
    display: grid;
    height: 100%;
    grid-template-columns: 1fr 50px;

    &__main {
      display: grid;
      grid-template-rows: repeat(3, 50px) 1fr;
      align-items: center;
    }

    &__right {
      align-self: end;
      justify-self: end;
      font-size: 1.2rem;
      background: rgba(grey, 0.3);
      border-radius: 15px;
      padding: 0.3rem 1rem;
    }

    &__product {
      font-size: 1.2rem;
      color: inherit;
      opacity: 0.7;
    }

    &__name {
      font-size: 3rem;
      font-weight: 600;
    }

    &__description {
      font-size: 1.4rem;
    }

    &__cta {
      display: inline-block;
      align-self: end;
      justify-self: start;
    }
  }
}

.white {
  background-image: radial-gradient(
    circle farthest-corner at 18.7% 37.8%,
    rgba(250, 250, 250, 1) 0%,
    rgba(225, 234, 238, 1) 90%
  );
}

.black {
  background: linear-gradient(to left, #0f2027, #203a43, #2c5364);
  color: white;
}

.dark-blue {
  background-image: radial-gradient(
    circle farthest-corner at 10% 20%,
    rgba(51, 102, 153, 1) 0%,
    rgba(68, 116, 163, 1) 90%
  );
  color: white;
}

.color-grey {
  color: rgba(white, 0.7);
}

.btn {
  background-color: #ffdd2d;
  border-radius: 4px;
  padding: 1.3rem 4rem;
  color: black;
  font-size: 1.4rem;
  color: #4a5568;

  &--slides-control {
    width: 16px;
    height: 16px;
  }
}
</style>
