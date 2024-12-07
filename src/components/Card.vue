<script setup lang="ts">
  defineProps<{
    background: string;
    // img: string;
    src: string;
    edition: string;
    rarity: 'common' | 'uncommon' | 'rare';
    title: string;
    description: string;
    illustrator: string;
    cardnum: number;
    translateX: number;
    translateY: number;
    scale: number;
  }>()
</script>

<template>
  <div class="card">

    <img :src="src" />
    <div class="edition meta">{{ edition }}</div>
    <div class="rarity meta">{{ rarity }}</div>
    <div class="title box">{{ title }}</div>
    <div class="img-spacer"></div>
    <div class="description box">{{ description }}</div>
    <div class="illustrator meta">Illustrated by {{ illustrator }}</div>
    <div class="card-num meta">Card Number {{ cardnum }}</div>
  </div>
</template>

<style scoped>
  .card {
    aspect-ratio: 63/88;
    background: v-bind(background);
    /* min-width: 270px; */
    width: 270px;
    border-radius: 10px;
    border: 5px solid grey;
    position: relative;
    display: grid;
    padding: 0 15px;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: calc(5/88*100%) calc(7/88*100%) calc(59/88*100%) calc(11/88*100%) calc(4/88*100%);
    grid-template-areas:
      "edition rarity"
      "title title"
      "img-spacer img-spacer"
      "desc desc"
      "illustrator cardnum"
    ;
    overflow: hidden;

    & .box {
      background: #000;
      border: 1px solid grey;
      display: grid;
      place-items: center;
    }

    .meta {
      font-size: 8px;
      -webkit-text-stroke: 0.1px #000;
      /* text-stroke: 1px #000; */
      text-shadow: 0.5px 0.5px 1px #000;
    }

    &>img {
      position: absolute;
      width: 100%;
      height: auto;
      object-fit: cover;
      z-index: -1;
      transform: translate(calc(v-bind(translateX) * 1px), calc(v-bind(translateY) * 1px)) scale(v-bind(scale));
      /* transform: translate(-0px, -30px) scale(1.5); */
    }

    .edition {
      grid-area: edition;
      display: grid;
      align-items: end;
    }

    .rarity {
      grid-area: rarity;
      display: grid;
      place-items: end;
    }

    .title {
      grid-area: title;
      text-transform: uppercase;
      font-weight: bold;
      font-size: 16px;
    }

    .img-spacer {
      grid-area: img-spacer;
    }

    .description {
      grid-area: desc;
      background: #000;
      font-size: 10px;
      font-weight: bold;
    }

    .illustrator {
      grid-area: illustrator;
    }

    .card-num {
      grid-area: cardnum;
      display: grid;
      justify-content: end;
    }

  }
</style>
