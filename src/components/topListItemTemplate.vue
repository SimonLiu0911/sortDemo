<script>
export default {
  props: {
    title: {
      type: String,
      default: ''
    },
    inputValue: {
      type: Number,
      default: 0
    }
  },
  computed: {
    curValue () {
      if (this.inputValue < 1) {
        return ''
      }
      return this.inputValue.toString()
    }
  },
  methods: {
    changeValue ($event) {
      const value = $event.target.value

      if (/^\d+$/.test(value)) {
        this.$emit('change', parseInt(value))
        $event.target.blur()
      } else {
        $event.target.value = ''
      }
    }

  }
}
</script>

<template>
  <tr>
    <td>
      <input
        type="text"
        class="form-control sortNum"
        style="width: 2.5em"
        :value="curValue"
        @input="changeValue"
      />
    </td>
    <td>{{ title }}</td>
    <td>
      <button
        type="button"
        class="btn btn-secondary"
        @click="$emit('removeSort')"
      >
        取消排序
      </button>
    </td>
  </tr>
</template>
