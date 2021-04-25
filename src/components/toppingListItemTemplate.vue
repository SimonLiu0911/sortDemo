<script>
export default ({
  props: {
    ToppingListItem: {
      type: Object,
      default: () => {
        return {}
      }
    },
    btnActive: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {}
  },
  methods: {
    onAddToTop () {
      this.$emit('add-sort')
    }
  }
})
</script>

<template>
  <tr type="text/html">
    <td>{{ ToppingListItem.title }}</td>
    <td>
      <button
        type="button"
        class="btn btn-primary"
        :data-id="ToppingListItem.id"
        :data-title="ToppingListItem.title"
        @click="onAddToTop"
        :disabled="btnActive"
      >
        加入排序
      </button>
    </td>
  </tr>
</template>
