<template>
  <article>
    <h2>{{anime.title}}</h2>
    <p>{{anime.description}}</p>
  </article>
</template>

<script>
export default {
  props: ['anime']
}
</script>

<style>
article {
    background-color:bisque;
    border-radius: 1rem;
    padding: 1rem 2rem;
    margin-bottom: 10px;
    box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
}
h2 {
  color:brown;
}
p {
  color: black;
}
</style>