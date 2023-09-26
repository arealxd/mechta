<script setup lang="ts">
import TheFooter from "@/components/footer/index.vue";
import TheCards from "@/components/cards/index.vue";
import TheLogo from "@/components/logo/index.vue";
import TheTitle from "@/components/title/index.vue";
import TheForm from "@/components/form/index.vue";
import { ref } from "vue";
import type { Delivery } from "@/types/Delivery";

const deliveryResponse = ref<Delivery[]>([])

const setResponse = (response: Delivery[]) => {
  deliveryResponse.value = response
}
</script>

<template>
  <div class="main">
    <div class="main__left">
      <the-logo />
      <the-title />
      <the-form @response="setResponse"/>
      <the-footer class="main__left--footer"/>
    </div>
    <div class="main__right">
      <img v-if="deliveryResponse.length === 0" src="/img/fastService.svg" alt="photo">
      <the-cards :delivery="deliveryResponse" class="main__right--cards"/>
    </div>
    <the-footer class="main__footer"/>
  </div>
</template>

<style lang="scss" scoped src="./index.scss" />
