<template>
  <div>
    <div class="card m-3" style="width:20rem">
        <div class="card-body">
            <h5 class="card-title">
                Sartaj's Daily exercise routine
            </h5>
        </div>
    </div>
    <div class="container-fluid" id="exercise">
        <ul class="list-group">
            <li class="list-group-item">
                <h4>Pushups: {{ pushups }}</h4>
                <button class="btn btn-primary" @click="addPushups"><i class="far fa-plus-circle"></i> Add</button>
            </li>
            <li class="list-group-item">
                <h4>Situps: {{ situps }}</h4>
                <button class="btn btn-primary" @click="addSitups"><i class="far fa-plus-circle"></i> Add</button>
            </li>
        </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Exercise',
  props: {
    msg: String
  },
  data() {
    return {
      pushups: 0,
      situps: 0
    }
  },
  methods: {
    addPushups(){
      this.pushups++
    },
    addSitups(){
      this.situps++
    }
  }
}
</script>
