<template>
  <div class="modal">
      <div class="card-modal modal-content py-6 px-4 text-center w-3/4 md:w-2/4 bg-white rounded-md">
        <div class="float-right" @click='emitclose'>
          <span class="iconify cancel text-red-500" data-icon="ic:sharp-cancel" data-inline="false"></span>
        </div>
        <div class=" mt-20 w-1/3 md:w-1/4 mx-auto lg:pl-4">
          <span class="iconify check text-green-500 block" data-icon="bx:bxs-check-circle" data-inline="false"></span>
        </div>
        <p class=" mt-3 font-bold">Payment complete!</p>
      </div>
    </div>
</template>

<script>
export default {
 methods:{
   emitclose(){
     this.$emit('closeModal')
   }
 }
}
</script>

<style scoped>
.card-modal{
  min-height: 300px;
}
.cancel{
  font-size: 25px
}
.check{
  font-size: 90px
}
</style>