<template>
   <div class="container my-5">
        <div class="row justify-content-center">
            <div class="col-8">
                <router-link :to="{name: 'post.index'}" 
                    class="btn btn-primary shadow"
                >Back</router-link>
            </div>
            <div class="card rounded shadow">
                <div class="card-header">
                   Edit Transaction
                </div>
                <div class="card-body">
                    <form @submit.prevent="update()">
                      <div class="mb-3">
                        <label for="" class="form-label">Title</label>
                        <input type="text" class="form-contol" v-model="posts.title" > 
                        <div v-if="validation.title" class="text-danger">
                          {{validation.title [0]}}
                        </div>
                      </div>
                      <div class="mb-3">
                        <label for="" class="form-label">Amount</label>
                        <input type="number" class="form-contol"  v-model="posts.amount">
                        <div v-if="validation.amount" class="text-danger">
                          {{validation.amount [0]}}
                        </div>
                      </div>
                      <div class="mb-3">
                        <label for="" class="form-label">Time</label>
                        <input type="text" class="form-contol" placeholder="yyyy-mm-dd hh:mm:ss"  v-model="posts.time">
                         <div v-if="validation.time" class="text-danger">
                          {{validation.time [0]}}
                        </div>
                      </div>
                      <div class="mb-3">
                        <label for="" class="form-label">Type</label>
                        <select name="" id="" class="form-select"  v-model="posts.type">
                          <option value="expense">Expense</option>
                          <option value="revenue">Revenue</option>
                        </select>
                         <div v-if="validation.type" class="text-danger">
                          {{validation.type[0]}}
                        </div>
                      </div>
                      <button class="btn btn-outline-primary">Submit</button>
                    </form>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

import { reactive, ref,  onMounted} from 'vue'
import { useRouter, useRoute} from 'vue-router'
import axios from 'axios'
export default {
  setup(){
    let posts = reactive ({
      title: '',
      amount: '',
      time: '',
      type: '',
    })
    const validation = ref([]);
    const router = useRouter();
    const route = useRoute();

    onMounted(() => {
        axios.get(`http://localhost:8000/api/transaction/${route.params.id}`)

        .then((result) => {
                posts.title = result.data.data.title
                posts.amount = result.data.data.amount
                posts.time = result.data.data.time
                posts.type = result.data.data.type
            })
        .catch((err) => {
              console.log(err.response.data)
            })
        
    })

    function update() {
        axios.put(
        `http://localhost:8000/api/transaction/${route.params.id}`,
        posts
      )
         .then(() => {
               router.push({
                 name: 'post.index'
               })
            })
        .catch((err) => {
                validation.value = err.response.data
            })
    }
    return {
      posts,
      validation,
      router,
      update
    }

  }

}
</script>

<style>

</style>