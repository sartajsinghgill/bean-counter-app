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
    <ul class="nav nav-tabs" id="myTab" role="tablist">
      <li class="nav-item" role="presentation">
        <a class="nav-link active" id="home-tab" data-bs-toggle="tab" href="#home" role="tab" aria-controls="home" aria-selected="true">Current Counters <span class="badge bg-success">{{ items.filter(item => !item.archived).length }}</span></a>
      </li>
      <li class="nav-item" role="presentation">
        <a class="nav-link" id="profile-tab" data-bs-toggle="tab" href="#profile" role="tab" aria-controls="profile" aria-selected="false">Archived Counters <span class="badge bg-danger">{{ items.filter(item => item.archived).length }}</span></a>
      </li>
    </ul>
    <div class="content-tab" id="myTabContent">
      <div class="tab-pane fade show active row row-cols-md-5 m-1 g-3" id="home" role="tabpanel" aria-labelledby="home-tab">
        <div v-for="(item, index) in listOfCards" :key="item.id">
          <div class="col">
            <div class="card border-dark text-primary">
              <h5 class="card-header bg-transparent">
                {{ item.text }}
                <span class="float-end">
                  <button class="btn btn-warning far fa-inbox-out mx-2 text-white" @click="archiveCard(index)" title="Archive"></button>
                  <button class="btn btn-danger far fa-times" @click="removeCounter(index)" title="Delete"></button>
                </span>
              </h5>
              <div class="card-body disable-dbl-tap-zoom">
                <span class="m-2">{{ item.count }}</span>
                <span class="float-end">
                  <button class="btn btn-outline-primary far fa-plus" @click="add(index)"></button>
                  <button class="btn btn-outline-danger mx-2 far fa-minus" @click="del(index)"></button>
                </span>
                <div>
                  <progress v-bind:value="item.progressVal" max="100" v-if="item.progressVal != ''"></progress>
                </div>
              </div>
              <div class="card-footer bg-transparent">
                <div class="input-group">
                  <div class="input-group-text">
                    <input type="checkbox" class="form-check-input" @change="timerStart(index)" v-model="item.timerChecked">
                  </div>
                  <input type="number" class="form-control" placeholder="Auto-increment(secs)" v-model="item.timerSeconds" min="0">
                </div>
                <div class="input-group my-2">
                  <div class="input-group-text">
                    <label for="startDate">Start Date</label>
                  </div>
                  <input type="date" class="form-control" name="startDate" id="startDate" placeholder="Start Date" v-bind:value="item.startDate" disabled required>
                </div>
                <!--
                <div class="input-group">
                  <div class="input-group-text">
                    <label for="endDate">End Date&nbsp;</label>
                  </div>
                  <input type="date" class="form-control" name="endDate" id="endDate">
                </div>-->
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="tab-pane fade row row-cols-md-5 m-1 g-3" id="profile" role="tabpanel" aria-labelledby="profile-tab">
        <div v-for="(item, index) in listOfArchivedCards" :key="item.id">
          <div class="col">
            <div class="card border-dark text-primary">
              <h5 class="card-header bg-transparent">
                {{ item.text }}
                <span class="float-end">
                  <button class="btn btn-warning far fa-inbox-in mx-2 text-white" @click="unArchiveCard(index)" title="Un-Archive"></button>
                  <button class="btn btn-danger far fa-times" @click="removeCounter(index)" title="Delete"></button>
                </span>
              </h5>
              <div class="card-body disable-dbl-tap-zoom">
                <span class="m-2">{{ item.count }}</span>
                <span class="float-end">
                  <button class="btn btn-outline-primary far fa-plus" @click="add(index)"></button>
                  <button class="btn btn-outline-danger mx-2 far fa-minus" @click="del(index)"></button>
                </span>
                <div>
                  <progress v-bind:value="item.progressVal" max="100" v-if="item.progressVal != ''"></progress>
                </div>
              </div>
              <div class="card-footer bg-transparent">
                <div class="input-group">
                  <div class="input-group-text">
                    <input type="checkbox" class="form-check-input" @change="timerStart(index)" v-model="item.timerChecked">
                  </div>
                  <input type="number" class="form-control" placeholder="Auto-increment(secs)" v-model="item.timerSeconds" min="0">
                </div>
                <div class="input-group my-2">
                  <div class="input-group-text">
                    <label for="startDate">Start Date</label>
                  </div>
                  <input type="date" class="form-control" name="startDate" id="startDate" placeholder="Start Date" v-bind:value="item.startDate" disabled required>
                </div>
                <!--
                <div class="input-group">
                  <div class="input-group-text">
                    <label for="endDate">End Date&nbsp;</label>
                  </div>
                  <input type="date" class="form-control" name="endDate" id="endDate">
                </div>-->
              </div>
            </div>
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
            count: 0,
            timerSeconds: "",
            timerChecked: false,
            progressVal: "",
            startDate: formatDate(),
            endDate: "",
            archived: false,
            intervalFunc: function(){},
            progressFunc: function(){}
          }
        )
      }
      this.counterLabel = ""
      this.storeToLocalStorage()
    },
    removeCounter(index){
      clearInterval(this.items[index].intervalFunc)
      clearInterval(this.items[index].progressFunc)
      this.items.splice(index,1)
      this.storeToLocalStorage()
    },
    archiveCard(index){
      this.items[index].archived = true
      this.items[index].timerChecked = false
      this.items[index].timerSeconds = ""
      clearInterval(this.items[index].intervalFunc)
      this.items[index].progressVal = ""
      clearInterval(this.items[index].progressFunc)
      this.storeToLocalStorage()
    },
    unArchiveCard(index){
      this.items[index].archived = false
      this.storeToLocalStorage()
    },
    clear(){
      this.items = []
      this.storeToLocalStorage()
    },
    timerStart(index){
      if(this.items[index].timerChecked){
        if(this.items[index].timerSeconds == ""){
          this.items[index].timerSeconds = 1
        }
        if(this.items[index].progressVal == ""){
          this.items[index].progressVal = 1
        }
        let progressTimeoutVal = this.items[index].timerSeconds*15 
        this.items[index].progressFunc = setInterval(function() {
          if(this.items[index].progressVal != 100){
            this.items[index].progressVal++
          }
          else{
            this.items[index].progressVal = 0
          }
        }.bind(this), progressTimeoutVal)
        let intervalTimeoutVal = this.items[index].timerSeconds*1000
        this.items[index].intervalFunc = setInterval(function() {
          this.add(index)
        }.bind(this), intervalTimeoutVal)
      }
      else{
        this.items[index].timerSeconds = ""
        clearInterval(this.items[index].intervalFunc)
        this.items[index].progressVal = ""
        clearInterval(this.items[index].progressFunc)
      }
    },
    storeToLocalStorage(){
      localStorage.setItem("beanCounters", JSON.stringify(this.items))
    }
  },
  computed: {
    listOfCards: function() {
      return this.items.filter(function (item){
        if(!item.archived){
          return item
        }
        else{
          return null
        }
      })
    },
    listOfArchivedCards: function() {
      return this.items.filter(function (item){
        if(item.archived){
          return item
        }
        else{
          return null
        }
      })
    }
  },
  beforeMount(){
    if(!JSON.parse(localStorage.getItem("beanCounters"))){
      localStorage.setItem("beanCounters", JSON.stringify([]))
    }
    this.items = JSON.parse(localStorage.getItem("beanCounters"))
  }
}

function formatDate(){
  var d = new Date()
  return d.getFullYear()+'-'+d.getMonth()+1+'-'+d.getDate()
}
// https://github.com/sartajsinghgill/bean-counter-app.git
</script>
