<script setup>
import EventService from "@/services/EventService.js";
import { computed, onMounted, ref } from "vue";
import router from "@/router";



const props = defineProps(["id"]);

const event = ref("");
const id = computed(() => props.id);
onMounted(() => {
  EventService.getEvent(id.value)
      .then((response) => {
        event.value = response.data;
      })
      .catch((error) => {
        if(error.response && error.response.status === 404){
       router.push({
         name: '404Resource',
         params: {resource: 'event' }
       }) } else {
          router.push({ name: 'NetworkError' })
        }
      });
});
</script>

<template>
  <div v-if="event">
    <h1>{{ event.title }}</h1>
    <div id="nav">

      <router-link :to="{name: 'EventDetails'}">Details</router-link>
      |  <!--url will look something like: /event/2 -->
      <router-link :to="{name: 'EventRegister'}">Register</router-link>
      |  <!--url will look something like: /event/2/register -->
      <router-link :to="{name: 'EventEdit'}">Edit</router-link>
      <!--url will look something like: /event/2/edit -->

    </div>
    <router-view :event="event" />
  </div>
</template>
