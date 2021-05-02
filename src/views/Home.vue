<script>
import TopListItemTemplate from '../components/topListItemTemplate'
import ToppingListItemTemplate from '../components/toppingListItemTemplate'

export default {
  data () {
    return {
      sort: [],
      unsort: [
        // {
        //   id: '21',
        //   imgURL: '/images/pic1.jpg',
        //   // imgURL: '圖片1',
        //   inputValue: 1
        // },
        // {
        //   id: '22',
        //   imgURL: '/images/pic2.jpg',
        //   inputValue: 2
        // },
        // {
        //   id: '23',
        //   imgURL: '/images/pic3.jpg',
        //   inputValue: 3
        // },
        // {
        //   id: '24',
        //   imgURL: '/images/pic4.jpg',
        //   inputValue: 4
        // },
        // {
        //   id: '25',
        //   imgURL: '/images/pic5.jpg',
        //   inputValue: 5
        // },
        // {
        //   id: '26',
        //   imgURL: '/images/pic6.jpg',
        //   inputValue: 6
        // }
      ],
      image: null,
      preview: null,
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
    addSort (id, imgURL) {
      this.judgeLength()
      this.deleteItem(id, 'unsort')
      this.addItem(id, imgURL, 'sort')
    },
    // 刪除已分類
    removeSort (id, imgURL) {
      this.deleteItem(id, 'sort')
      this.addItem(id, imgURL, 'unsort')
    },
    deleteItem (id, t) {
      this[t] = this[t].filter(item => {
        return item.id !== id
      })
    },
    addItem (id, imgURL, t) {
      const item = {
        id: id,
        imgURL: `${imgURL}`,
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
    },
    // 上傳檔案
    previewImage () {
      const input = event.target
      if (input.files) {
        const reader = new FileReader()
        reader.onload = (e) => {
          this.preview = e.target.result
        }
        this.image = input.files[0]
        reader.readAsDataURL(input.files[0])
      }
    },
    // 儲存檔案
    storeImg (preview) {
      if (this.image !== null) {
        this.addItem(Date.now(), preview, 'unsort')
        this.preview = null
        this.image = null
      }
    },
    // 重設檔案
    resetImg () {
      this.preview = null
      this.image = null
    },
    // 刪除檔案
    deleteImg (id) {
      this.deleteItem(id, 'unsort')
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
                    :imgURL="TopListItem.imgURL"
                    :inputValue="TopListItem.inputValue"
                    @removeSort="removeSort(TopListItem.id, TopListItem.imgURL)"
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
                    :imgURL="ToppingListItem.imgURL"
                    :btn-active="btnActive"
                    @deleteImg="deleteImg(ToppingListItem.id)"
                    @addSort="addSort(ToppingListItem.id, ToppingListItem.imgURL)"
                  ></ToppingListItemTemplate>
                </tbody>
              </table>
            </div>
          </div>
        </div>
      </div>

      <hr class="my-4" />

      <form action="" enctype="multipart/form-data">
        <button type="reset" @click="resetImg" class="btn btn-secondary">取消</button>
        <button type="reset" @click="storeImg(preview)" class="btn btn-success mx-3">確認</button>
        <input id="imgInp" name="progressbarTW_img" type="file" accept="image/jpeg, image/png" @change="previewImage">
        <div class="mt-4" v-if="preview">
          <img :src="preview" width="670" alt="" />
        </div>
      </form>

    </div>
  </section>
</template>
