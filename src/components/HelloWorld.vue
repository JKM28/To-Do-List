<template>
  <v-container fluid class="background-image pa-4">
    <v-row>
      <v-col cols="12">
        <h1 class="display-1 text-center white-text mb-4">To-Do List</h1>
      </v-col>

      <!-- Add Task Input -->
      <v-col cols="12" md="8" class="mx-auto">
        <v-text-field
          v-model="newTask"
          label="Add a new task"
          outlined
          clearable
          @keyup.enter="addTask"
          class="unique-text-field"
        />
      </v-col>

      <!-- Add Task Button -->
      <v-col cols="12" class="text-center mt-4">
        <v-btn
          @click="addTask"
          :disabled="!newTask.trim()"
          class="custom-button"
        >
          <v-icon left>mdi-plus-circle-outline</v-icon>
          Add Task
        </v-btn>
      </v-col>

      <!-- Task List -->
      <v-col cols="12" md="8" class="mx-auto">
        <v-list>
          <v-list-item-group v-if="tasks.length">
            <v-list-item
              v-for="(task, index) in tasks"
              :key="index"
              class="d-flex align-center mb-2 task-list-item"
            >
              <!-- Task Text -->
              <v-list-item-content>
                <v-list-item-title :class="{'text-decoration-line-through': task.completed, 'task-text': true}">
                  {{ task.text }}
                </v-list-item-title>
              </v-list-item-content>

              <!-- Complete/Remove Task Buttons -->
              <v-list-item-action>
                <v-btn
                  icon
                  @click="toggleComplete(index)"
                >
                  <v-icon v-if="!task.completed">mdi-check-circle-outline</v-icon>
                  <v-icon v-else color="green">mdi-check-circle</v-icon>
                </v-btn>

                <v-btn
                  icon
                  color="red"
                  @click="removeTask(index)"
                >
                  <v-icon>mdi-delete</v-icon>
                </v-btn>
              </v-list-item-action>
            </v-list-item>
          </v-list-item-group>
          <v-list-item v-else>
            <v-list-item-content>
              <v-list-item-title class="text-center task-text">
                No tasks available
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: JSON.parse(localStorage.getItem('tasks')) || [],
    };
  },
  methods: {
    addTask() {
      if (this.newTask.trim()) {
        this.tasks.push({ text: this.newTask, completed: false });
        this.newTask = '';
        this.saveTasks();
      }
    },
    toggleComplete(index) {
      this.tasks[index].completed = !this.tasks[index].completed;
      this.saveTasks();
    },
    removeTask(index) {
      this.tasks.splice(index, 1);
      this.saveTasks();
    },
    saveTasks() {
      localStorage.setItem('tasks', JSON.stringify(this.tasks));
    },
  },
};
</script>

<style>
.background-image {
  background-image: url('@/assets/csubg.jpg'); /* Path to your image */
  background-size: cover; /* Adjusts the size of the background image */
  background-position: center; /* Centers the image */
  background-repeat: no-repeat; /* Prevents image repetition */
  min-height: 100vh; /* Ensures the background covers the entire viewport height */
}

.text-decoration-line-through {
  text-decoration: line-through;
}

.white-text {
  color: white; /* Sets text color to white */
}

.task-text {
  color: black; /* Sets task list text color to black */
}

/* Unique styles for v-text-field */
.unique-text-field .v-input__control {
  background: rgba(255, 255, 255, 0.2); /* Light transparent background */
  border-radius: 12px; /* Rounded corners */
  border: 2px solid #42a5f5; /* Blue border */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3); /* Box shadow */
  transition: all 0.3s ease; /* Smooth transition */
}

.unique-text-field .v-input__control .v-input__slot {
  padding: 8px 16px; /* Adjust padding */
}

.unique-text-field .v-label {
  color: rgba(255, 255, 255, 0.8) !important; /* Light label color */
  font-weight: bold; /* Bold label */
}

.unique-text-field .v-text-field__details {
  color: rgba(255, 255, 255, 0.8) !important; /* Lighter hint color */
}

.unique-text-field .v-text-field__control:hover,
.unique-text-field .v-text-field__control:focus-within {
  border-color: #64b5f6 !important; /* Change border color on focus */
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3); /* Enhanced box shadow on focus */
}

/* Custom button styles */
.custom-button {
  background: linear-gradient(45deg, #8aba87, #86a085) !important; /* Gradient background */
  color: white !important; /* Text color */
  border-radius: 50px; /* Fully rounded corners */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.3); /* Soft shadow */
  padding: 12px 24px; /* Adjust padding for better size */
  text-transform: uppercase; /* Uppercase text */
  font-weight: bold; /* Bold text */  
  font-size: 16px; /* Font size */
  transition: background 0.3s ease, box-shadow 0.3s ease; /* Smooth transition */
}

.custom-button:hover {
  background: linear-gradient(45deg, #2fc61b, hwb(94 5% 20%)) !important; /* Darker gradient on hover */
  box-shadow: 0 6px 12px rgba(0, 0, 0, 0.4); /* Enhanced shadow on hover */
}

.custom-button:disabled {
  background: rgba(255, 255, 255, 0.2) !important; /* Lightened background for disabled state */
  color: rgba(255, 255, 255, 0.6) !important; /* Lightened text color */
  box-shadow: none; /* Remove shadow for disabled state */
}
</style>
