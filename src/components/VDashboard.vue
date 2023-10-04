<template>
    <div class="panel">
        <div class="img-blur">
            <div class="blur"></div>
            <img class="bg-image" src="../assets/nicebg.jpg" />
        </div>
        <div 
            class="skeleton"
            style="width: 190px; height: 26px;"
        >
            <div class="loading">
            </div>
        </div>

        <div 
            class="skeleton"
            style="width: 155px; height: 10px;"
        >
            <div class="loading">
            </div>
        </div>
        <div 
            class="skeleton"
            style="width: 190px; height: 10px;"
        >
            <div class="loading">
            </div>
        </div>
        <div 
            class="skeleton"
            style="width: 170px; height: 10px;"
        >
            <div class="loading">
            </div>
        </div>
        <div 
            class="skeleton"
            style="width: 160px; height: 10px;"
        >
            <div class="loading">
            </div>
        </div>
        <div 
            class="skeleton"
            style="width: 140px; height: 10px;"
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
    </div>
</template>

<script setup>

</script>

<style lang="scss" scoped>
.panel{
  z-index: 1000;
  display: grid;
  justify-items: center;
  background-color: $base-grey;
  border-radius: 10px;
  border: 1px solid $border-grey;
}
.img-blur{
    position: relative;
    margin-top: 14px;
}
.blur {
  border-radius: 10px;
  position: absolute;
  display: block;
  bottom: 25px;
  left: 0;
  right: 0;
  top: 0;
  -webkit-backdrop-filter: blur(5px);
  backdrop-filter: blur(5px);
  background-color: rgba(0, 0, 0, 0.1);
}
.bg-image{
  border-radius: 10px;
}

</style>