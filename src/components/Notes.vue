<template>
  <div class="row">
    <div class="col-12 mb-3">
      <b-button v-b-modal.note variant="primary">New Note</b-button>
    </div>
    <p class="col-3" v-for="item in items" :key="item.index">
      <b-card :title="item.title">
        <b-card-text>{{ item.text }}</b-card-text>
      </b-card>
    </p>
    <b-modal id="note" title="Note" @ok="modalOK">
      <b-form-input id="input" class="mb-3" v-model="title" placeholder="Title"></b-form-input>
      <b-form-textarea id="textarea"
                       v-model="text"
                       rows="4"
                       max-rows="20"
                       placeholder="Some text.....">
      </b-form-textarea>
    </b-modal>
  </div>
</template>

<script>
export default {
  name: 'Notes',
  data() {
    return {
      items: [
          { title: '1', text: '2' }
      ]
    }
  },
  mounted() {
    if(localStorage.items){
      this.items = JSON.parse(localStorage.items);
    }
  },
  methods: {
    modalOK: function (){
      this.items.push({title: this.title, text: this.text})
      localStorage.items = JSON.stringify(this.items);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped></style>
