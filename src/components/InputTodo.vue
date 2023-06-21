<template>
    <b-row class="ml-0 mt-4">
        <div class="mb-4 form-inline">
            <b-form-input class="w-50" v-model="enteredTodo" placeholder="Enter Task"></b-form-input>
            <b-button class="ml-2" variant="primary" @click="addTodo">Add Todo</b-button>
        </div>
    </b-row>
</template>

<script>
export default {
    emits: ['new-todo'],
    data() {
        return {
            enteredTodo: ''
        }
    },
    methods: {
        addTodo() {
            if(this.enteredTodo === ''){
                alert('cannot empty')
            }else{
                this.$emit('new-todo', this.enteredTodo)
            }
            this.enteredTodo = ''
        }
    }
}


</script>