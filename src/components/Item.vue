<template>
  <li :style="{backgroundColor:bgColor}" @mouseenter="handleEnter(true)" @mouseleave="handleEnter(false)">
    <label>
      <input type="checkbox" v-model="todo.complete"/>
      <span>{{todo.title}}</span>
    </label>
    <button class="btn btn-danger" v-show="isShow" @click ="removeItem">删除</button>
  </li>
</template>

<script>
import pubsub from 'pubsub-js'
export default {
  props: {
    todo: Object,
    // deleteTodo:Function,
    index:Number
  },
  data () {
    return {
      bgColor: 'white',
      isShow: false
    }
  },
  methods: {
    handleEnter (isEnter) {
      // if (isEnter) {
      //   this.bgColor = '#ccc'
      //   this.isShow = true
      // } else {
      //   this.bgColor = '#fff'
      //   this.isShow = false
      // }
      this.bgColor = isEnter ? '#ccc' : '#fff'
      this.isShow = isEnter
    },
    removeItem () {
      const {todo,deleteTodo,index} = this;
      if(`确定删除${todo.title}吗`) {
        // deleteTodo(index);
        //发布消息（）
        pubsub.publish('deleteTodo',index)

      }
    }
  }
}
</script>

<style scoped>
  li {
    list-style: none;
    height: 36px;
    line-height: 36px;
    padding: 0 5px;
    border-bottom: 1px solid #ddd;
  }

  li label {
    float: left;
    cursor: pointer;
  }

  li label li input {
    vertical-align: middle;
    margin-right: 6px;
    position: relative;
    top: -1px;
  }

  li button {
    float: right;
    margin-top: 3px;
  }

  li:before {
    content: initial;
  }

  li:last-child {
    border-bottom: none;
  }
</style>
