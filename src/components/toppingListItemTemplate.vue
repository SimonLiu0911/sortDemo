<script>
export default {
  props: {
    imgURL: {
      type: String,
      default: ''
    },
    btnActive: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {}
  }
}
</script>

<template>
  <tr type="text/html">
    <!-- <td>{{ imgURL }}</td> -->
    <td>
      <!-- {{ imgURL }} -->
      <div class="img-wrapper">
        <img :src="imgURL" />
      </div>
    </td>
    <td>
      <button
        type="button"
        class="btn btn-primary mr-2"
        @click="$emit('addSort')"
        :disabled="btnActive"
      >
        加入排序
      </button>
      <button
        type="button"
        class="btn btn-danger"
        @click="$emit('deleteImg')"
      >
        刪除
      </button>
    </td>
  </tr>
</template>

<style scoped>
.img-wrapper  {
  width: 200px;
  height: 200px;
}
img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center center;
}
</style>
