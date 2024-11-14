<template>
  <div>
      <h1>
          Image Gallery by TR-Groups
      </h1>
      <div class="controls">
          <input v-model="newImageUrl" placeholder="Enter image URL" />
          <button @click="addImage">Add Image</button>
      </div>
      <div class="image-container">
          <div v-for="(image, index) in images" :key="index" class="image-wrapper">
              <img :src="image" alt="Image" @click="showPreview(image, index)">
              <button class="delete-btn" @click="deleteImage(index)">&times;</button>
          </div>
      </div>
      <div class="modal" v-if="previewImage">
          <span class="close" @click="closePreview">&times;</span>
          <button class="nav-btn prev" @click="prevImage">&lt;</button>
          <button class="nav-btn next" @click="nextImage">&gt;</button>
          <img :src="previewImage" alt="Preview">
      </div>
  </div>
</template>

<script>
export default {
    data() {
        return {
            images: [
                'https://images.ctfassets.net/x7j9qwvpvr5s/43adRuY33iuCayAyMy3wTw/5545b174f876fc95ffcfff3d643c4d23/Ducati-MY25-Panigale-V4-overview-carousel-hero-link-1600x650-01.jpg',
                'https://files.porsche.com/filestore/galleryimagerwd/multimedia/none/modelseries-911gt3-rs-gallery-03/zoom2/7aff50b3-117c-11ed-80f5-005056bbdc38;sO;twebp/porsche-zoom2.webp',
                'https://files.porsche.com/filestore/galleryimagerwd/multimedia/none/modelseries-911gt3-rs-gallery-04/zoom2/8141901d-117c-11ed-80f5-005056bbdc38;sO;twebp/porsche-zoom2.webp',
                'https://files.porsche.com/filestore/galleryimagerwd/multimedia/none/modelseries-911gt3-rs-gallery-01/zoom2/6d533072-117c-11ed-80f5-005056bbdc38;sI;twebp/porsche-zoom2.webp',
                'https://files.porsche.com/filestore/galleryimagerwd/multimedia/none/modelseries-911gt3-rs-gallery-06/zoom2/b108eb0a-117c-11ed-80f5-005056bbdc38;sI;twebp/porsche-zoom2.webp'
            ],
            newImageUrl: '',
            previewImage: '',
            currentIndex: 0
        };
    },
    methods: {
        showPreview(image, index) {
            this.previewImage = image;
            this.currentIndex = index;
        },
        closePreview() {
            this.previewImage = '';
            this.currentIndex = 0;
        },
        prevImage() {
            if (this.currentIndex > 0) {
                this.currentIndex--;
                this.previewImage = this.images[this.currentIndex];
            }
        },
        nextImage() {
            if (this.currentIndex < this.images.length - 1) {
                this.currentIndex++;
                this.previewImage = this.images[this.currentIndex];
            }
        },
        addImage() {
            if (this.newImageUrl) {
                this.images.push(this.newImageUrl);
                this.newImageUrl = ''; // Clear input after adding
            } else {
                alert('Please enter a valid image URL');
            }
        },
        deleteImage(index) {
            this.images.splice(index, 1);
            if (this.currentIndex >= this.images.length) {
                this.currentIndex = this.images.length - 1; // Adjust currentIndex if necessary
            }
            if (this.images.length === 0) {
                this.closePreview(); // Close modal if no images remain
            }
        }
    }
};
</script>

<style>
  body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
    background-color: #f4f4f4;
}

h1 {
    text-align: center;
    margin: 20px 0;
    color: #333;
}

.controls {
    display: flex;
    justify-content: center;
    margin: 20px;
}

.controls input {
    padding: 10px;
    font-size: 16px;
    margin-right: 10px;
    width: 300px;
}

.controls button {
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
}

.image-container {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(320px, 1fr));
    gap: 15px;
    justify-items: center;
    padding: 20px;
}

.image-wrapper {
    position: relative;
    width: 320px;
    height: 240px; /* Fixed height for all images */
    overflow: hidden;
}

.image-wrapper img {
    width: 100%;
    height: 100%;
    object-fit: cover;
    border-radius: 12px;
    box-shadow: 0 6px 14px rgba(0, 0, 0, 0.3);
    transition: transform 0.4s ease, box-shadow 0.4s ease;
}

.image-wrapper img:hover {
    transform: scale(1.1);
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.4);
}

.delete-btn {
    position: absolute;
    top: 10px;
    right: 10px;
    background-color: rgba(255, 0, 0, 0.7);
    color: #fff;
    border: none;
    padding: 5px;
    font-size: 20px;
    cursor: pointer;
    border-radius: 50%;
    transition: background-color 0.3s ease;
}

.delete-btn:hover {
    background-color: rgba(255, 0, 0, 0.9);
}

.modal {
    display: flex;
    align-items: center;
    justify-content: center;
    position: fixed;
    z-index: 1000;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: hidden;
    background-color: rgba(0, 0, 0, 0.8);
    opacity: 0;
    animation: fadeIn 0.5s forwards;
}

.modal img {
    max-width: 80%;
    max-height: 70%;
    border-radius: 12px;
    box-shadow: 0 8px 20px rgba(0, 0, 0, 0.5);
    transition: transform 0.4s ease;
}

.modal img:hover {
    transform: scale(1.05);
}

.close {
    color: #fff;
    position: absolute;
    top: 20px;
    right: 20px;
    font-size: 36px;
    font-weight: bold;
    cursor: pointer;
    transition: color 0.3s ease;
}

.close:hover,
.close:focus {
    color: #ddd;
    text-decoration: none;
}

.nav-btn {
    position: absolute;
    top: 50%;
    transform: translateY(-50%);
    background-color: rgba(0, 0, 0, 0.6);
    color: #fff;
    border: none;
    padding: 10px 20px;
    font-size: 24px;
    cursor: pointer;
    border-radius: 50%;
    transition: background-color 0.3s ease;
}

.nav-btn:hover {
    background-color: rgba(0, 0, 0, 0.8);
}

.prev {
    left: 20px;
}

.next {
    right: 20px;
}

@keyframes fadeIn {
    from {
        opacity: 0;
    }
    to {
        opacity: 1;
    }
}

</style>
