<template>
 
  <v-table class="mt-5" style="margin: 30px">
    <template v-slot:default>
      <thead>
        <tr>
          <th style="width:calc(100% / 6);" class="text-center">Title</th>
          <th style="width:calc(100% / 6);" class="text-center">Description</th>
          <th style="width:calc(100% / 6);" class="text-center">Deadline</th>
          <th style="width:calc(100% / 6);" class="text-center">Priority</th>
          <th style="width:calc(100% / 6);" class="text-center">Is Complete</th>
          <th style="width:calc(100% / 6);" class="text-center">Action</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="task in tasks" :key="task.title" style="height: 100px">
          <td class="text-center">{{ task.title }}</td>
          <td class="text-center">{{ task.description }}</td>
          <td class="text-center">{{ formDate(task.deadline) }}</td>
          <td class="text-center">{{ task.priority }}</td>
          <td>
            <v-row class="d-flex" justify="center">
              <div>
                <v-checkbox class="d-flex" justify="center" v-model="task.checkbox"></v-checkbox>
              </div>
            </v-row>
          </td>
          
          <td >
            <v-row class="d-flex" justify="center" v-if="!task.checkbox">
              <AddButton @task-edited="updateTask" :currentTask="task" :editTitle="task.title" />
            </v-row>
            <v-row class="d-flex" justify="center">
              <DeleteButton @task-deleted="deleteTask" :delTitle="task.title"/>
            </v-row>
            
          </td>
        </tr>
        
      </tbody>
    </template>
  </v-table>

  <v-snackbar v-model="showToastr" :timeout="timeout" color="green" location="bottom right" style="z-index: 9999;">
        Task was deleted successfully!
    </v-snackbar>

</template>
  
<script>
import AddButton from '@/components/AddButton.vue'
import DeleteButton from '@/components/DeleteButton.vue'
import moment from "moment";

  export default {
    props: {
      tasks: Array
    },
    components: {
      AddButton,
      DeleteButton
    },
    methods: {
      // method to update the task in the table
      updateTask(updateTask) {
        for (let i = 0; i < this.tasks.length; i++) {
          if (this.tasks[i].title == updateTask.title) {
            this.tasks[i] = {
              title: this.tasks[i].title,
              description: updateTask.description,
              deadline: updateTask.deadline,
              priority: updateTask.priority,

            };
          }
        }
        console.log(moment(updateTask).format('MM/DD/YYYY'))
    },
    formDate(date){
      if(!(date == "")){
        return moment(date).format('MM/DD/YYYY')
      } else{
        return ""
      }
    },
    deleteTask(title){
      for (let i = 0; i < this.tasks.length; i++) {
          if (this.tasks[i].title == title) {
            this.tasks.splice(i, 1)
            
          }
        }
        this.showToastr = true
    }
  },
  data(){
    return{
      checkbox: false,
      showToastr: false,
      timeout: 3000,

    }
  }
}
</script>