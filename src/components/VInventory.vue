<template>
  <div class="stock">
    <div
      v-for="(item, index) in stock"
      :key="index"
      @drop="dropItem($event, item)"
      @dragenter.prevent
      @dragover.prevent
      class="cell"
      :style="active(index)"
    >
      <div
        v-if="item.quantity"
        draggable="true"
        @dragstart="dragItem($event, item)"
        @drag="dragHandler"
        @dragend="dragEnd"
        @click="modal(item, index)"
      >
        <div class="squares">
          <span :class="item.color">
            <span class="square-1"></span>
            <span class="square-2"></span>
          </span>
          <div class="quantity">
            {{ item.quantity }}
          </div>
        </div>
      </div>
    </div>
    <VModal
      @updateItemQuantity="updateItemQuantity"
      @closeModal="closeModal"
      :currentItem="currentItem"
      :showModal="showModal"
    />
  </div>
  <div ref="dragPreview" class="drag-preview" :style="dragStyle"></div>
</template>

<script setup>
import VModal from './VModal.vue'
import { ref, onMounted, computed } from 'vue'

const stock = ref([
  { id: 0, quantity: 4, color: 'green' },
  { id: 1, quantity: 2, color: 'orange' },
  { id: 2, quantity: 5, color: 'purple' },
  {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}, {}
])

onMounted(() => {
  const savedStock = localStorage.getItem('stock')
  if (savedStock) {
    stock.value = JSON.parse(savedStock)
  }
})
const updateStock = (newStock) => {
  stock.value = newStock
  localStorage.setItem('stock', JSON.stringify(newStock))
}

let x = ref(null)
let y = ref(null)
const dragHandler = (event) => {
  x.value = event.pageX - 50
  y.value = event.pageY - 50
}
const dragStyle = computed(() => {
  return {
    top: `${y.value}px`,
    left: `${x.value}px`
  }
})
let dragPreview = ref(null)
let dragPreviewNode = ref(null)
const dragItem = (event, item) => {
  dragPreviewNode.value = event.target.cloneNode(true)
  dragPreview.value.appendChild(dragPreviewNode.value)
  setTimeout(function(){dragPreview.value.firstChild.style.visibility = 'visible'}, 10);
  event.dataTransfer.setDragImage(new Image(), 0, 0)
  event.dataTransfer.dropEffect = 'move'
  event.dataTransfer.effectAllowed = 'move'
  event.dataTransfer.setData('itemID', item.id)
}
const dragEnd = () => {
  x.value = null
  y.value = null
  dragPreview.value.removeChild(dragPreview.value.firstChild)
  dragPreview.value = null
  dragPreviewNode.value = null
}

const dropItem = (event, cell) => {
  const itemID = event.dataTransfer.getData('itemID')
  const item = stock.value.find((a) => a.id == itemID)
  const oldCell = stock.value.findIndex((a) => a == item)
  const newCell = stock.value.findIndex((a) => a == cell)
  if (!cell.quantity) {
    stock.value[newCell] = item
    stock.value[oldCell] = {}
  }
  updateStock(stock.value)
}

const isActive = ref(false)
const active = (index) => ({
  backgroundColor: index == currentItemIndex.value && showModal.value ? '#2F2F2F' : '#262626'
})

let showModal = ref(false)
let currentItem = ref({})
let currentItemIndex = ref({})

const closeModal = () => {
  isActive.value = false
  showModal.value = false
}

const modal = (item, index) => {
  isActive.value = true
  currentItem.value = item
  currentItemIndex.value = index
  showModal.value = true
}

const updateItemQuantity = (decrementQuantity) => {
  let newQuantity = (stock.value[currentItemIndex.value].quantity -= decrementQuantity)
  if (newQuantity <= 0) {
    stock.value[currentItemIndex.value] = {}
    showModal.value = false
  }
  updateStock(stock.value)
}
</script>

<style lang="scss" scoped>
.stock {
  position: relative;
  overflow: hidden;
  outline: 1px solid #4d4d4d;
  border-radius: 12px;
  list-style-type: none;
  place-content: space-between;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
  div:nth-child(1) {
    border-top-left-radius: 10px;
  }
  div:nth-child(5) {
    border-top-right-radius: 10px;
  }
  div:nth-child(21) {
    border-bottom-left-radius: 10px;
  }
  div:nth-child(25) {
    border-bottom-right-radius: 10px;
  }
  .cell {
    position: relative;
    align-items: center;
    height: 100px;
    width: 105px;
    background-color: $base-grey;
    padding: 5px;
    outline: 1px solid $border-grey;
    justify-content: center;
  }
}
.squares {
  .square-1,
  .square-2 {
    cursor: pointer;
    position: absolute;
    width: 48px;
    height: 48px;
  }
  .square-1 {
    top: 29px;
    left: 26px;
  }
  .square-2 {
    top: 23px;
    left: 32px;
  }
  .quantity {
    background: $base-grey;
    position: absolute;
    border-top-left-radius: 6px;
    outline: 1px solid $border-grey;
    color: #4d4d4d;
    padding: 0px 5px;
    right: 0px;
    bottom: 0px;
    font-size: 10px;
  }
}
.drag-preview {
  pointer-events: none;
  position: absolute;
  visibility: hidden;
  .squares {
    animation: color 1s infinite linear;
    background-color: $base-grey;
    border-radius: 24px;
    outline: 1px solid $border-grey;
    height: 100px;
    width: 105px;
  }
  @keyframes color {
    0% {
      background: $base-grey;
    }
    100% {
      background: $border-grey;
    }
  }
  .quantity {
    display: none;
  }
}
</style>
