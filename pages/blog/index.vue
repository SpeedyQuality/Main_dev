<template>
<div>
  <HeroSection
    title="Notre Blog"
    subtitle="Découvrez nos articles, conseils et actualités sur la transformation digitale des entreprises."
    primaryButtonText="Explorer nos articles"
    primaryButtonLink="#articles"
  />
  
  <!-- Featured Article -->
  <section class="py-12 bg-gray-light">
    <div class="container">
      <h2 class="text-2xl font-bold mb-8">Article à la une</h2>
      <div class="bg-white rounded-lg shadow-elegant overflow-hidden">
        <div class="grid grid-cols-1 lg:grid-cols-2">
          <div class="relative aspect-[4/3] lg:aspect-auto">
            <img 
              src="https://images.unsplash.com/photo-1552664730-d307ca884978?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2340&q=80" 
              alt="Stratégie digitale pour PME" 
              class="w-full h-full object-cover"
            >
          </div>
          <div class="p-8 lg:p-12 flex flex-col justify-center">
            <div class="flex items-center mb-4">
              <span class="bg-primary/10 text-primary px-3 py-1 rounded-full text-sm">Stratégie Digitale</span>
              <span class="mx-2 text-gray-dark">•</span>
              <time class="text-gray-dark text-sm">1 avril 2025</time>
            </div>
            <h3 class="text-2xl font-bold mb-4">Je suis une PME, ai-je réellement besoin d'une stratégie digitale ?</h3>
            <p class="text-gray-dark mb-6">Découvrez pourquoi la transformation digitale n'est plus une option mais une nécessité pour la croissance et la pérennité des PME en 2025.</p>
            <NuxtLink to="/blog/pme-strategie-digitale" class="btn-primary inline-flex items-center">
              Lire l'article
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 ml-2" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M12.293 5.293a1 1 0 011.414 0l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414-1.414L14.586 11H3a1 1 0 110-2h11.586l-2.293-2.293a1 1 0 010-1.414z" clip-rule="evenodd" />
              </svg>
            </NuxtLink>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- All Articles -->
  <section id="articles" class="section">
    <div class="container">
      <div class="flex flex-col md:flex-row justify-between items-center mb-12">
        <h2 class="text-2xl font-bold">Tous nos articles</h2>
        <div class="flex gap-4 mt-4 md:mt-0">
          <button 
            v-for="category in categories" 
            :key="category.value"
            @click="selectedCategory = category.value"
            class="px-4 py-2 rounded-full transition-colors duration-300"
            :class="selectedCategory === category.value ? 'bg-primary text-white' : 'bg-gray-light text-gray-dark hover:bg-primary/10'"
          >
            {{ category.label }}
          </button>
        </div>
      </div>

      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-8">
        <article v-for="article in filteredArticles" :key="article.slug" class="bg-white rounded-lg shadow-elegant overflow-hidden transform transition-all duration-300 hover:-translate-y-2 hover:shadow-hover">
          <div class="relative aspect-video">
            <img :src="article.image" :alt="article.title" class="w-full h-full object-cover">
            <div class="absolute top-4 left-4">
              <span class="bg-primary text-white px-4 py-1 rounded-full text-sm">{{ article.category }}</span>
            </div>
          </div>
          <div class="p-6">
            <div class="flex items-center mb-4 text-sm text-gray-dark">
              <time :datetime="article.dateISO">{{ article.date }}</time>
              <span class="mx-2">•</span>
              <span>{{ article.readTime }} min de lecture</span>
            </div>
            <h3 class="text-xl font-bold mb-3 line-clamp-2">{{ article.title }}</h3>
            <p class="text-gray-dark mb-4 line-clamp-3">{{ article.excerpt }}</p>
            <NuxtLink :to="`/blog/${article.slug}`" class="inline-flex items-center text-primary font-semibold hover:text-accent transition-colors duration-300">
              Lire l'article
              <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 ml-2" viewBox="0 0 20 20" fill="currentColor">
                <path fill-rule="evenodd" d="M12.293 5.293a1 1 0 011.414 0l4 4a1 1 0 010 1.414l-4 4a1 1 0 01-1.414-1.414L14.586 11H3a1 1 0 110-2h11.586l-2.293-2.293a1 1 0 010-1.414z" clip-rule="evenodd" />
              </svg>
            </NuxtLink>
          </div>
        </article>
      </div>
    </div>
  </section>
  
  <!-- Newsletter Section -->
  <section class="section bg-gray-light">
    <div class="container">
      <div class="max-w-2xl mx-auto text-center">
        <h2 class="text-3xl font-bold mb-4">Restez informé</h2>
        <p class="text-gray-dark mb-8">
          Inscrivez-vous à notre newsletter pour recevoir nos derniers articles et conseils sur la transformation digitale.
        </p>
        <form @submit.prevent="subscribeNewsletter" class="flex flex-col sm:flex-row gap-4">
          <input 
            type="email" 
            v-model="email" 
            placeholder="Votre adresse email" 
            class="flex-grow px-4 py-3 rounded-lg border border-gray-medium focus:outline-none focus:ring-2 focus:ring-primary/50"
            required
          >
          <button type="submit" class="btn-primary whitespace-nowrap">
            S'inscrire
          </button>
        </form>
      </div>
    </div>
  </section>
