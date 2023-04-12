<template>
    <v-dialog v-model="dialogVisible"
    max-width="25%">
        <template v-slot:activator="{ props }">
            <v-btn
            style="background-color: #2064c4; color: white"
            elevation="4"
            @click="onClick"
            
            >
                <v-icon>{{addBtn ? 'mdi-plus-circle' : 'mdi-pencil-box-outline'}}</v-icon>
                {{ this.btnTitle }}
            </v-btn>
        </template>
      
        <v-card>
            <v-card-title style="background-color: #2064c4; color: white">
                <v-icon>{{this.addBtn ? 'mdi-plus-circle' : 'mdi-pencil-box-outline'}}</v-icon> {{ this.cardTitle }}
            </v-card-title>
                <v-container>

                    <!--Dialog Title Field-->
                    <v-row v-if="addBtn">
                        <v-col>
                            <v-text-field
                            label="Title"
                            v-model="title"
                            :rules="[rules.titleReq]"
                            :error-messages="this.titleError"
                            >
                            </v-text-field>
                        </v-col>
                    </v-row>

                    <!--Dialog Description Field-->
                    <v-row>
                        <v-col>
                            <v-text-field
                            label="Description"
                            v-model="description"
                            :rules="[rules.descReq]"
                            :error-messages="this.descError"
                            v-if="addBtn || currentTask"
                            >
                            </v-text-field>
                        </v-col>
                    </v-row>

                    
                    <!--Dialog Deadline Field-->
                    <v-row>
                        <v-col>
                            <v-text-field
                            type="date"
                            label="Deadline"
                            v-model="deadline"
                            v-if="addBtn || currentTask"
                            ></v-text-field>
                            
                        </v-col>
                    </v-row>

                    <!--Dialog Priority Section-->
                    <v-row>
                        <v-col>
                            <v-radio-group inline label="Priority" v-model="priority" v-if="addBtn || currentTask">
                                <v-radio label="Low" value="Low"></v-radio>
                                <v-radio label="Medium" value="Medium"></v-radio>
                                <v-radio label="High" value="High"></v-radio>
                            </v-radio-group>
                            
                        </v-col>
                    </v-row>

                </v-container>
                <v-card-actions>
                    <v-spacer></v-spacer>
                    
                    <!--Dialog submit button-->
                    <v-btn
                    prepend-icon="mdi-plus-circle"
                    color="primary"
                    variant="flat"
                    @click="onSubmit"
                    :disabled="submitDisabled"
                    >
                        {{this.submitTitle}}
                    </v-btn>
                    
                    <!--Dialog cancel button-->
                    <v-btn
                    prepend-icon="mdi-cancel"
                    color="error"
                    variant="flat"
                    @click="cancel"
                    >
                        Cancel
                    </v-btn>
            </v-card-actions>
        </v-card>

    </v-dialog>

    <v-snackbar v-model="showToastr" :timeout="timeout" color="green" location="bottom right">
        {{ toastrText }}
    </v-snackbar>
</template>
  
<script>
import moment from "moment";

export default {
    name: 'AddButton',
    props: {
       addBtn: Boolean,
        add: Function,
        editTitle: String,
        currentTask: Object,
        isTitleUnique: Boolean,
        taskList: Array
        
    },
    computed: {
        submitDisabled() {
            if(this.addBtn){
                return !this.title || !this.description || !this.deadline || !this.priority;
            } else{
                return !this.description || !this.deadline || !this.priority;
            }
            
        }
    },
    methods: {
        onClick() {
            if(this.addBtn){
                this.dialogVisible = true;
                
            } else{
                this.description = this.currentTask.description
                this.deadline = this.currentTask.deadline
                this.priority = this.currentTask.priority
                this.dialogVisible = true;
            }
            
        
        },
        onSubmit(e){
            e.preventDefault()
            
            if(!this.addBtn){
                this.dialogVisible = false
                console.log(this.currentTask)
                const updatedTask = {
                    title: this.editTitle,
                    description: this.description,
                    deadline:  this.deadline,
                    priority: this.priority,
                };
                this.$emit("task-edited", updatedTask)
                this.description = ''
                this.deadline = null
                this.priority = ''
                this.descError = []

                this.toastrText = 'Task was updated successfully!'
                this.showToastr = true
            } else {
                this.isTitleUnique = !this.taskList.some(task => task.title === this.title);
                if (!this.isTitleUnique) {
                    this.titleError.push("Title must be unique")
                    return;
                }

                this.dialogVisible = false
                const newTask = {
                    title: this.title,
                    description: this.description,
                    deadline: this.deadline,
                    priority: this.priority
                }
                this.$emit("task-added", newTask)
                this.title = ''
                this.description = ''
                this.deadline = null
                this.priority = ''
                this.titleError = []

                this.toastrText = 'Task was added successfully!'
                this.showToastr = true
            }

            
            
            
           
        
    },
    cancel(){
        this.dialogVisible = false
        this.title = ''
        this.description = ''
        this.deadline = ''
        this.priority = ''
        this.titleError = []
        this.descError = []
        
    }
},
    data(){
        return{
            dialogVisible: false,
            cardTitle: "",
            submitTitle: "",
            btnTitle: "",
            title: "",
            description: "",
            priority: null,
            deadline: null,
            isTitleUnique: true,
            descError: [],
            titleError: [],
            rules: {
                titleReq: value => !!value || 'Title is required',
                descReq: value => !!value || 'Description is required',
            },
            currentTaskk: {
                title: '',
                description: '',
                priority: null,
                deadline: null,
            },
            showToastr: false,
            timeout: 3500,
            toastrText: ""

        };
    },
    created(){
            if(this.addBtn){
                this.cardTitle = "Add Task"
                this.submitTitle = "Add",
                this.btnTitle = "Add"
            } 

            if(!this.addBtn){
                this.cardTitle = "Edit Task"
                this.submitTitle = "Edit"
                this.btnTitle = "Update"

              
            }
        }

}

</script>


  