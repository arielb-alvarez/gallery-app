<script setup lang="ts">
import { ref } from 'vue';
import axios from 'axios';

const images = ref([]);
const isAddImageLoading = ref(false);

const addImage = async () => {
  isAddImageLoading.value = true;
  const response = await axios.get('https://picsum.photos/200')
    .then(response => {
      isAddImageLoading.value = false;
      return response;
    });
  images.value.push(response.request.responseURL);
};

const removeImage = () => {
  if (images.value.length) {
    const random = Math.floor(Math.random() * images.value.length);
    images.value.splice(random, 1);
  }
};
</script>

<template>
    <div class="gallery">
      <div v-if="!images.length && !isAddImageLoading" class="no-image">No Image Available. <br> click "Add Image" button to add image(s).</div>
      <img class="item" v-for="image in images" :src="image" />
      <div v-if="isAddImageLoading === true" class="item-skeleton"></div>
    </div>
    <div class="action-buttons">
        <button class="btn add" @click="addImage" :disabled="isAddImageLoading === true">Add Image</button><br>
        <button class="btn remove" @click="removeImage">Remove Random Image</button>
    </div>
</template>

<style scoped>
  .gallery {
    display: flex;
    gap: 10px;
    flex-wrap: wrap;
    justify-content: center;
  }

  .no-image {
    text-align: center;
    color: #cecece;
    font-size: 14px;
  }

  .item,
  .item-skeleton {
    padding: 20px;
    border: solid 1px rgba(206, 206, 206, 0.5);
    border-radius: 5px;
  }

  .item-skeleton {
    width: 242px;
    height: 242px;
    background: #eee;
    background: linear-gradient(110deg, #ececec 8%, #f5f5f5 18%, #ececec 33%);
    border-radius: 5px;
    background-size: 200% 100%;
    animation: 1.5s shine linear infinite;
  }

  .action-buttons {
    margin-top: 20px;
    text-align: center;
  }

  .btn {
    display: inline-block;
    border: none;
    background: none;
    cursor: pointer;

    &.add {
      font-size: 30px;
      border: solid 1px rgba(165, 165, 165, 0.5);
      color: rgba(165, 165, 165, 0.5);
      padding: 10px 20px;
      border-radius: 5px;

      &:hover {
        color: rgb(165, 165, 165);
        border: solid 1px rgb(165, 165, 165);
      }

      &:disabled {
        cursor: not-allowed;
      }
    }

    &.remove {
      font-size: 10px;
      color: rgba(255, 0, 0, 0.4);

      &:hover {
        color: rgba(255, 0, 0, 1);
      }
    }
  }

  @keyframes shine {
    to {
      background-position-x: -200%;
    }
  }
</style>