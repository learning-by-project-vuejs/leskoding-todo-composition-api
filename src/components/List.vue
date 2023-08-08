<template>
    <ul class="list-group">
        <li
            v-for="(todo, index) in todos" :key="index"
            class="list-group-item d-flex justify-content-between"
        >
        <span v-if="todo.isDone">
            <del>{{ todo.activity }}</del>
        </span>
        <span v-else>
            {{ todo.activity }}
        </span> 
        <span>
            <span class="badge bg-primary mx-2 pointer" @click="emitDone(index)">Done</span>
            <span class="badge bg-danger pointer" @click="emitDestroy(index)">Delete</span>
        </span>
        </li>
    </ul>
</template>

<script>
export default {
    props: {
        todos: {
            type: Array
        }
    },
    setup(props, {emit}) {
        const emitDone = (index) => {
            emit('emitDone', index)
        }

        const emitDestroy = (index) => {
            emit('emitDestroy', index)
        }

        return { emitDone, emitDestroy }
    }
}
</script>

<style scoped>
.pointer {
    cursor: pointer;
}
</style>