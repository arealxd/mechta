<script setup lang="ts">
import type { Delivery } from "@/types/Delivery";
import {computed} from "vue";

const props = withDefaults(
    defineProps<{
      delivery: Delivery[]
    }>(),
    {
      delivery: undefined
    }
)

const pickup = computed(() =>
    props.delivery.find((item) => item.type === 'pickup')
)

const courier = computed(() =>
    props.delivery.find((item) => item.type === 'courier')
)

const post = computed(() =>
    props.delivery.find((item) => item.type === 'post')
)
</script>

<template>
  <div v-if="delivery" class="cards">
    <div v-if="pickup" class="cards__check" :class="{'not-available': !pickup?.available}">
      <div class="cards__check--inner" :class="{'cards__check--split': !pickup?.available}">
        <div class="cards__check--inner__info" :class="{regap: !pickup?.available}">
          <div class="cards__check--inner__info--title">
            <p class="cards__check--inner__info--title__main">Pick up</p>
            <p v-if="!pickup?.available" class="cards__check--inner__info--title__child">Not availible in that city</p>
          </div>
          <div v-if="pickup?.price !== null" class="cards__check--inner__info--price">
            {{ pickup?.price }}$
          </div>
        </div>
        <img class="cards__check--inner__icon" src="/img/pickup.svg" alt="pickup">
      </div>
      <div class="cards__check--mark">
        <img src="/img/check.svg" alt="check">
      </div>
    </div>
    <div v-if="courier" class="cards__item" :class="{'not-available': !courier?.available}">
      <div class="cards__item--info" :class="{regap: !courier?.available}">
        <div class="cards__item--info__title">
          <p class="cards__item--info__title--main">Courier</p>
          <p v-if="!courier?.available" class="cards__item--info__title--child">Not availible in that city</p>
        </div>
        <div v-if="courier?.price !== null" class="cards__item--info__price">
          {{ courier?.price }}$
        </div>
      </div>
      <img class="cards__item--icon" src="/img/courier.svg" alt="courier">
    </div>
    <div v-if="post" class="cards__item" :class="{'not-available': !post?.available}">
      <div class="cards__item--info" :class="{regap: !post?.available}">
        <div class="cards__item--info__title">
          <p class="cards__item--info__title--main">Post</p>
          <p v-if="!post?.available" class="cards__item--info__title--child">Not availible in that city</p>
        </div>
        <div v-if="post?.price !== null" class="cards__item--info__price">
          {{ post?.price }}$
        </div>
      </div>
      <img class="cards__item--icon" src="/img/post.svg" alt="post">
    </div>
  </div>
</template>

<style lang="scss" scoped src="./index.scss" />
