<template>
  <div>
    <div class="card m-3 text-center border-light">
      <h5 class="card-header">
        Bean Counter (Count anything you like - pushups completed, seconds breath held etc.)
      </h5>
      <div class="card-body">
          <div class="input-group mb-3 card-text">
            <div class="input-group mb-3">
              <span class="input-group-text">New Counter</span>
              <input type="text" name="addCounter" id="addCounter" class="form-control" v-model="addCounter">
              <button class="input-group-text far fa-plus" @click="addNewCounter"></button>
            </div>
          </div>
      </div>
      <div class="card-footer text-muted">
        Built by Sartaj using VueJS
      </div>
    </div>
    <div class="row row-cols-1 row-cols-md-6 m-1">
      <div class="col" v-for="(item, index) in items" :key="item.id">
        <div class="card border-dark text-primary">
          <h5 class="card-header bg-transparent">
            {{ item.text }}
            <button class="btn btn-danger far fa-times" @click="removeCounter(index)"></button>
          </h5>
          <div class="card-body">
            {{ item.count }}
            <br />
            <button class="btn btn-outline-primary my-2 far fa-plus" @click="add(index)"></button>
            <button class="btn btn-outline-danger mx-2 far fa-minus" @click="del(index)"></button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
 
<script>
export default {
  name: 'Exercise',
  data() {
    return {
      addCounter: "",
      items: []
    }
  },
  methods: {
    add(index){
      this.items[index].count++
    },
    del(index){
      if(this.items[index].count>0){
        this.items[index].count--
      }
    },
    addNewCounter(){
      if(this.addCounter != ""){
          this.items.push(
          {
            text: this.addCounter,
            count: 0
          }
        )
      }
      this.addCounter = ""
      this.storeToLocalStorage()
    },
    removeCounter(index){
      this.items.splice(index,1)
      this.storeToLocalStorage()
    },
    storeToLocalStorage(){
      localStorage.setItem("beanCounters", JSON.stringify(this.items))
    }
  },
  beforeMount(){
    this.items = JSON.parse(localStorage.getItem("beanCounters"))
  }
}
</script>
