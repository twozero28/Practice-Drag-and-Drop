<script setup>
import {nextTick, ref} from "vue";
import {useDraggable} from "@vueuse/core";

const start = ref(false)
const dragStart = (event) => {
  const elements = document.querySelectorAll(`[data-index="1"]`);
  const newElement = document.createElement('div');
  newElement.id = 'dragImage'
  newElement.classList.add('flex', 'absolute');
  newElement.innerHTML = `${elements[0].outerHTML}${elements[1].outerHTML}`
  newElement.style.width = `${elements[0].offsetWidth + elements[1].offsetWidth}px`
  document.body.appendChild(newElement);
  console.log('newElement', newElement.style.opacity)
  const hideNode = newElement.cloneNode(true);
  hideNode.style.opacity = 0
  hideNode.id = 'hide-node'
  document.body.appendChild(hideNode);
  event.dataTransfer.setDragImage(hideNode, 0, 0)
  console.log('dragStart', event)
}
const drag = (event) => {
  start.value = true
  const dragImage = document.getElementById('dragImage');
  dragImage.style.top = `${event.y + 1}px`
  dragImage.style.left = `${event.x + 1}px`
}
const onDrop = (event) => {
  start.value = false
  console.log('onDrop', event)
}

const dragEnd = () => {
  document.getElementById('dragImage').remove()
  document.getElementById('hide-node').remove()
  start.value = false;
}
</script>

<template>
  <div class="flex flex-col items-center justify-center h-screen space-x-10"
       @drop.prevent="onDrop"
       @dragenter.prevent
       @dragover.prevent
  >
    <div class="w-full bg-gray-300 h-16" @dragover.prevent="() => console.log('으아아아악')">
      드랍 존
    </div>
    <div class="flex flex-row w-full" >
      <div v-show="!start" class="w-1/2 border bg-blue-200 flex flex-row" data-index="1">
        <div @dragstart="dragStart" @drag="drag" @dragend="dragEnd" draggable="true">d</div>
        <div class="ml-20">
          Hello 👋🏼
        </div>
      </div>
      <div v-show="!start" class="w-1/2 border bg-blue-200" data-index="1">
        bye 👋🏼
      </div>
    </div>
  </div>
</template>

<style scoped>

</style>
