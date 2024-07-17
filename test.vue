<template>
  <div class="container">
    <div class="blocks-wrapper">
      <v-block
        v-for="block in blocks"
        :active="block.active"
        :id="`block-${block.id}`"
        @click="block.active = true"
      ></v-block>
    </div>
  </div>
</template>

<script setup>
const deactiveStartIndex = 7;
const blocks = ref([
  { id: 1, active: false },
  { id: 2, active: false },
  { id: 3, active: false },
  { id: 4, active: false },
  { id: 5, active: false },
  { id: 6, active: false },
  { id: 7, active: false },
  { id: 8, active: false },
  { id: 8, active: false },
]);

const deactiveAllBlock = () => {
  for (let block of blocks.value) {
    // block.active = false
    console.log("deactive value");
    setTimeout(() => {
      console.log("time out called");
      block.active = false;
    }, 2000);
  }
};

watch(
  blocks,
  (newValue) => {
    const updatedBlocks = toRaw(newValue);
    const activeBlocks = updatedBlocks.filter((b) => b.active);

    if (activeBlocks.length >= deactiveStartIndex) {
      console.log("should deactive");
      deactiveAllBlock();
    } else {
      console.log("should not deactive");
    }
  },
  { deep: true }
);
</script>

<style scoped>
.container {
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

.blocks-wrapper {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  grid-template-rows: repeat(3, 1fr);
  gap: 15px;
  padding: 1rem;
  padding: 10px;
  width: 420px;
  height: 420px;
  border: 2px solid black;
  grid-template-areas:
    "block-1 block-2 block-3"
    "block-4 . block-5"
    "block-6 block-7 block-8";
}

#block-1 {
  grid-area: block-1;
}
#block-2 {
  grid-area: block-2;
}
#block-3 {
  grid-area: block-3;
}
#block-4 {
  grid-area: block-4;
}
#block-5 {
  grid-area: block-5;
}
#block-6 {
  grid-area: block-6;
}
#block-7 {
  grid-area: block-7;
}
#block-8 {
  grid-area: block-8;
}
</style>
