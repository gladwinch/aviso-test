<template>
  <div class="container">
    <div class="blocks-wrapper">
      <v-block
        v-for="(block, i) in blocks"
        @click="activeBlockItem(i)"
        :active="block.active"
        :id="`block-${block.id}`"
      ></v-block>
    </div>
  </div>
</template>

<script setup>
let activeBlockSeqIndex = [];
const deactivationThreshold = 7;
const latency = 1000;

// block states with unique IDs.
const blocks = ref([
  { id: 1, active: false },
  { id: 2, active: false },
  { id: 3, active: false },
  { id: 4, active: false },
  { id: 5, active: false },
  { id: 6, active: false },
  { id: 7, active: false },
  { id: 8, active: false },
  { id: 9, active: false },
]);

// activate a block and add its index to the sequence tracker.
const activeBlockItem = (index) => {
  blocks.value[index].active = true;
  activeBlockSeqIndex.push(index);
};

// deactivate all blocks sequentially with a delay.
const deactiveAllBlocks = async () => {
  while (activeBlockSeqIndex.length) {
    await delay(latency);

    let lastItemIndex = activeBlockSeqIndex.pop();
    blocks.value[lastItemIndex].active = false;
  }
};

// create a delay
function delay(milliseconds) {
  return new Promise((resolve) => {
    setTimeout(resolve, milliseconds);
  });
}

// automatically deactivate blocks when the threshold is reached
watch(
  blocks,
  async (newValue) => {
    const updatedBlocks = toRaw(newValue);
    const activeBlocks = updatedBlocks.filter((b) => b.active);
    if (activeBlocks.length >= deactivationThreshold) {
      await deactiveAllBlocks();
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
#block-9 {
  grid-area: block-8;
}
</style>
