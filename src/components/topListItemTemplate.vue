<script>
import $ from 'jquery'

export default {
  props: {
    TopListItem: {
      type: Object,
      default () {
        return {}
      }
    },
    maxIndex: {
      type: Number,
      default: 5
    },
    minIndex: {
      type: Number,
      default: 1
    }
  },
  data () {
    return {
      checkArr: []
    }
  },
  methods: {
    onRemoveFromTop () {
      this.$emit('remove-sort')
    },
    checkValue (id) {
      // 1. 介於1~5 V
      // 2. 不能是整數以外 V
      // 3. 不能重複 V
      const regPos = /^[0-9]+.?[0-9]*/
      const val = $(`#sortNum-${id}`).val()
      if (regPos.test(val)) {
        this.ovarLimit(id, val)
        this.checkRepeatVal(id)
        return true
      } else {
        $(`#sortNum-${id}`).val('')
        console.log('請輸入1~5的數字')
        return false
      }
    },
    ovarLimit (id, val) {
      if (val > this.maxIndex || val < this.minIndex) {
        $(`#sortNum-${id}`).val('')
        console.log('請輸入1~5的數字')
      }
    },
    checkRepeatVal (id) {
      const $selectedInputVal = parseInt($(`#sortNum-${id}`).val())
      this.filterItemSelf(id)
      this.checkArr.forEach(item => {
        const itemNum = parseInt($(item).val())
        if (itemNum === $selectedInputVal) {
          $(`#sortNum-${id}`).val('')
          console.log('有重複')
        }
      })
    },
    filterItemSelf (id) {
      const $selectedInput = $(`#sortNum-${id}`)
      const $input = $('.sortNum')
      const $inputArr = Array.from($input)
      $inputArr.filter(item => {
        if ($(item).attr('id') !== $selectedInput.attr('id')) {
          this.checkArr.push(item)
          return this.checkArr
        }
      })
    }
  },
  computed: {
    // getValue (id) {
    //   const $selectedInputVal = $(`#sortNum-${id}`).val()
    //   return $selectedInputVal
    // }
  }
}
</script>

<template>
  <tr :data-order="TopListItem.inputValue">
    <td>
      <input
        type="text"
        :id="`sortNum-${TopListItem.id}`"
        class="form-control sortNum"
        style="width: 2.5em"
        name="index[]"
        value=""
        @blur="checkValue(TopListItem.id)"
      />
      <input type="hidden" name="id[]" :value="TopListItem.inputValue" />
    </td>
    <td>{{ TopListItem.title }}</td>
    <td>
      <button type="button" class="btn btn-secondary"
        :data-id="TopListItem.id"
        :data-title="TopListItem.title"
        @click="onRemoveFromTop"
      >
        取消排序
      </button>
    </td>
  </tr>
</template>
