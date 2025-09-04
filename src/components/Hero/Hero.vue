<script setup lang="ts">
import {
  image,
  image2,
  image3,
  image4,
  image5,
  image6,
  image7,
  image8,
} from "../../constants";
import { onMounted, ref } from "vue";
import gsap from "gsap";
import { Observer } from "gsap/Observer";

const images = ref<HTMLElement | null>(null);
const images2 = ref<HTMLElement | null>(null);
const images3 = ref<HTMLElement | null>(null);

const speed = 0.015;
let x = 0;
let y = 0;
let easing = 0.08;

let requestAnimationFrameId: any = null;
const lerp = (start: number, target: number, amount: number) =>
  start * (1 - amount) + target * amount;

const animate = () => {
  x = lerp(x, 0, easing);
  y = lerp(y, 0, easing);

  gsap.set(images.value, {
    x: `+=${x}`,
    y: `+=${y}`,
  });
  gsap.set(images2.value, {
    x: `+=${x * 0.25}`,
    y: `+=${y * 0.25}`,
  });
  gsap.set(images3.value, {
    x: `+=${x * 0.35}`,
    y: `+=${y * 0.35}`,
  });

  if (Math.abs(x) < 0.01) x = 0;
  if (Math.abs(y) < 0.01) y = 0;

  if (x != 0 || y != 0) {
    requestAnimationFrame(animate);
  } else {
    cancelAnimationFrame(requestAnimationFrameId);
    requestAnimationFrameId = null;
  }
};

onMounted(() => {
  gsap.registerPlugin(Observer);
  if (!images.value || !images2.value || !images3.value) return;

  Observer.create({
    target: document.documentElement,
    type: "mouse, touch, pointer",
    onMove: (e) => {
      const movementX = e.deltaX * speed;
      const movementY = e.deltaY * speed;

      x += movementX;
      y += movementY;

      if (requestAnimationFrameId == null) {
        requestAnimationFrameId = requestAnimationFrame(animate);
      }
    },
  });
});
</script>

<template>
  <section class="hero">
    <div class="title">
      <h1>Breath of Nature</h1>
      <p class="text-2xl">A gentle breath, carried through nature’s silence.</p>
    </div>

    <div class="images" ref="images">
      <img :src="image" alt="1" />
      <img :src="image2" alt="2" />
      <img :src="image3" alt="3" />
    </div>

    <div class="images" ref="images2">
      <img :src="image4" alt="4" />
      <img :src="image5" alt="5" />
      <img :src="image6" alt="6" />
    </div>

    <div class="images" ref="images3">
      <img :src="image7" alt="7" />
      <img :src="image8" alt="8" />
    </div>
  </section>
</template>

<style scoped></style>