</div>
</template>

<script setup>
const email = ref('')
const selectedCategory = ref('all')

const categories = [
{ label: 'Tous', value: 'all' },
{ label: 'Stratégie', value: 'strategie' },
{ label: 'Marketing', value: 'marketing' },
{ label: 'Conversion', value: 'conversion' }
]

const articles = [
{
  slug: 'pme-strategie-digitale',
  title: 'Je suis une PME, ai-je réellement besoin d\'une stratégie digitale ?',
  excerpt: 'Découvrez pourquoi la transformation digitale n\'est plus une option mais une nécessité pour la croissance et la pérennité des PME en 2025.',
  image: 'https://images.unsplash.com/photo-1552664730-d307ca884978?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2340&q=80',
  category: 'Stratégie',
  categoryValue: 'strategie',
  date: '1 avril 2025',
  dateISO: '2025-04-01',
  readTime: 7
},
{
  slug: 'importance-strategie-digitale',
  title: 'L\'importance d\'une stratégie digitale pour les PME en 2025',
  excerpt: 'Découvrez pourquoi une stratégie digitale solide est devenue indispensable pour les PME et comment la mettre en place efficacement.',
  image: 'https://images.unsplash.com/photo-1519389950473-47ba0277781c?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2340&q=80',
  category: 'Stratégie',
  categoryValue: 'strategie',
  date: '15 avril 2025',
  dateISO: '2025-04-15',
  readTime: 5
},
{
  slug: 'tendances-marketing-digital',
  title: 'Les tendances du marketing digital à suivre en 2025',
  excerpt: 'Analyse des principales tendances qui façonnent le marketing digital cette année et comment les intégrer dans votre stratégie.',
  image: 'https://images.unsplash.com/photo-1460925895917-afdab827c52f?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2340&q=80',
  category: 'Marketing',
  categoryValue: 'marketing',
  date: '10 avril 2025',
  dateISO: '2025-04-10',
  readTime: 8
},
{
  slug: 'optimisation-conversion',
  title: 'Comment optimiser votre taux de conversion en 5 étapes',
  excerpt: 'Guide pratique pour améliorer le taux de conversion de votre site web et transformer plus de visiteurs en clients.',
  image: 'https://images.unsplash.com/photo-1551434678-e076c223a692?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2340&q=80',
  category: 'Conversion',
  categoryValue: 'conversion',
  date: '5 avril 2025',
  dateISO: '2025-04-05',
  readTime: 6
}
]

const filteredArticles = computed(() => {
if (selectedCategory.value === 'all') {
  return articles
}
return articles.filter(article => article.categoryValue === selectedCategory.value)
})

const subscribeNewsletter = () => {
// TODO: Implémenter l'inscription à la newsletter
console.log('Email inscrit:', email.value)
email.value = ''
alert('Merci pour votre inscription !')
}

useHead({
title: 'Blog',
meta: [
  { 
    name: 'description', 
    content: 'Découvrez nos articles, conseils et actualités sur la transformation digitale des entreprises. Restez informé des dernières tendances du digital.'
  }
]
})

definePageMeta({
layout: 'default'
})
</script>