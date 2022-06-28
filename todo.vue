<template>
    <div class="container w-50 p-3 mt-5" style="background-color: #eee">
        <h2 class="text-center mt-5 text-dark">TO-Do List</h2>

        <!----Input----->
        <div class="d-flex mt-5">
            <input v-model="task" type="text" placeholder="Enter task" class="form-control">
            <button @click="submitTask" class="btn btn-warning rounded-0">Submit</button>
        </div>

        <!---Task table--->
        <table class="table table-bordered mt-5">
            <thead>
                <tr>
                <th scope="col" class="text-center">Task</th>
                <th scope="col" class="text-center">Status</th>
                <th scope="col" class="text-center">Edit</th>
                <th scope="col" class="text-center">Delete</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="(task,index) in tasks" :key="index">
                    <td>
                        <span :class="{'finished': task.status === 'finished'}">
                            {{task.name}}
                        </span>
                    </td>

                    <td class="text-center" style="width: 120px">
                        <span @click="changeStatus(index)" class="pointer"
                        :class="{  'text-success': task.status === 'to-do',
                                    //'text-warning': task.status === 'in-progress',
                                    'text-danger': task.status === 'finished',
                        }"
                        >
                            {{firstCharUpper(task.status)}}
                        </span>
                    </td>

                    <td>
                        <div class="text-center" @click="editTask(index)">
                            <span class="fa fa-pen"></span>
                        </div>
                    </td>
                    <td>
                        <div class="text-center" @click="deleteTask(index)">
                            <span class="fa fa-trash"></span>
                        </div>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    export default{
        name: 'HelloWorls',
        props:{
            msg: String
        },

        data(){
            return{
                task:'',
                editedTask : null,
                availableStatuses: ['to-do','finished'],

                tasks:[
                    {
                        name: 'Class At 8 a.m',
                        status: 'to-do'
                    },
                    {
                        name: 'Work At 5 p.m',
                        status: 'to-do'
                    },
                ]
            }
        },

        methods:{
            submitTask(){
                if(this.task.length === 0) return;

                if(this.editedTask === null){
                    this.tasks.push({
                        name: this.task,
                        status: 'to-do',
                    });
                }
                else{
                        this.tasks[this.editedTask].name = this.task;
                        this.editedTask = null;
                }
                
                
                this.task = '';
            },

            deleteTask(index){
                this.tasks.splice(index,1);
            },

            editTask(index){
                this.task = this.tasks[index].name;
                this.editedTask =  index;
            },

            changeStatus(index){
                let newIndex = this.availableStatuses.indexOf(this.tasks[index].status);
                if(++newIndex > 2) newIndex = 0;
                this.tasks[index].status = this.availableStatuses[newIndex];
            },

            firstCharUpper(str){
                return str.charAt(0).toUpperCase() + str.slice(1);
            }
        }
    };
</script>

<style scoped>
    .pointer{
        cursor: pointer;
    }

    .finished{
        text-decoration: line-through;
    }
</style>