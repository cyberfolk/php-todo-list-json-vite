<script>
import { state } from "../../state.js";
import axios from "axios";
export default {
    name: "TasksDeleted",
    data() { return { state, }; },
    mounted() { state.fetchDeleted() },
    methods: {
        restoreTask(i) {
            const data = { index: i, start_path: "tasks/tasksDeleted.json" }
            const options = state.composeOptions('restore.php', 'POST', data)
            axios(options).catch(error => { console.error(error.message); })
            state.fetchTodo()
            state.fetchDeleted()
        },
        restoreAll() {
            const options = state.composeOptions('restoreAll.php', 'POST')
            axios(options).catch(error => { console.error(error.message); })
            state.fetchTodo()
            state.fetchDeleted()
        },
        recycleBin() {
            const options = state.composeOptions('recycleBin.php', 'POST')
            axios(options).catch(error => { console.error(error.message); })
            state.fetchTodo()
            state.fetchDeleted()
        }
    }
};
</script>

<template>
    <section id="tasks_deleted" class="col-4">
        <div class="d-flex align-items-end mb-2 justify-content-center">
            <h2 class="text-light text-center m-0 pe-2">Deleted Tasks</h2>
            <button @click="recycleBin(index)" class="bg-danger border-0 rounded py-1 px-2 me-1" title="Recycle Bin">
                <i class="fa-solid fa-recycle text-light"></i>
            </button>
            <button @click="restoreAll()" class="bg-warning border-0 rounded py-1 px-2 me-1" title="Restore All">
                <i class="fa-solid fa-trash-can-arrow-up text-light"></i>
            </button>
        </div>
        <!-- /#title_and_button -->

        <ul v-if="state.tasksDeleted.length" class="list-group">
            <li v-for="(task, index) in state.tasksDeleted" class="list-group-item d-flex fw-bold ps-2">
                <button @click="restoreTask(index)" class="bg-warning border-0 rounded py-1 px-2 me-1">
                    <i class="fa-solid fa-trash-can-arrow-up text-light"></i>
                </button>
                <span class="ms-2" role="button">{{ task.name }}</span>
            </li>
        </ul>

        <div class="alert alert-danger " role="alert" v-else>
            <strong>Al momento non ci sono task nel cestino</strong>
        </div>

    </section>
    <!-- /#tasks_deleted -->
</template>

<style lang="scss" scoped></style>
