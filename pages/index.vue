<template>
  <section class="util__container">
    <component
      v-bind:is="story.content.component"
      v-if="story.content.component"
      :key="story.content._uid"
      :blok="story.content">
    </component>
  </section>
</template>

<script>
export default {
  data () {
    return { story: { content: {} } }
  },
  mounted () {
    this.$storyblok.init()
    this.$storyblok.on(['change', 'published'], () => {
      location.reload(true)
    })
  },
  asyncData (context) {
    // Check if we are in the editor mode
    const version = context.query._storyblok || context.isDev ? 'draft' : 'published'

    // Load the JSON from the API
    return context.app.$storyapi.get('cdn/stories/home', {
      version
    }).then((res) => {
      return res.data
    }).catch((res) => {
      context.error({ statusCode: res.response.status, message: res.response.data })
    })
  }
}
</script>
