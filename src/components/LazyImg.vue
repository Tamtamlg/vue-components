<template>
  <picture>
    <source
      v-if="desktop2xWebp && desktopWebp"
      class="lazy"
      :data-srcset="`${desktop2xWebp} 2x, ${desktopWebp} 1x`"
      type="image/webp"
      :media="`(min-width: ${media}px)`"
    />
    <source
      v-if="desktop2x && desktop"
      class="lazy"
      :data-srcset="`${desktop2x} 2x, ${desktop} 1x`"
      :media="`(min-width: ${media}px)`"
    />
    <source
      v-if="mobile2xWebp && mobileWebp"
      class="lazy"
      :data-srcset="`${mobile2xWebp} 2x, ${mobileWebp} 1x`"
      type="image/webp"
    />
    <source v-if="mobile2x && mobile" class="lazy" :data-srcset="`${mobile2x} 2x, ${mobile} 1x`" />
    <img
      class="lazy"
      src="data:image/gif;base64,R0lGODlhAQABAAAAACH5BAEKAAEALAAAAAABAAEAAAICTAEAOw=="
      :data-src="desktop"
      :data-srcset="desktop2x ? `${desktop2x} 2x` : ''"
      :alt="alt"
    />
  </picture>
</template>

<script>
export default {
  props: {
    alt: {
      type: String,
      default: ""
    },
    media: {
      type: Number,
      default: 768
    },
    desktop: {
      type: String,
      required: true
    },
    desktop2x: {
      type: String,
      default: ""
    },
    mobile: {
      type: String,
      required: ""
    },
    mobile2x: {
      type: String,
      default: ""
    },
    desktopWebp: {
      type: String,
      required: ""
    },
    desktop2xWebp: {
      type: String,
      default: ""
    },
    mobileWebp: {
      type: String,
      required: ""
    },
    mobile2xWebp: {
      type: String,
      default: ""
    }
  },
  mounted() {
    const lazyImages = [].slice.call(document.querySelectorAll(".lazy"));

    if (
      "IntersectionObserver" in window &&
      "IntersectionObserverEntry" in window &&
      "intersectionRatio" in window.IntersectionObserverEntry.prototype
    ) {
      const lazyImageObserver = new IntersectionObserver(entries => {
        entries.forEach(entry => {
          if (entry.isIntersecting) {
            const lazyImage = entry.target;
            if (lazyImage.dataset.src) {
              lazyImage.src = lazyImage.dataset.src;
            }
            lazyImage.srcset = lazyImage.dataset.srcset;
            lazyImage.classList.remove("lazy");
            lazyImageObserver.unobserve(lazyImage);
          }
        });
      });

      lazyImages.forEach(lazyImage => {
        lazyImageObserver.observe(lazyImage);
      });

      this.$once("hook:beforeDestroy", () => {
        lazyImageObserver.disconnect();
      });
    } else {
      lazyImages.forEach(lazyImage => {
        lazyImage.src = lazyImage.dataset.src;
        lazyImage.srcset = lazyImage.dataset.srcset;
      });
    }
  }
};
</script>