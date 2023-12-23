<script lang="ts">
export default {
    data() {
        return {
            /* Daten */
            task: "",
            description: "",
            tasks: [] as any
        }
    },
    methods: {
        createTask() {
            
            if (this.tasks !== '') {
                /* Maximal 10 Kommentare */
                if (this.tasks.length < 10) {
                    this.tasks.push({
                    task: this.task,
                    description: this.description
                });

                localStorage.setItem("todos", JSON.stringify(this.tasks));

                this.task = '';
                this.description = '';
                
                } else {
                    alert("Maximale Anzahl von Kommentaren erreicht (10).");
                }
            }
        },

        deleteTask() {
            this.tasks.splice(this.tasks.indexOf(this.tasks), 1)
            localStorage.setItem("todos", JSON.stringify(this.tasks));
        },

        deleteAllTask() {
            this.tasks = [];
            localStorage.setItem("todos", JSON.stringify(this.tasks));
            alert("Alle Aufgaben werden gelöscht!");
        }
    },
    mounted() {
        this.tasks = JSON.parse(localStorage.getItem("todos") || "");
    }
}
</script>

<template>
    <div class="container">
        <div class="row">
            <!--Create task form-->
            <div class="col-md-12">
                <form class="create-task" @submit.prevent="createTask">
                    <h4>Aufgabe erstellen</h4>
                    <div class="mb-3">
                        <input v-model="task" type="text" class="form-control" placeholder="Aufgabe" maxlength="25" required>
                    </div>
                    <div class="mb-3">
                        <textarea class="form-control" v-model="description" placeholder="Beschreibung" maxlength="100" rows="1" required></textarea>
                    </div>
                    <div class="mb-3">
                        <input type="submit" class="btn btn-success" value="Erstellen">
                    </div>
                </form>
            </div>
            <!--Delete all tasks-->
            <div class="col-md-12">
                <div class="task-delete-all">
                    <h4>Anzahl Aufgaben: <span class="badge rounded-pill text-bg-light">{{ tasks.length }}</span></h4>
                    <button class="btn btn-danger" @click="deleteAllTask()">Alle Aufgaben Löschen</button>
                </div>
            </div>
            <!--Overview of my tasks-->
            <div class="col-md-12">
                <div class="task-overview">
                    <h4>Deine Aufgaben</h4>
                    <div class="row" id="tasks" v-for="task in tasks">
                        <div class="col-md-3">
                            <h5>Aufgabe:</h5>
                            <p>{{ task.title }}</p>
                        </div>
                        <div class="col-md-6">
                            <h5>Beschreibung:</h5>
                            <p>{{ task.description }}</p>
                        </div>
                        <div class="col-md-3">
                            <div class="button-position">
                                <button class="btn btn-danger" @click="deleteTask()">Löschen</button>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>