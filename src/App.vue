<script setup>

import { ref, computed, onMounted } from 'vue';

import BlogPost from './components/BlogPost.vue';
import PaginatePost from './components/PaginatePost.vue';
import LoadingSpinner from './components/LoadingSpinner.vue';

const posts = ref([]);
const postxpage = 10;
const inicio = ref(0);
const fin = ref(postxpage);
const loading = ref(true);

const favorito = ref('');

const cambiarFavorito = (title) => {
  favorito.value = title;
};

const next = () =>{
  inicio.value = inicio.value + postxpage;
  fin.value = fin.value + postxpage;
};

const prev = () =>{
  inicio.value += -postxpage;
  fin.value += -postxpage;
};

/* onMounted(async() => {

  try {
    const res = await fetch('https://jsonplaceholder.typicode.com/posts')
    posts.value = await res.json()
  } catch (error) {
    console.log(error)
  }finally {
    setTimeout(() => {
        loading.value = false;
      }, 500);
  }

}); */

  /* fetch('https://jsonplaceholder.typicode.com/posts')
    .then(res => res.json())
    .then(data => {posts.value = data;})
    .catch(e => console.log(e))
    .finally(() => {
      setTimeout(() => { //este callback de setTimeout es solo para mostrar que si funciona el spinner Loading no es recomendable usarlo en un proyecto la forma correcta seria setTimeout(() => { loading.value = false });
        loading.value = false;
      }, 500);
    }); */

const fetchData = async() => {
  try {
    const res = await fetch('https://jsonplaceholder.typicode.com/posts')
    posts.value = await res.json()
  } catch (error) {
    console.log(error)
  }finally {
    setTimeout(() => {
        loading.value = false;
      }, 500);
  }
}

fetchData()

  const maxLength = computed(() => posts.value.length )


</script>

<template>
  <LoadingSpinner v-if="loading"/>
  <div class="container" v-else>
    <h1>App</h1>
    <h2>Mis post Fvaoritos : {{ favorito }}</h2>

    <PaginatePost @next="next" @prev="prev" :inicio="inicio" :fin="fin" :maxLength="maxLength" class="mb-2" />

    <BlogPost 
    v-for="post in posts.slice(inicio, fin)" 
    :key="post.id" 
    :title="post.title" 
    :id="post.id" 
    :body="post.body"
    @cambiarFavoritoNombre="cambiarFavorito"
    class="mb-2"
    ></BlogPost>

  </div>
 
</template>