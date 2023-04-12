<template>
  <v-app >
    <v-card class="mt-9">
      <v-card-title>
        <v-app-bar color="primary">
        <!-- Creating Frameowrks title-->
        <v-toolbar-title
        class="flex text-center" dense>
            <v-app-bar-nav-icon 
              size="32"
            ></v-app-bar-nav-icon>
          FRAMEWORKS
          {{ subtitle }}
        </v-toolbar-title>
        <!--Adding Button-->
        <AddButton @task-added="addTask" :taskList="this.tasks" addBtn/>
        
        
        
        
        
      </v-app-bar>
      </v-card-title>
      
      <v-container fluid>
        <Table :tasks="tasks"/>
      </v-container>
    </v-card>
  </v-app>
  
</template>

<script>

  import Table from '@/components/Table.vue'
  import AddButton from './components/AddButton.vue'

  export default{
    components: {
      Table,
      AddButton
    },
    methods: {
      addTask(newTask){
        this.tasks.push(newTask)    
      },
      deleteTask(){
        console.log("todo deleted")
        this.tasks = this.tasks.filter((task) => task.title !== title)
      }
    },
    data(){
      return{
        tasks: [],
        isTitleUnique: null,
      }
    }
  }
</script>
