<template>
  <!-- v-model="data 변수명" 을 활용해 데이터 바인딩이 가능 -->
  <!-- 데이터가 바인딩 되어 있으므로 화면 조작 <-> DOM 조작 쌍방향 모두 적용이 됨 -->
  <div class="inputBox shadow">
    <input type="text" v-model="newTodoItem" v-on:keyup.enter="addTodo" />
    <!-- click 했을 때 실행 될 메소드명 기입(이벤트 핸들러 -> v-on:event-name="method-name") -->
    <!-- <button v-on:click="addTodo">add</button> -->
    <span class="addContainer" v-on:click="addTodo">
      <i class="fas fa-plus addBtn"></i>
    </span>
  </div>
</template>

<script>
export default {
  // 컴포넌트단 data영역
  data() {
    return {
      newTodoItem: "",
      testData: "",
    };
  },
  methods: {
    addTodo() {
      if (this.newTodoItem === "" || this.newTodoItem === null) return;

      // 저장하는 로직
      // 브라우저의 localStorage에 key : value 형태로 저장
      // 이 컴포넌트에서 관리되는 (바인딩 되어있는) data 및 methods 영역에 접근하기 위해서는 this 키워드 필요
      var obj = {
        completed: false,
        item: this.newTodoItem,
      };

      localStorage.setItem(this.newTodoItem, JSON.stringify(obj));
      this.clearInput();
    },
    clearInput() {
      this.newTodoItem = "";
    },
    changeKeyword(e) {
      // 이와 같은 input event handler로 한글 바인딩 지연 해결 가능
      this.testData = e.target.value;
    },
  },
};
</script>

<style scoped>
input:focus {
  outline: none;
}
.inputBox {
  background: white;
  height: 50px;
  line-height: 50px;
  border-radius: 5px;
}
.inputBox input {
  border-style: none;
  font-size: 0.9rem;
}
.addContainer {
  float: right;
  background: linear-gradient(to right, #6478fb, #8763fb);
  display: block;
  width: 3rem;
  border-radius: 0 5px 5px 0;
}
.addBtn {
  color: white;
  vertical-align: middle;
}
</style>