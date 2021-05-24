<template>
    <AddTask v-show="showAddTask" @add-task="addTask" />
    <Tasks @toggle-reminder="toggleReminder" @delete-task="deleteTask" :tasks="tasks" />
</template>

<script>
import Tasks from '../components/Tasks'
import AddTask from '../components/AddTask'

export default {
    name: 'Home',
    props: {
        showAddTask: Boolean,
    },
    components: {
        Tasks,
        AddTask,
    },
    data() {
        return {
            db: "DB",
            tasks: [],
        }  
    },
    methods: {
        addTask(task) {
            this.tasks = [...this.tasks, task]
            this.saveTasks()
        },
        deleteTask(id) {
            this.tasks = this.tasks.filter(task => task.id !== id)
            this.saveTasks()
        },
        toggleReminder(id) {
            this.tasks = this.tasks.map(task => task.id === id ? {...task, reminder: !task.reminder} : task)
            this.saveTasks()
        },
        saveTasks() {
            localStorage.setItem(this.db, JSON.stringify(this.tasks))
        }
    },
    created() {
        const res = localStorage.getItem(this.db)
        const tasks = JSON.parse(res)
        if(tasks !== null) this.tasks = tasks
    }
}
</script>
