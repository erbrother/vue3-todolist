<template>
  <div class="container"
       @mousedown="mousedown">
    <h1>ToDoList</h1>
    <h3>共有<span class="text-primary">{{lists.length}}</span>个任务,其中<span class="text-success">{{finished.length}}</span>已完成</h3>
    <h3>未完成列表</h3>
    <ul class="list-group">
      <template v-for="(item, index) in lists">
        <li class="list-group-item d-flex justify-content-between"
            v-if="!item.checked"
            :key="index">
          <div class="form-check form-check mb-0">
            <input class="form-check-input"
                   type="checkbox"
                   @click="() => item.checked = !item.checked"
                   :id="'item-' + index"
                   v-model="item.checked"
                   value="option1">
            <label class="form-check-label"
                   v-if="!item.isEdit"
                   @dblclick="showEdit(item,index)">{{item.name}}</label>
            <label class="form-check-label"
                   v-else
                   :for="'item-' + index">
              <input type="text"
                     ref="myinput"
                     v-model="editorValue">
            </label>
          </div>
          <button type="button" class="close" aria-label="Close" @click="remove(index)">
            <span aria-hidden="true">&times;</span>
          </button>
        </li>
      </template>

    </ul>
    <h3>已完成列表</h3>
    <ul class="list-group">
      <li class="list-group-item"
          v-for="(item, index) in finished"
          :key="index">
        <div class="form-check form-check">
          <input class="form-check-input"
                 type="checkbox"
                 :id="'item-' + index"
                 v-model="item.checked"
                 disabled
                 value="option1">
          <label class="form-check-label"
                 :for="'item-' + index">{{item.name}}</label>
        </div>
      </li>
    </ul>
    <h3>添加新的任务❤️</h3>
    <div class="form-group">
      <label for="add"></label>
      <input type="text"
             class="form-control"
             id="add"
             placeholder="添加新的Task"
             v-model="value"
             @keydown.enter="add">
    </div>
    <button type="button"
            class="btn btn-primary btn-lg btn-block"
            @click="add">确定添加</button>
  </div>
</template>

<script>
import { reactive, toRefs, computed, ref } from 'vue'

// @ is an alias to /src
export default {
  name: 'Home',
  setup() {
    const myinput = ref(null)
    let editIndex = 0
    // 1. 加入checkbox -> checked
    // 2. 统计哪些 -> checked -> finish 列表
    // 3. add添加item
    // 4. 双击进行编辑
    // 5. 删除元素
    const add = () => {
      state.lists.push({
        name: state.value,
        checked: false,
        isEdit: false
      })
      state.value = ''
    }
    const showEdit = (item, index) => {
      editIndex = index

      item.isEdit = true
      state.editorValue = item.name
    }

    const mousedown = e => {
      if (myinput.value && !e.target.contains(myinput.value)) {
        state.lists[editIndex] = {
          name: state.editorValue,
          checked: false,
          isEdit: false
        }
      }
    }
    const remove = (index) => {
      state.lists.splice(index, 1)
    }
    const state = reactive({
      value: '',
      editorValue: '',
      lists: [
        {
          name: '1',
          checked: false,
          isEdit: false
        },
        {
          name: '2',
          checked: false,
          isEdit: false
        },
        {
          name: '3',
          checked: false,
          isEdit: false
        }
      ],
      finished: computed(() =>
        state.lists.filter(item => item.checked === true)
      ),
      unfinished: computed(() =>
        state.lists.filter(item => item.checked === false)
      )
    })

    return { ...toRefs(state), add, showEdit, mousedown, myinput , mousedown, remove}
  }
}
</script>
