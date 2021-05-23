<template>
  <div>
    <DocsCategoryHeader
      :emoji="categoryInfo.emoji"
      :title="categoryInfo.title"
      :description="categoryInfo.description"
    />
    <AppContainer class="relative z-10 docs-category__content">
      <div class="rounded-md bg-white shadow-md overflow-hidden">
        <DocsArticleItem
          v-for="article in articles"
          :key="article.slug"
          :to="`/docs/${categoryInfo.slug}/${article.slug}/`"
          :title="article.title"
          :description="article.description"
        />
      </div>
    </AppContainer>
  </div>
</template>

<script>
import { head } from '~/utils/head'

export default {
  async asyncData({ $content, params }) {
    const categoryInfo = await $content(
      'docs',
      '_categories',
      params.category
    ).fetch()
    const articles = await $content('docs', params.category)
      .sortBy('priority', 'asc')
      .fetch()

    return {
      categoryInfo,
      articles,
    }
  },
  head() {
    return head(
      `${this.categoryInfo.title} — マニュアル`,
      this.categoryInfo.description
    )
  },
}
</script>

<style lang="scss" scoped>
.docs-category {
  &__content {
    margin-top: #{-$spacing-xl};
  }
}
</style>