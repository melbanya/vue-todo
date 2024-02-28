<template>
  <div id="wrap">
    <main class="app">
      <h1>Simple to-do list</h1>
      <!-- 자바스크립트 사용시 {{  }} 사용 -->
      <div class="todo_count">
      </div>
      <div class="todo_add">
      </div>
      <ul class="todo_list">
        <li v-for="(todo, i) in data.items" v-bind:key="todo.id" v-bind:class="{ completed: todo.completed }">
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
  data.items.push({
    id: data.items.length,
    text: data.newItem,
    completed: false,
  });
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

  height: 100vh;
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
}

.todo_count {
  margin-bottom: 10px;
}

.todo_add input[type='text'] {
  width: calc(100% - 60px);
  border: 1px solid #ddd;
  border-radius: 4px;
}

.add_btn {
  padding: 0 10px;
  color: #fff;
  background: #333;
  height: 40px;
  border-radius: 4px;
  margin-left: 10px;
  border: none;
}

.todo_list {
  margin-top: 20px;
}

.todo_list li {
  display: flex;
  align-items: center;
  margin-bottom: 10px;
}

.todo_list li.completed label {
  text-decoration: line-through;
}

.todo_list label {
  width: 100%;
}

.remove_btn {
  height: 32px;
  padding: 0 10px;
  background: none;
  border: 1px solid var(--point-color1);
  color: var(--point-color1);
  border-radius: 4px;
  margin-left: 20px;
}
</style>
