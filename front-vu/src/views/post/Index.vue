<template>
    <div class="container my-5">
        <div class="row justify-content-center">
            <div class="col-8">
                <router-link :to="{name: 'post.create'}" 
                    class="btn btn-primary shadow"
                >Add</router-link>
            </div>
            <div class="card rounded shadow">
                <div class="card-header">
                    Post List
                </div>
                <div class="card-body">
                    <table class="table">
                        <thead>
                            <tr>
                                <th>Title</th>
                                <th>Amount</th>
                                <th>Type</th>
                                <th>Action</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(transaction, index) in posts.data" :key="index">
                                <td>{{transaction.title}}</td>
                                <td>{{transaction.amount}}</td>
                                <td>{{transaction.type}}</td>
                                <td>
                                    <div class="btn-group">
                                        <router-link :to="{name: 'post.edit', params:{id: transaction.id}}"
                                        class="btn btn-outline-info">Edit</router-link>
                                        <button  class="btn btn-sm btn-danger ml-2"
                                        @click.prevent="destroy(transaction.id, index)"
                                        >Delete</button>
                                    </div>  
                                </td> 
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'
import {onMounted, ref} from 'vue'

export default {
    setup() {
        let posts = ref([]);

        onMounted(() => {
            // get data from api
            axios.get('http://localhost:8000/api/transaction')
            .then((result) => {
                posts.value= result.data
            })
            .catch((err) => {
                console.log(err.response)

            })
        })

        function destroy(id, index ){
              axios.delete(
        `http://localhost:8000/api/transaction/${id}`,
    
      )
         .then(() => {
            posts.value.data.splice(index, 1)
        })
        .catch((err) => {
                console.log(err.response.data)
            })
        }
        return {
            posts,
            destroy
        }
    }
}
</script>

<style>

</style>