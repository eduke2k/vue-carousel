<template>
  <div v-show="carousel.pageCount > 1" class="VueCarousel-pagination">
    <ul class="VueCarousel-dot-container" role="tablist">
      <li
        class="VueCarousel-dot"
        aria-hidden="false"
        role="presentation"
        :aria-selected="isCurrentDot(index) ? 'true' : 'false'"
        v-bind:class="{ 'VueCarousel-dot--active': isCurrentDot(index) }"
        v-for="(page, index) in paginationCount"
        :key="`${page}_${index}`"
        v-on:click="goToPage(index)"
        :style="
          `
          margin-top: ${carousel.paginationPadding * 2}px;
          padding: ${carousel.paginationPadding}px;
        `
        "
      >
        <button
          type="button"
          role="button"
          aria-label="`Item ${index}`"
          :title="`Item ${index}`"
          class="VueCarousel-dot-button"
          :tabindex="0"
          :style="
            `
            width: ${carousel.paginationSize}px;
            height: ${carousel.paginationSize}px;
            background: ${
              isCurrentDot(index)
                ? carousel.paginationActiveColor
                : carousel.paginationColor
            };
            `
          "
        ></button>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: "pagination",
  inject: ["carousel"],
  computed: {
    paginationCount() {
      return this.carousel && this.carousel.scrollPerPage
        ? this.carousel.pageCount
        : this.carousel.slideCount && this.carousel.currentPerPage
        ? this.carousel.slideCount - this.carousel.currentPerPage + 1
        : 0;
    }
  },
  methods: {
    /**
     * Change page by index
     * @param {number} index
     * return {void}
     */
    goToPage(index) {
      /**
       * @event paginationclick
       * @type {number}
       */
      this.$emit("paginationclick", index);
    },

    /**
     * Check on current dot
     * @param {number} index - dot index
     * @return {boolean}
     */
    isCurrentDot(index) {
      return index === this.carousel.currentPage;
    }
  }
};
</script>
