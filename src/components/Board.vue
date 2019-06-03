<template lang="pug">
    md-content
        md-field
            label Name of Todo
            .md-title(v-if="board.name") {{ board.name }}
            md-input(v-else v-model="boardName" @keyup.enter="addBoardName()")
        draggable(v-model="board.todos" group="todos")
            transition-group.container
                md-card.md-primary.card(v-for="(todo, index) in board.todos" :key="index") 
                    md-card-header
                        md-card-header-text
                            .md-subheading {{todo}}
                        md-button.md-accent(@click="$emit('deleteTodo', board.boardId, index)") Delete
        md-field
            label Add new task
            md-input(v-model="task" @keyup.enter="addTodo(board)")
        md-button.md-accent.delContainer(@click="$emit('deleteboard', board.boardId)") Delete
</template>

<script>
import draggable from 'vuedraggable'

export default {
    props: ['board'],
    data() {
        return {
            task: null,
            boardName: null
        } 
    },
    components: {
        draggable
    },
    methods: {
        addTodo(boardId) {
            this.$emit('addTodo', this.board.boardId, this.task)
            this.task = null
        },
        addBoardName() {
            this.board.name = this.boardName
        }
    }
}
</script>

<style lang="scss" scoped>
.md-content {
    width: 300px;
    min-height: 200px;
    padding: 10px;
    margin: 20px;
    display: flexbox;
    border: 1px solid;  
}
.md-card-header-text {
    max-width: 160px;
    overflow-wrap: break-word;
}
.container {
    display: block;
    min-height: 20px;
}
.delContainer {
    float: right;
}
</style>
