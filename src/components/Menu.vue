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
        md-list-item(v-for="value in values" :key="value.id" @click="$emit('chosenMenu', value.id, value.data)") {{ value.data }}
</template>

<script>
export default {
    data() {
        return {
            active: false,
            values: [],
            id: 0,
            value: null
        }
    },
    methods: {
        addNewBoard() {
            if(this.value != '' && this.value != null) {
                if(this.values.includes(this.value.data)) {
                    alert(`You have already used ${this.value}`)
                    this.value = ''
                } else {
                    this.values.push({id: this.id, data: this.value})
                    this.id++
                    this.value = ''
                }
            }
        },
        cancel() {
            this.value = null
        },
        deleteItem(menuId) {
            this.values.forEach((value) => {
                if(value.id === menuId)
                    this.values.splice(this.values.indexOf(value), 1)
            })
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