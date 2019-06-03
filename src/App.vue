<template lang="pug">
    .grid
        .grid__menu
            Menu(ref="Menu" @chosenMenu="chosenMenu")
        .grid__board
            h1.md-title(v-if="(id != null)") {{ title }}
            md-button.md-raised.md-primary(v-if="(id != null)" @click="createBoard()") Create todo
            md-button.md-accent(v-if="(id != null)" @click="deleteMenu()") Delete
            md-divider(v-if="(id != null)")
            draggable
                .container(v-for="board in boards")
                    Board(v-show="(board.id == id)" :board="board" 
                    @addTodo="addTodo" @deleteTodo="deleteTodo" @deleteboard="deleteboard")
</template>

<script>
import Menu from './components/Menu'
import Board from './components/Board'
import draggable from 'vuedraggable'

export default {
    name: 'app',
    data() {
        return {
            id: null,
            title: null,
            counter: 0,
            boards: []
        }
    },
    components: {
        Menu,
        Board,
        draggable
    },
    methods: {
        chosenMenu(index, title) {
            this.id = index
            this.title = title
        },
        createBoard() {
            this.boards.push({id: this.id, name: null, boardId: this.counter, todos: []})
            this.counter++
        },
        addTodo(boardId, task) {
            this.boards.forEach((board) => {
                if(boardId === board.boardId)
                    board.todos.push(task)
            })
        },
        deleteTodo(boardId, index) {
            this.boards.forEach((board) => {
                if(boardId === board.boardId)
                    board.todos.splice(index, 1)
            })
        },
        deleteMenu() {
            this.boards.forEach((board) => {
                if(board.id === this.id)
                    this.boards.splice(this.boards.indexOf(board), 1)
            })
            this.$refs.Menu.deleteItem(this.id);
            this.id = null
            this.title = null
        },
        deleteboard(boardId) {
            this.boards.forEach((board) => {
                if(board.boardId === boardId) {
                    this.boards.splice(this.boards.indexOf(board), 1)
                }
            })
        }
    }
}
</script>

<style lang="scss">
.grid {
    display: grid;
    grid-template-areas: "menu board";
    grid-template-columns: 2fr 8fr;
    height: 100vh;

    &__menu {
        grid-area: menu;
    }

    &__board {
        display: block;
        grid-area: board;
    }
}
.md-title {
    margin: 20px;
}
.container {
    display: inline-flex;
}
</style>
