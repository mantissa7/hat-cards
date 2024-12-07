<script setup lang="ts">
  import { ref } from "vue";
  import Card from "@/components/Card.vue"

  const dragover = ref(false);

  const title = ref('Wine');
  const description = ref('Wine or Cheese?');
  const illustrator = ref('Woss_');
  const edition = ref('1st Edititon');
  const cardnum = ref(1);
  const image = ref('');
  const translateX = ref(0);
  const translateY = ref(0);
  const scale = ref(1);

  const onZoomImg = (e: WheelEvent) => {
    if (!image) {
      return;
    }

    const trueScale = scale.value + e.deltaY * -0.01;

    // Clamp value
    scale.value = Math.min(Math.max(0.125, trueScale), 4);
  }

  const onMoveImg = (e: PointerEvent) => {
    // Check if buttons bitmask includes "left mouse"
    const isDrag = Boolean(e.buttons & 1);

    if (!isDrag) {
      return;
    }

    const x = e.movementX;
    const y = e.movementY;

    translateX.value += x;
    translateY.value += y;
  }

  const setImage = (f: File) => {
    image.value = URL.createObjectURL(f);
  }

  const onFile = async (e: Event) => {
    const t = e.target as HTMLInputElement;
    if (!t) {
      return null;
    }

    if (!t.files || !t.files?.length) {
      return null;
    }

    setImage(t.files[0]);
  }

  const onDragEnter = (e: DragEvent) => {
    dragover.value = true;
  };

  const onDrag = (e: DragEvent) => {
    e.preventDefault();
    dragover.value = true;
  };

  const onDragLeave = (e: DragEvent) => {
    e.preventDefault();
    dragover.value = false;
  };

  const onDrop = (e: DragEvent) => {
    e.preventDefault();
    dragover.value = false;

    if (e.dataTransfer?.items) {
      // Use DataTransferItemList interface to access the file(s)
      [...e.dataTransfer.items].forEach((item, i) => {
        // If dropped items aren't files, reject them
        if (item.kind === "file") {
          const file = item.getAsFile();
          if (!file) {
            console.warn(`No File Found for ${item.type}`);

            return;
          }
          setImage(file);
        }
      });
    } else {
      // Use DataTransfer interface to access the file(s)
      [...(e.dataTransfer?.files ?? [])].forEach((file, i) => {
        setImage(file);
      });
    }
  }

</script>

<template>
  <main>
    <form class="config">
      <label>
        <span>Title</span>
        <input
          type="text"
          name="title"
          v-model="title"
        >
      </label>
      <label>
        <span>Description</span>
        <input
          type="text"
          name="description"
          v-model="description"
        >
      </label>
      <label>
        <span>Image</span>
        <input
          type="file"
          name="file"
          @change="onFile"
        >
      </label>
      <label>
        <span>Illustrator</span>
        <input
          type="text"
          name="illustrator"
          v-model="illustrator"
        >
      </label>
    </form>
    <div
      id="img-crop-wrapper"
      :class="{ dragover: dragover }"
      @wheel="onZoomImg"
      @pointermove="onMoveImg"
      @dragenter="onDragEnter"
      @dragover="onDrag"
      @dragleave="onDragLeave"
      @drop="onDrop"
      draggable
    >
      <Card
        background=""
        :edition="edition"
        :src="image"
        rarity="common"
        :title="title"
        :description="description"
        :illustrator="illustrator"
        :cardnum="cardnum"
        :translateX="translateX"
        :translateY="translateY"
        :scale="scale"
      />
    </div>
  </main>
</template>

<style scoped>
  main {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 40px;

    .config {
      display: grid;
      align-content: start;
      gap: 16px;
    }

    #img-crop-wrapper {
      justify-self: start;
      user-select: none;

      &.dragover {
        /* opacity: 0; */
        /* pointer-events: none; */
        /* position: absolute; */
        /* inset: 0; */
        border: 2px dotted #fff;
        /* display: grid; */
        /* place-items: center; */
        /* background: rgba(23, 23, 23, 0.7); */
        /* z-index: 10; */
      }
    }
  }
</style>