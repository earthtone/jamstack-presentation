<template>
  <section>
    <global-events @keydown.right="next" @keydown.left="prev"/>
    <article v-html="$page.slide.content" class="flex flex-col items-center justify-center w-screen h-full min-h-screen p-6 text-gray-800 bg-orange-100"/>
  </section>
</template>
<page-query>
  query Slide ($path: String!) {
    slide(path: $path) {
      id
      path
      content
    }

    slides: allSlide (order:ASC){
      edges {
        node {
          path
        }
      }
    }
  }
</page-query>
<script>
export default {
  methods: {
    next () {
      this.$router.push(this.nextSlide)
    },
    prev () {
      if (this.currentIndex <= 0) return
      this.$router.push(this.prevSlide)
    }
  },
  computed: {
    slides () {
      return this.$page.slides.edges.map(({ node }) => node.path).sort((a,b) => a > b)
    },
    currentIndex () {
      return this.slides.findIndex(p => p === this.$page.slide.path)
    },
    nextSlide () {
      return this.slides[(this.currentIndex + 1) % this.slides.length]
    },
    prevSlide () {
      return this.slides[(this.currentIndex - 1) % this.slides.length]
    }
  }
}
</script>
<style scoped>
/deep/ p {
  @apply w-full flex justify-center;
}

/deep/ img[alt="webpack"] {
  @apply w-64;
}

/deep/ img[alt="jamstack workflow"] {
  @apply h-screen;
}

/deep/ pre {
  @apply h-full px-16 max-w-full;
}

/deep/ img[alt="jamstack logo"] {
  @apply w-screen;
}

/deep/ h2 {
  @apply text-4xl uppercase;
}

/deep/ ul {
  @apply list-inside list-disc my-12;
}

/deep/ li {
  @apply text-2xl;
}

/deep/ table {
  @apply table-auto text-6xl;
}

/deep/ td {
  @apply border px-4 py-2;
}

/deep/ blockquote {
  @apply text-3xl w-1/2 text-gray-700;
}
</style>
