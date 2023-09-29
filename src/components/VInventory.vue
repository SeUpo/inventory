<template>
    <div class="stock">
        <div 
            v-for="cell in stock"
            :key="cell"
            @drop="dropItem($event, cell)"
            @dragenter.prevent
            @dragover.prevent
            class="cell"
        >
            <div 
                v-for="item in cell"
                :key="item.id"
                @dragstart="dragItem($event, item)"
                @click="modal(item.id)"
                class="item"
            >
                <div 
                    v-if="item.quantity > 0"
                    class="squares"
                >
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
            :showModal="showModal"
            :infoIndex="infoIndex"
            :item="item"
        />
    </div>
</template>

<script setup>
import VModal from './VModal.vue'
import { ref } from 'vue'
 
const stock = ref([
  {item: { id: 0, title: 'Item 1', quantity: 4, color: 'green', }, },
  {item: { id: 1, title: 'Item 2', quantity: 2, color: 'orange', }, },
  {item: { id: 2, title: 'Item 3', quantity: 5, color: 'purple', }, },
  {item: {}},
  {item: {}},
  {item: {}},
  {item: {}},
  {item: {}},
  {item: {}},
  {item: {}},
  {item: {}},
  {item: {}},
  {item: {}},
  {item: {}},
  {item: {}},
  {item: {}},
  {item: {}},
  {item: {}},
  {item: {}},
  {item: {}},
  {item: {}},
  {item: {}},
  {item: {}},
  {item: {}},
  {item: {}},
])

const dragItem = (event, item) => {
  event.dataTransfer.dropEffect = 'move'
  event.dataTransfer.effectAllowed = 'move'
  event.dataTransfer.setData('itemID', item.id)
}

const dropItem = (event, cell) => {
  const itemID = event.dataTransfer.getData('itemID')
  const item =  stock.value.find((a) => a.item.id == itemID)
  const oldCell =  stock.value.findIndex((a) => a == item)
  const newCell =  stock.value.findIndex((a) => a == cell)

  if(!cell.item.title) {
    stock.value[newCell] = item 
    stock.value[oldCell] = {item: {}}
  }
}

let infoIndex = ref(0)
let showModal = ref(false)
let item = ref({})

const closeModal = () => {
    showModal.value = false
}

const modal = (index) => {
    showModal.value = true
    infoIndex.value = index
    item.value = stock.value[index].item
}

const updateItemQuantity = (decrementQuantity) => {
    let newQuantity = stock.value[infoIndex.value].item.quantity -= decrementQuantity
    if(newQuantity <= 0){
        stock.value[infoIndex] = {item: {}}
        showModal.value = false
    } 
}
</script>

<style lang="scss" scoped>
.stock{
    position: relative;
    overflow: hidden;
    outline: 1px solid #4D4D4D;
    border-radius: 12px;
    list-style-type: none;
    place-content: space-between;
    display: grid;
    grid-template-columns: 1fr 1fr 1fr 1fr 1fr;
}
.stock div:nth-child(1) {
    border-top-left-radius: 10px;
}
.stock div:nth-child(5) {
    border-top-right-radius: 10px;
}
.stock div:nth-child(21) {
    border-bottom-left-radius: 10px;
}
.stock div:nth-child(25) {
    border-bottom-right-radius: 10px;
}
.cell{
    position: relative;
    align-items: center;
    height: 100px;
    width: 105px;
    background-color: $base-grey;
    padding: 5px;
    outline: 1px solid $border-grey;
    justify-content: center;
}
.item{
    cursor: pointer;
}
.squares .square-1,.square-2{
    position: absolute;
    width: 48px;
    height: 48px;
}
.square-1{
    top: 29px;
    left: 26px;
}
.square-2{
    top: 23px;
    left: 32px;
}
.quantity{    
    position: absolute;
    border-top-left-radius: 6px;
    outline: 1px solid $border-grey;
    color: #4D4D4D;
    padding: 0px 5px;
    right: 0px;
    bottom: 0px;
    font-size: 10px;
}
</style>