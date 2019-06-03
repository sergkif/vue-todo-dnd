<template lang="pug">
    md-list.list.md-scrollbar
        md-dialog-prompt(
            :md-active.sync="active"
            v-model="value"
            md-title="New board"
            md-input-maxlength="30"
            md-input-placeholder="Name of new board"
            md-confirm-text="Done"
            @md-confirm="addNewBoard()"
            @md-cancel="cancel()")
        md-list-item.item(@click="active = true") New Board
        md-list-item(v-for="(val, index) in values" v-bind:key="index" @click="$emit('chosenMenu', index, val)" @remove="removeFromMenu()") {{ val }}
</template>

<script>
export default {
    data() {
        return {
            active: false,
            values: [],
            value: null
        }
    },
    methods: {
        addNewBoard() {
            if(this.value != '' && this.value != null) {
                if(this.values.includes(this.value)) {
                    alert(`You have already used ${this.value}`)
                    this.value = ''
                } else {
                    this.values.push(this.value)
                    this.value = ''
                }
            }
        },
        cancel() {
            this.value = null
        },
        deleteItem(id) {
            this.values.splice(id, 1)
        }
    }
}
</script>

<style lang="scss" scoped>
    .list {
        height: 100%;
        width: 100%;
        overflow: scroll;
    }
</style>