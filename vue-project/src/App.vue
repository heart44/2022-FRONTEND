<template>
    <div id="app">
        <TodoHeader></TodoHeader>
        <TodoInput @childAddTodo="addTodo" @open="open"></TodoInput>
        <TodoList :propsItems="todoItems" @childRemoveTodo="removeTodo"></TodoList>   <!-- v-bind는 생략 가능 -->
        <TodoFooter @childClearTodo="clearTodo"></TodoFooter>
        <AlertModal :show="modalShow" header="알림창" body="내용을 입력해 주세요" @close="close"></AlertModal>
    </div>
</template>

<script>
    import TodoHeader from './components/todo/TodoHeader.vue';
    import TodoInput from './components/todo/TodoInput.vue';
    import TodoList from './components/todo/TodoList.vue';
    import TodoFooter from './components/todo/TodoFooter.vue';
    import AlertModal from './components/common/AlertModal.vue';

    export default {
    name: 'App',
    data() {
        return {
            todoItems: [],
            cnt: 0,
            modalShow: false,
        }
    },
    methods: {
        open() {
            this.modalShow = true;
        },
        close() {
            this.modalShow = false;
        },
        addTodo(todoItem) {
            // localStorage.setItem(todoItem, todoItem);
            this.todoItems.push({
                key: this.cnt++,
                value: todoItem
            });
            // this.changeValue();
        },
        removeTodo(key) {
            // localStorage.removeItem(todoItem);
            // this.todoItems.splice(idx, 1);
            this.todoItems.forEach((item, idx) => {
                if(item.key === key) {
                    this.todoItems.splice(idx, 1);
                }
            })
            // this.changeValue();
        },
        clearTodo() {
            // this.todoItems = [];
            // this.todoItems.length = 0;   //빈 배열로 만드는 방법
            this.todoItems.splice(0);
            this.cnt = 0;
            // this.changeValue();
            // localStorage.clear();
        },
        changeValue() {
            const json = JSON.stringify(this.todoItems);
            localStorage.setItem('todoItems', json);
            localStorage.setItem('cnt', this.cnt);
        }
    },
    created() {
        const json = localStorage.getItem('todoItems');
        if(json) {
            const todoItems = JSON.parse(json);
            todoItems.forEach(item => {
                this.todoItems.push(item);
            }); 
            const cnt = localStorage.getItem('cnt');
            this.cnt = cnt;
        }
        // if(localStorage.length) {
        //     for(let i=0; i<localStorage.length; i++) {
        //         this.todoItems.push(localStorage.key(i));
        //     }
        // }
    },
    watch: {
        todoItems: {
            deep: true,     //배열 안 쪽의 값이 변경된다는 것을 알려주는 것이 deep
            handler() {
                this.changeValue();
            }
        }
    },
    components: {   
        //'TodoHeader': TodoHeader 이렇게 적어야하지만 
        //key값과 value값이 같으면 아래처럼 적어도 됨
        TodoHeader, 
        TodoInput, 
        TodoList, 
        TodoFooter,
        AlertModal,
    }
}
</script>

<style>
    body { text-align: center; background-color: #F6F6F8; }
    input { border-style: groove; width: 200px; }
    button { border-style: groove; }
    .shadow { box-shadow: 5px 10px 10px rgba(0, 0, 0, 0.03); }
</style>
