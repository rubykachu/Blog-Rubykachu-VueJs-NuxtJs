<template>
  <v-container class="article-category-page">
    <v-row class="mb-10">
      <v-col cols="12" class="px-0">
        <v-card
          class="rounded-15 text-center white--text"
          color="grey"
          min-width="100%"
          height="140px"
          img="/bg_default_post_detail_1600x500.jpg"
        >
          <div>
            <v-icon class="article-category-page__icon" color="white" size="38" :style="{ backgroundColor: category.color }"
              >mdi-tag-outline</v-icon
            >
            <h2 class="article-category-page__subject">{{ category.name }}</h2>
          </div>
        </v-card>
      </v-col>
    </v-row>

    <v-row>
      <v-col class="mb-8" cols="md-4" sm="6" v-for="article in articles" :key="article.id" v-if="articles">
        <article-item :article="article" />
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import ArticleItem from '@/components/article/ArticleItem.vue'
export default {
  components: {
    ArticleItem
  },
  head() {
    return {
      title: this.category.name
    }
  },
  async asyncData({ store, route, redirect }) {
    try {
      let category_id = route.params.id
      const articles = await store.dispatch('article/getArticlesByCategory', category_id)
      const category = await store.dispatch('category/findSlug', category_id)

      return { articles, category }
    } catch (e) {
      console.log(e)
      store.dispatch('toast/show')
      redirect('/404')
    }
  }
}
</script>
