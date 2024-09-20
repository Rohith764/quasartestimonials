<template>
  <div>
    <h1 style="font-size: 2.5rem;">Customer Testimonials</h1>
    <div v-if="testimonials.length" class="testimonials-row">
      <div v-for="testimonial in testimonials" :key="testimonial.id" class="testimonial-box">
        <q-card class="my-card q-pa-md" style="background: lightblue;">
          <q-card-section class="card-section">
            <h3>{{ testimonial.attributes.name }} - {{ testimonial.attributes.position }}</h3>
            <p>{{ testimonial.attributes.testimonial_text }}</p>
            <p><strong>Company:</strong> {{ testimonial.attributes.company }}</p>
            <p><strong>Rating:</strong> {{ testimonial.attributes.rating }} / 5</p>
            <p><strong>Featured:</strong> {{ testimonial.attributes.featured ? 'Yes' : 'No' }}</p>
            <p><small>Published on: {{ new Date(testimonial.attributes.publishedAt).toLocaleDateString() }}</small></p>
          </q-card-section>
        </q-card>
      </div>
    </div>
    <div v-else>
      <p>Loading testimonials...</p>
    </div>
  </div>
</template>

<style scoped>

.testimonials-row {
  display: flex;
  flex-wrap: wrap;
  gap: 16px; 
}

.testimonial-box {
  flex: 1 1 calc(33.33% - 16px); 
  max-width: 400px;
  margin-bottom: 16px;
  transform: scale(0.95);
}

.my-card {
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  border-radius: 30px;
}

.card-section {
  display: flex;
  flex-direction: column;
}

.card-section h3 {
  font-size: 20px;
  margin-bottom: 8px;
}

.card-section p, .card-section strong {
  font-size: 16px;
  margin: 4px 0;
}

.card-section small {
  font-size: 15px;
}
</style>

<script setup lang="ts">
import { ref, onMounted } from 'vue';
import axios from 'axios';

interface Testimonial {
  id: number;
  attributes: {
    name: string;
    position: string;
    company: string;
    testimonial_text: string;
    rating: number;
    featured: boolean;
    createdAt: string;
    updatedAt: string;
    publishedAt: string;
  };
}

const testimonials = ref<Testimonial[]>([]);

async function fetchTestimonials() {
  try {
    const response = await axios.get('https://www.marketing-backend.scientiflow.com/api/testimonials', {
      headers: {
        Authorization: 'Bearer 9d13375ffa60153ff36a8b802ff4f4e72aec52e0a5ad752a435e3d26736d36a80479056f21d5831f6a5a783f2524987ae51be2c88c1032f642cfb110d7577e567a49e65f83e7cb0aca79037ee8fb295ecc160cb5cb4b8d317582379bb10403a788b090ed2e3703da52b1f84cd2ff824e387990269cbbedbc614ef1b969d63666',
      },
    });
    testimonials.value = response.data.data; 
  } catch (error) {
    console.error('Error fetching testimonials:', error);
  }
};

onMounted(fetchTestimonials);
</script>
