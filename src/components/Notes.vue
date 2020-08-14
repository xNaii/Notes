<template>
  <div class="row">
    <div class="col-12 mb-3 ml-3">
      <b-button v-b-modal.note variant="primary">New Note</b-button>
    </div>
    <div class="col-12 text-light" v-if="items.length === 0">
        No notes is stored.
    </div>
    <div class="col-12 d-flex flex-md-wrap" v-if="items.length > 0">
      <div class="col-3" v-for="(item, i) in items" :key="item.i">
        <b-card :id="'title'+i" :contenteditable="item.isEditable" :title="item.title">
          <b-card-text :id="'text'+i">{{ item.text }}</b-card-text>
          <div class="text-right">
            <b-button size="sm" variant="outline-primary" v-on:click="edit(i, this)"><b-icon-pencil/></b-button>
            <b-button class="ml-1" size="sm" variant="outline-danger" v-on:click="remove(i)"><b-icon-trash/></b-button>
          </div>
        </b-card>
      </div>
    </div>
    <b-modal id="note" title="Note" @ok="modalOK">
      <b-form-input id="input" class="mb-3" v-model="title" placeholder="Title"></b-form-input>
      <b-form-textarea id="textarea"
                       v-model="text"
                       rows="4"
                       max-rows="20"
                       required
                       placeholder="Some text.....">
      </b-form-textarea>
    </b-modal>
  </div>
</template>

<script>
// TODO: Add note pins to top!
export default {
  name: 'Notes',
  data() {
    return {
      title: '',
      text: '',
      items: []
    }
  },
  mounted() {
    if(localStorage.items){
      this.items = JSON.parse(localStorage.items);
    }else{
      this.items = [
        { title: 'VueNotes', text: 'Here you can create your own notes', isEditable: false },
        { title: 'VueNotes', text: 'Simple Design \nEasy to use', isEditable: false },
        { title: 'VueNotes', text: 'Just 1 2 3 \nAnd some interesting thoughts... :)', isEditable: false },
        { title: 'VueNotes', text: 'Version 1.0.0', isEditable: false },
      ]
    }
  },
  methods: {
    modalOK: function (){
      this.items.push({ title: this.title, text: this.text, isEditable: false })
      localStorage.items = JSON.stringify(this.items);

      this.title = "";
      this.text = "";
    },
    edit: function (i, el) {
      console.log(el);

      if (!this.items[i].isEditable) {
        this.items[i].isEditable = true;
        return;
      }else{
        this.items[i].isEditable = false;
      }

      this.items[i].title = document.getElementById('title' + i).querySelector('.card-title').innerText;
      this.items[i].text = document.getElementById('text' + i).innerText;
      localStorage.items = JSON.stringify(this.items);
    },
    remove: function (i){
      this.items.splice(i, 1);
      localStorage.items = JSON.stringify(this.items);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .col-3 {
    margin-top: 0.5em;
  }

  div[contenteditable="false"] {
    background-color: white;
    transition: background-color .5s ease-in-out;
  }

  div[contenteditable="true"] {
    background-color: rgba(46,46,46,1);
    color: white;
    box-shadow: 0 0 5px #434343;

    transition: background-color .5s ease-in-out;
  }
</style>
