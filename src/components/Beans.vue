<template>
  <div>
    <div class="card m-3 border-light">
      <h5 class="card-header text-center ">
        Bean Counter (Count anything you like - pushups completed, seconds breath held etc.)
        <button type="button" class="btn btn-outline-dark far fa-minus float-end" data-bs-toggle="collapse" data-bs-target=".multi-collapse"></button>
      </h5>
      <div class="collapse multi-collapse show card-body float-start">
          <div class="input-group mb-3 card-text container-sm">
              <span class="input-group-text">New Counter</span>
              <input type="text" name="counterLabel" id="counterLabel" class="form-control" v-model="counterLabel" autofocus :maxlength="counterLabelMax">
              <span class="input-group-text" v-text="counterLabelMax-counterLabel.length"></span>
              <button class="input-group-text far fa-plus btn btn-primary" @click="addNewCounter"></button>
          </div>
          <!--<button class="btn btn-success far fa-save mx-2" @click="storeToLocalStorage"> Save All</button>-->
          <button class="btn btn-danger far fa-times mx-2" @click="clear"> Clear All</button>
      </div>
      <div class="collapse multi-collapse show card-footer text-muted text-center">
        Built by Sartaj using VueJS. All data stored automatically at each interaction using localstorage in browser
      </div>
    </div>
    <div class="row row-cols-1 row-cols-md-5 m-1 g-3">
      <div class="col" v-for="(item, index) in items" :key="item.id">
        <div class="card border-dark text-primary">
          <h5 class="card-header bg-transparent">
            {{ item.text }}
            <button class="btn btn-danger far fa-times float-end" @click="removeCounter(index)"></button>
          </h5>
          <div class="card-body disable-dbl-tap-zoom">
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
      counterLabel: "",
      counterLabelMax:15,
      items: []
    }
  },
  methods: {
    add(index){
      this.items[index].count++
      this.storeToLocalStorage()
    },
    del(index){
      if(this.items[index].count>0){
        this.items[index].count--
        this.storeToLocalStorage()
      }
    },
    addNewCounter(){
      if(this.counterLabel != ""){
          this.items.push(
          {
            text: this.counterLabel,
            count: 0
          }
        )
      }
      this.counterLabel = ""
      this.storeToLocalStorage()
    },
    removeCounter(index){
      this.items.splice(index,1)
      this.storeToLocalStorage()
    },
    clear(){
      this.items = []
      this.storeToLocalStorage()
    },
    storeToLocalStorage(){
      localStorage.setItem("beanCounters", JSON.stringify(this.items))
    }
  },
  beforeMount(){
    if(!JSON.parse(localStorage.getItem("beanCounters"))){
      localStorage.setItem("beanCounters", JSON.stringify([]))
    }
    this.items = JSON.parse(localStorage.getItem("beanCounters"))
  }
}
</script>
