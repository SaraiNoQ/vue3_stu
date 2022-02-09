<template>
  <div class="container" @mousedown="mouseDown">
    <h1>ToDoList</h1>

    <h3>unFinished Task:</h3>
    <ul class="list-group">
      <template v-for="(item, index) in lists">
        <li
          :key="index"
          class="list-group-item d-flex justify-content-between"
          v-if="!item.checked"
        >
          <div class="form-group form-check mb-0">
            <input
              type="checkbox"
              class="form-check-input"
              v-model="item.checked"
              @click="() => item.checked = !item.checked"
            >
            <label
              class="form-check-label"
              v-if="!item.isEdit"
              @dblclick="showEdit(item, index)"
            >
              {{item.name}}
            </label>
            <label
              class="form-check-label"
              :for="'item-'+index"
              @keydown.enter="onblur()"
              v-else
            >
              <input type="text" v-model="editValue" />
            </label>
          </div>
          <button type="button" class="close mb-2" aria-label="Close" @click="remove(index)">
            <span aria-hidden="true">&times;</span>
          </button>
        </li>
      </template>
    </ul>

    <h3>Finished Task:</h3>
    <ul class="list-group">
      <li
        v-for="(item, index) in finished"
        :key="'finished-'+index"
        class="list-group-item"
      >
        <div class="form-check">
          <input
            type="checkbox"
            class="form-check-input"
            :id="'finished-'+index"
            v-model="item.checked"
            disabled
          >
          <label
            class="form-check-label"
            :for="'finished-'+index"
          >
            {{item.name}}
          </label>
        </div>
      </li>
    </ul>

    <h3>Add Task:</h3>
    <div class="form-group">
      <input
        type="text"
        class="form-control"
        id="add"
        placeholder="add new task"
        v-model="value"
        @keydown.enter="add"
      >
    </div>
    <button class="btn btn-primary btn-lg btn-clock" type="button" @click="add">ADD</button>
  </div>
</template>

<script>
import { reactive, toRefs } from '@vue/reactivity'
import { computed } from '@vue/runtime-core'
// import { ref } from 'vue'

export default {
  name: 'Home',
  setup () {
    let editIndex = 0
    // const myinput = ref(null)
    //  * 1. 加入checkbox -> checked
    //  * 2. 统计那些 -> checked -> finished列表
    //  * 3. add添加item -> item数据结构 -> name, checked, isEdit
    const add = () => {
      state.lists.push({
        name: state.value,
        checked: false,
        isEdit: false
      })
      state.value = ''
    }
    //  * 4. 双击进行编辑
    const showEdit = (item, index) => {
      editIndex = index
      item.isEdit = true
      state.editValue = item.name
    }

    // const mouseDown = (e) => {
    //   console.log(e.target)
    //   console.log(myinput)
    //   if (myinput.value && !e.target.contains(myinput.value)) {
    //     state.lists[editIndex] = {
    //       name: state.editValue,
    //       checked: false,
    //       isEdit: false
    //     }
    //   }
    // }
    const onblur = () => {
      state.lists[editIndex] = {
        name: state.editValue,
        checked: false,
        isEdit: false
      }
    }
    //  * 5. 删除功能 -> 删除特定index的元素
    const remove = (index) => {
      state.lists.splice(index, 1)
    }

    const state = reactive({
      value: '',
      editValue: '',
      lists: [
        {
          name: 1,
          checked: false,
          isEdit: false
        },
        {
          name: 2,
          checked: false,
          isEdit: false
        },
        {
          name: 3,
          checked: false,
          isEdit: false
        }
      ],
      finished: computed(() => state.lists.filter((item) => item.checked === true)),
      add,
      showEdit,
      onblur,
      remove
    })
    return toRefs(state)
  }
}
</script>
