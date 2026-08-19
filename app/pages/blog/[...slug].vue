<!-- app/pages/blog/[...slug].vue -->
<script setup>
const route = useRoute()
const { data: article } = await useAsyncData(route.path, () => {
  return queryCollection('blog').path(route.path).first()
})

const { data: surround } = await useAsyncData(`surround-${route.path}`, () => {
  return queryCollectionItemSurroundings('blog', route.path)
    .where('category', '=', article.value?.category)
    .order('date', 'DESC')
})
</script>

<template>
  <article class="container">
    <ContentRenderer :value="article" />
    <Author :author="article.author" />
    <PrevNext :prev="surround?.[0]" :next="surround?.[1]" />
  </article>
</template>
