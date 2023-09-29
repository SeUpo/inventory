<template>
    <div>
        <transition name="fade" appear>
            <div class="modal-overlay" 
                v-if="showModal" 
                @click="$emit('closeModal', closeModal)"
            >
            </div>
        </transition>
        <transition name="fade-left" appear>
            <div class="modal" 
                v-if="showModal"
            >
                <div class="fr-1">
                    <img 
                        class="close-btn"
                        @click="$emit('closeModal', closeModal)"
                        src="../assets/Vector.svg" 
                        alt="x"
                    />
                    <div class="squares">
                        <span :class="item.color">
                            <span class="square-1"></span>
                            <span class="square-2"></span>
                        </span>
                    </div>
                </div>

                <div class="fr-2">
                    <div class="divider"></div>
                    <div 
                        class="skeleton"
                        style="width: 211px; height: 30px; margin: 6px 0px 10px;"
                    >
                        <div class="loading">
                        </div>
                    </div>
                    <div 
                        class="skeleton"
                        style="width: 211px; height: 10px;"
                    >
                        <div class="loading">
                        </div>
                    </div>
                    <div 
                        class="skeleton"
                        style="width: 211px; height: 10px;"
                    >
                        <div class="loading">
                        </div>
                    </div>
                    <div 
                        class="skeleton"
                        style="width: 211px; height: 10px;"
                    >
                        <div class="loading">
                        </div>
                    </div>
                    <div 
                        class="skeleton"
                        style="width: 180px; height: 10px;"
                    >
                        <div class="loading">
                        </div>
                    </div>
                    <div 
                        class="skeleton"
                        style="width: 80px; height: 10px;"
                    >
                        <div class="loading">
                        </div>
                    </div>
                    <div class="divider"></div>
                </div>

                <div class="fr-2">
                    <button 
                        v-show="!deleteModal"
                        @click="showDeleteModal"
                        class="btn-delete"
                    >
                        Удалить предмет
                    </button>
                    <transition name="fade-up" appear>
                        <div class="deleteModal-overlay" 
                            v-if="deleteModal" 
                        >
                        <label>
                            <input 
                                v-model.number="decrementQuantity" 
                                class="text-input"
                                type="text" 
                                placeholder="Введите количество"/>
                        </label>
                        <button 
                            @click="closeDeleteModal"
                            class="btn-cancel"
                        >
                            Отмена
                        </button>
                        <button 
                            @click="$emit('updateItemQuantity', decrementQuantity)"
                            class="btn-submit"
                        >
                            Подтвердить
                        </button>
                        </div>
                    </transition>
                </div>
            </div>
        </transition>
    </div>
</template>

<script setup>
import { ref } from 'vue'

const props = defineProps({
    showModal: {
        type: Boolean,
        required: true
    },
    item: {
        type: Object,
        required: true
    }
})

const decrementQuantity = ref();
const deleteModal = ref(false)
const showDeleteModal = () => {
    deleteModal.value = true
}
const closeDeleteModal = () => {
    deleteModal.value = false
}
</script>

<style lang="scss" scoped>
.modal{
    position: absolute;
    display: grid;
    grid-template-rows: 45% 40% 25%;
    justify-items: center;
    justify-content: center;
    top: 0;
    right: 0;
    bottom: 0;
    margin: auto;
    text-align: center;
    width: fit-content;
    max-width: 250px;
    padding: 2rem;
    outline: 1px solid $border-grey;
    background: #262626;
    z-index: 999;
    transform: none;
}
.fr-2{
    display: grid;
    justify-items: center;
    align-items: center;
}
.modal-overlay {
  content: '';
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 998;
  cursor: pointer;
}
.deleteModal-overlay{
    position: absolute;
    justify-items: center;
    justify-content: center;
    bottom: 0;
    right: 0;
    margin: auto;
    text-align: center;
    width: fit-content;
    max-width: 250px;
    padding: 20px;
    outline: 1px solid $border-grey;
    background: #262626;
    z-index: 999;
    transform: none;
}
.fade-left-enter-active {
  transition: all 0.3s ease-out;
}
.fade-left-leave-active {
  transition: all 0.3s ease-out;
}
.fade-left-enter-from,
.fade-left-leave-to {
  transform: translateX(100px);
  opacity: 0;
}
.fade-up-enter-active {
  transition: all 0.3s ease-out;
}
.fade-up-leave-active {
  transition: all 0.3s ease-out;
}
.fade-up-enter-from,
.fade-up-leave-to {
  transform: translateY(100px);
  opacity: 0;
}
.close-btn {
  position: absolute;
  top: 14px;
  right: 14px;
  cursor: pointer;
}
.squares .square-1,.square-2{
    position: absolute;
    width: 115.56px;
    height: 115.56px;
}
.square-1{
    top: 69.44px;
    left: 60px;
}
.square-2{
    top: 55px;
    left: 74.44px;
}
.text-input{
    padding: 12px;
    color: #fff;
    background-color: $base-grey;
    border: 1px solid $border-grey;
    border-radius: 4px;
    width: 210px;
    height: 40px;
}
.btn-delete{
    color: #fff;
    background-color: #FA7272;
    border: none;
    border-radius: 8px;
    width: 220px;
    height: 39px;
    top: 443px;
    left: 15px;
}
.btn-cancel{
    color: #000000;
    background-color: #fff;
    border: none;
    border-radius: 8px;
    width: 88px;
    height: 33px;
    margin: 20px 8px 0 0;
    padding: 8px 18px 8px 18px;
    border-radius: 8px;
}
.btn-submit{
    color: #fff;
    background-color: #FA7272;
    border: none;
    border-radius: 8px;
    width: 112px;
    height: 33px;
    margin: 20px 0 0 0;
    padding: 8px 15px 8px 15px;
    border-radius: 8px;
}
.btn-cancel, .btn-submit{
    box-shadow: 0px 0px 11px 4px rgba(187, 81, 77, 0.363);
}
</style>