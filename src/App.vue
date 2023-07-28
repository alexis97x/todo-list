<script setup>

    import {ref} from 'vue'
    import axios from 'axios'
    const newTask = ref('')
    const completedTasks = ref(0)
    const remainingTasks = ref(0)
    const tasks = ref([])
    let task_id = 1
    const quote = ref('')
    const author = ref('')
    const loading = ref(true)

    axios.get('https://api.api-ninjas.com/v1/quotes?category=success', {
        headers: {
            //YOUR API NINJA API KEY idk if its safe to paste my api key here?
            'X-Api-Key': 'd6/WVsvKqOAyv/a10rxNLw==Fjfg95hQ6NwryCOj'
        }
    })
    .then(response => {

        quote.value = response.data[0].quote
        author.value = response.data[0].author
        loading.value = false
    })

    const create = () => {
        tasks.value.push({
            task_id: task_id++,
            name: newTask.value,
            is_completed: false
        })
        newTask.value = ''
        remainingTasks.value = tasks.value.filter(task => task.is_completed === false).length
    }

    const completeTask = (task) => {
        task.is_completed = !task.is_completed
        completedTasks.value = tasks.value.filter(task => task.is_completed === true).length
        remainingTasks.value = tasks.value.filter(task => task.is_completed === false).length
    }


</script>


<template>
    <v-app>
        <v-main>
            <v-container class="text-center">
                <v-container class="w-100">
                    <v-container>
                        <h4>
                            <i>{{quote}}</i>
                        </h4>
                        <p class="text-end mr-4">
                            {{ '-'+author }}
                        </p>
                            <v-progress-circular
                            indeterminate
                            v-show="loading"
                            color="grey"
                            ></v-progress-circular>
                    </v-container>
                    <v-text-field
                        v-model="newTask"
                        @keydown.enter="create"
                        label="What are you working on?"
                    >
                        <template v-slot:append-inner>
                            <v-fade-transition>
                                <v-tooltip text="Creates a new task">
                                    <template v-slot:activator="{ props }">
                                        <v-btn
                                            icon="mdi-plus"
                                            size="x-small"
                                            v-bind="props"
                                            v-show="newTask"
                                            @click="create"
                                        ></v-btn>
                                    </template>
                                </v-tooltip>
                            </v-fade-transition>
                        </template>
                    </v-text-field>
                    <!-- <h1>Tasks: 
                        <v-fade-transition leave-absolute>
                            <span :key="`${tasks.length}`">
                                {{tasks.length}}
                            </span>
                        </v-fade-transition>
                    </h1> -->
                    <v-divider />
                    <v-row class="mt-3">
                        <v-col
                            cols="4"
                            class="my-2"
                        >
                            Remaining: 
                            <v-fade-transition leave-absolute>
                                <span :key="remainingTasks" class="text-red">
                                    {{ remainingTasks }}
                                </span>
                            </v-fade-transition>
                        </v-col>
                        <v-divider vertical class="my-2" />
                        <v-col
                            cols="4"
                            class="my-2"
                        >
                            Completed: 
                            <v-fade-transition leave-absolute>
                                <span :key="completedTasks" class="text-success">
                                    {{ completedTasks }}
                                </span>
                            </v-fade-transition>
                        </v-col>
                        <v-col
                            cols="4"
                        >
                            <v-progress-circular
                                v-show="tasks.length > 0"
                                class="me-3 mt-2"
                                color="success"
                                :model-value="(completedTasks / tasks.length) * 100 "
                            >
                            </v-progress-circular>
                        </v-col>
                    </v-row>
                    <v-divider class="mt-5" />

                    <v-card class="mt-4" v-show="tasks.length > 0">
                        <v-slide-y-transition group>
                            <div v-for="task in tasks" :key="task.task_id">
                                <v-hover v-slot="{isHovering, props}">
                                    <v-divider />
                                    <v-list-item :class="isHovering ? 'bg-grey-lighten-3' : ''" v-bind="props">
                                        <v-checkbox-btn
                                            @click="completeTask(task)"
                                            :label="task.name"
                                            :class="task.is_completed ? 'text-grey' : 'text-black'"
                                        >
                                        </v-checkbox-btn>
                                        <template v-slot:append>
                                            <v-fade-transition>
                                                <v-icon
                                                    color="success"
                                                    v-show="task.is_completed"
                                                >
                                                    mdi-check
                                                </v-icon>
                                            </v-fade-transition>
                                        </template>
                                    </v-list-item>
                                </v-hover>
                            </div>
                        </v-slide-y-transition>
                    </v-card>
                </v-container>
            </v-container>

        </v-main>
    </v-app>
</template>


