<template>
  <div id="wrap">
    <main class="app">
      <h1>Simple to-do list</h1>
      <div class="todo_count">
        완료: {{ isComplete }} / 할 일: {{ totalItems }}
      </div>
      <div class="todo_add">
        <!-- v-on:이벤트명 이벤트 연결 디렉티브 -->
        <!-- v-model로 데이터 양방향 연결 -->
        <input type="text" title="할 일을 입력하세요" placeholder="할 일을 입력하세요" v-model="data.newItem" />
        <!-- v-on:keyup.enter="addItem()" 삭제 -->
        <button type="button" class="add_btn" v-on:click="addItem()">추가</button>
      </div>
      <ul class="todo_list">
        <!-- v-bind 디렉티브: 데이터 단방향 연결 -->
        <!-- 리스트는 고유값을 key속성에 연결하여 순서가 변경되도 구별가능하도록 -->
        <!-- todo.completed가 true면 completed문자열이 클래스명이 됨 -->
        <li v-for="(todo, i) in data.items" v-bind:key="todo.id" v-bind:class="{ completed: todo.completed }">
          <!-- id가 달라야되므로 id속성과 todo.id값을 단방향 연결 -->
          <input type="checkbox" v-bind:id="`check${todo.id}`" v-model="todo.completed" />
          <!-- 라벨클릭시 for로 연결된 체크박스에서 true, false가 발생 -->
          <!-- v-model로 연결된 데이터의 내용이 true, false로 변경됨 -->
          <label v-bind:for="`check${todo.id}`">{{ todo.text }}</label>
          <button type="button" class="remove_btn" v-on:click="deleteItem(todo.id)">
            -
          </button>
        </li>
      </ul>
    </main>
  </div>
</template>
<script setup>
// vue3 컴포지션 api 가져오기
// 객체구조분해로 필요한 함수 불러오기
import { reactive, computed } from 'vue';

// reactive함수: 객체 데이터에 반응성을 주입하여 값이 변경되면 리렌더링이 되어 화면이 갱신됨
const data = reactive({
  newItem: '',
  items: [],
});

// 할일 완료 요소의 배열 길이 구하기
// 렌더링간에 새로운 변수가 생성되며 변경된 값이 변수에 각각 할당됨
// 완료된 요소를 새로운 배열로 만들고 갯수를 구해줌
const isComplete = computed(
  () => data.items.filter((item) => item.completed).length
);

// computed함수: data.items가 변경될때만 함수를 실행(결과를 캐싱함)
const totalItems = computed(() => data.items.length);

// 할일 추가
// 할일 객체데이터는 고유 id추가하여 데이터를 구분할 수 있도록 해야함
function addItem() {
  if (data.newItem === '') {
    alert('투두리스트 내용을 입력해주세요');
  } else {
    data.items.push({
      id: data.items.length,
      text: data.newItem,
      completed: false,
    });
  }
  // 할일 추가뒤 입력필드와 연결된 데이터를 빈문자로 초기화
  data.newItem = '';
}

// 할일 삭제
function deleteItem(id) {
  // id에 맞는 아이템을 찾기
  const itemToDelete = data.items.find((item) => item.id === id);

  // 삭제할 아이템의 위치 찾기
  const idx = data.items.indexOf(itemToDelete);

  // splice(시작위치, 삭제갯수)
  data.items.splice(idx, 1);
}
</script>
<style scoped>
#wrap {
  background: url(./assets/images/ee.jpg) no-repeat 0 0 / cover;
  object-fit: cover;
  height: 100vh;
  width: 100%;
}

.app {
  padding: 40px;
  max-width: 700px;
  margin: 0 auto;
}

.app h1 {
  font-size: 30px;
  font-weight: 700;
  margin-bottom: 20px;
  padding: 20px 20px 20px 0;
  border-radius: 5px;
  color: #bc2e1e;
  text-shadow: 0 1px 0px #378ab4, 1px 0 0px #5dabcd, 1px 2px 1px #378ab4, 2px 1px 1px #5dabcd, 2px 3px 2px #378ab4, 3px 2px 2px #5dabcd, 3px 4px 2px #378ab4, 4px 3px 3px #5dabcd, 4px 5px 3px #378ab4, 5px 4px 2px #5dabcd, 5px 6px 2px #378ab4, 6px 5px 2px #5dabcd, 6px 7px 1px #378ab4, 7px 6px 1px #5dabcd, 7px 8px 0px #378ab4, 8px 7px 0px #5dabcd;
  position: relative;
  z-index: 99;
}

.app h1::before {
  content: '';
  position: absolute;
  left: 0px;
  bottom: 11px;
  width: 250px;
  height: 20%;
  background: #edde9c;
  z-index: -1;
}

.todo_count {
  display: inline-block;
  margin-bottom: 10px;
  background: #fff;
  padding: 7px 10px;
  font-weight: 600;
}

.todo_add {
  width: 100%;
}

.todo_add input[type='text'] {
  width: calc(100% - 60px);
  border: 3px solid #000;
}

.add_btn {
  padding: 0 10px;
  color: #fff;
  background: #000;
  height: 40px;
  margin-left: 10px;
  border: 0;
}

.todo_list {
  margin-top: 20px;
}

.todo_list li {
  display: flex;
  align-items: center;
  padding: 10px;
  margin-bottom: 10px;
  background: #fff;
}

.todo_list li.completed label {
  text-decoration: line-through;
}

.todo_list label {
  width: 100%;
  font-weight: 600;
}

.remove_btn {
  height: 32px;
  padding: 0 10px;
  background: #bc2e1e;
  color: #fff;
  border: 0;
  margin-left: 20px;
  border-radius: 2px;
  vertical-align: middle;
}

input[type='checkbox']+label::before {
  border: 2px solid #000;
  margin-right: 10px;
}
</style>