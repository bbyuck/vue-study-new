<template>
  <div>
    <ul>
      <!-- v-for 구문을 통해 markup tag 반복문을 돌릴 수 있음. 반복문 사용 시 v-bind:key=""를 이용해 각 태그마다 key값 설정 필요 -->
      <li v-for="(todoItem, idx) in todoItems" v-bind:key="idx" class="shadow">
        <i
          class="checkBtn fas fa-check"
          v-bind:class="{ checkBtnComplted: todoItem.completed }"
          v-on:click="toggleComplete(todoItem)"
        ></i>
        <span v-bind:class="{ textCompleted: todoItem.completed }">
          {{ todoItem.item }}
        </span>
        <span class="removeBtn" v-on:click="removeTodo(todoItem, idx)">
          <i class="fas fa-trash-alt"></i>
        </span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  data() {
    return {
      todoItems: [],
    };
  },
  methods: {
    removeTodo: function (item, index) {
      // localStorage에서 삭제
      localStorage.removeItem(item);
      // 화면단 script에서 삭제
      this.todoItems.splice(index, 1);
    },
    toggleComplete: function (param) {
      param.completed = !param.completed;
      localStorage.removeItem(param.item);
      localStorage.setItem(param.item, JSON.stringify(param));
    },
  },
  // Vue lifecycle hook 중 하나 -> 이벤트 핸들러와 같은 역할 -> 이 컴포넌트가 생성되는 시점에 실행
  created: function () {
    if (localStorage.length > 0) {
      for (var i = 0; i < localStorage.length; i++) {
        if (localStorage.key(i) === "loglevel:webpack-dev-server") continue;
        this.todoItems.push(
          JSON.parse(localStorage.getItem(localStorage.key(i)))
        );
        // this.todoItems.push(localStorage.key(i));
        // console.log(localStorage.key(i));
      }
    }
  },
};
</script>

<style>
ul {
  list-style-type: none;
  padding-left: 0px;
  margin-top: 0;
  text-align: left;
}
li {
  display: flex;
  min-height: 50px;
  height: 50px;
  line-height: 50px;
  margin: 0.5rem 0;
  padding: 0 0.9rem;
  background: white;
  border-radius: 5px;
}

.checkBtn {
  line-height: 45px;
  color: #62acde;
  margin-right: 5px;
}

.checkBtnCompleted {
  color: #b3adad;
}

.removeBtn {
  margin-left: auto;
  color: #de4343;
}

.textCompleted {
  text-decoration: line-through;
  color: #b3adad;
}
</style>