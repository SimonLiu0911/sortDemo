<script>
export default ({
  props: {
    title: {
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
})
</script>

<template>
  <tr type="text/html">
    <td>{{ title }}</td>
    <td>
      <button
        type="button"
        class="btn btn-primary"
        @click="$emit('addSort')"
        :disabled="btnActive"
      >
        加入排序
      </button>
    </td>
  </tr>
</template>
