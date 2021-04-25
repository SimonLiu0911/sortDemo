<script>
import TopListItemTemplate from '../components/topListItemTemplate'
import ToppingListItemTemplate from '../components/toppingListItemTemplate'

export default {
  data () {
    return {
      sort: [],
      unsort: [
        {
          id: '21',
          title: '圖片1',
          inputValue: 1
        },
        {
          id: '22',
          title: '圖片2',
          inputValue: 2
        },
        {
          id: '23',
          title: '圖片3',
          inputValue: 3
        },
        {
          id: '24',
          title: '圖片4',
          inputValue: 4
        },
        {
          id: '25',
          title: '圖片5',
          inputValue: 5
        },
        {
          id: '26',
          title: '圖片6',
          inputValue: 6
        }
      ],
      MIN_INDEX: 1,
      MAX_INDEX: 5
    }
  },
  methods: {
    checkValid (sortIndex) {
      return sortIndex >= this.MIN_INDEX && sortIndex <= this.MAX_INDEX
    },
    sortByInput (item, sortIndex) {
      // TODO
      if (this.checkValid(sortIndex)) {
        this.sort.map((row, i, arr) => {
          if (row.inputValue === sortIndex) {
            arr[i].inputValue = 0
          }
        })
      } else {
        sortIndex = 0
      }
      this.changeItem(item.id, { inputValue: sortIndex })
    },
    changeItem (id, data = {}) {
      this.sort = this.sort.map(item => {
        if (item.id === id) {
          return { ...item, ...data }
        }
        return item
      })
    },
    // 增加到已分類
    addSort (id, title) {
      this.judgeLength()
      this.deleteItem(id, 'unsort')
      this.addItem(id, title, 'sort')
    },
    // 刪除已分類
    removeSort (id, title) {
      this.deleteItem(id, 'sort')
      this.addItem(id, title, 'unsort')
    },
    deleteItem (id, t) {
      this[t] = this[t].filter(item => {
        return item.id !== id
      })
    },
    addItem (id, title, t) {
      const item = {
        id: id,
        title: `${title}`,
        inputValue: this.sort.length + 1
      }
      this[t].push(item)
    },
    sortItem () {
      this.sort = this.sort.sort(function (a, b) {
        return a.inputValue - parseInt(b.inputValue)
      })
    },
    // 判斷已分類陣列長度
    judgeLength () {
      if (this.sort.length >= this.MAX_INDEX) {
        this.btnActive = true
        return this.btnActive
      }
    }
  },
  computed: {
    btnActive () {
      return this.sort.length >= this.MAX_INDEX
    },
    sorting () {
      return this.sort.slice().sort((a, b) => a.inputValue - b.inputValue)
    }
  },
  components: {
    TopListItemTemplate,
    ToppingListItemTemplate
  }
}
</script>

<template>
  <section>
    <div class="container-fluid pb-4 mt-3">
      <div class="row">
        <div class="col-12 col-md-8 col-xl-6">
          <div class="card">
            <pre>
              {{ sorting }}
            </pre>
            <div class="card-header">
              <h5 class="card-title h5">置頂排序中 (5 筆)</h5>
            </div>
            <div class="card-body">
              <table id="toppingList" class="table table-bordered table-hover">
                <thead>
                  <tr>
                    <th>順位</th>
                    <th>標題</th>
                    <th>操作</th>
                  </tr>
                </thead>
                <tbody>
                  <!-- Top -->
                  <TopListItemTemplate
                    class="TopListItemTemplate"
                    v-for="(TopListItem, index) in sorting"
                    :key="index"
                    :title="TopListItem.title"
                    :inputValue="TopListItem.inputValue"
                    @removeSort="removeSort(TopListItem.id, TopListItem.title)"
                    @change="($value) => sortByInput(TopListItem, $value)"
                  ></TopListItemTemplate>
                </tbody>
              </table>
            </div>
            <div class="card-footer text-right">
              <button type="submit" class="btn btn-success" @click="()=>{}">儲存</button>
            </div>
          </div>
        </div>
      </div>

      <hr class="my-4" />

      <div class="row">
        <div class="col-12 col-md-8 col-xl-6">
          <div class="card">
            <div class="card-header">
              <h5 class="card-title h5">未排序</h5>
            </div>

            <div class="card-body">
              <table id="topList" class="table table-bordered table-hover">
                <thead>
                  <tr>
                    <th>標題</th>
                    <th>操作</th>
                  </tr>
                </thead>
                <tbody>
                  <!-- Bottom -->
                  <ToppingListItemTemplate
                    class="ToppingListItemTemplate"
                    v-for="(ToppingListItem, index) in unsort"
                    :key="index"
                    :title="ToppingListItem.title"
                    @addSort="addSort(ToppingListItem.id, ToppingListItem.title)"
                    :btn-active="btnActive"
                  ></ToppingListItemTemplate>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>
  </section>
</template>
