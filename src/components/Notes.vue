<template>
  <div class="row">
    <div class="col-md-12 mb-2 d-inline-flex">
      <div class="col-md-8 col-sm-5">
        <b-button v-b-modal.note variant="primary">New Note</b-button>
      </div>
      <div class="col-md-4 col-sm-7 text-right">
        <label class="btn btn-light mt-2 mr-2" for="file-import">Import</label>
        <input type="file" id="file-import" class="inputFile" ref="file" accept=".note" v-on:change="importLocal()"/>
        <b-button variant="outline-light" @click="exportLocal">Export</b-button>
      </div>
    </div>
    <div class="col-md-12 text-light" v-if="items.length === 0">
        No notes is stored.
    </div>
    <div class="col-md-12 d-md-flex flex-md-wrap" v-if="items.length > 0">
      <div class="col-sm-12 col-md-6 col-lg-3 mb-2" v-for="(item, i) in items" :key="item.i">
        <div class="card" :onedit="item.isEditable?true:'no'">
          <div class="card-body">
            <h4 :id="'title'+i" class="card-title" :contenteditable="item.isEditable">{{ item.title }}</h4>
            <b-card-text :id="'text'+i" :contenteditable="item.isEditable">{{ item.text }}</b-card-text>
            <div class="text-right">
              <b-button size="sm" variant="outline-primary" v-on:click="edit(i)"><b-icon-pencil/></b-button>
              <b-button class="ml-1" size="sm" variant="outline-danger" v-on:click="remove(i)"><b-icon-trash/></b-button>
            </div>
        </div>
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
  </div>
</template>

<script>
// TODO: Add note pins to top
// TODO: Add draggable to arrange notes
import moment from "moment";
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
    edit: function (i) {
      if (!this.items[i].isEditable) {
        this.items[i].isEditable = true;
        return;
      }else{
        this.items[i].isEditable = false;
      }
      console.log(document.getElementById('title' + i));
      console.log(document.getElementById('title' + i).innerText);
      this.items[i].title = document.getElementById('title' + i).innerText;
      this.items[i].text = document.getElementById('text' + i).innerText;
      localStorage.items = JSON.stringify(this.items);
    },
    remove: function (i){
      this.items.splice(i, 1);
      localStorage.items = JSON.stringify(this.items);
    },
    importLocal: function (){
      const file = this.$refs.file.files[0];
      new Promise(function(resolve) {
        const reader = new FileReader();
        reader.onloadend = function() {
          resolve(reader.result)
        }
        reader.readAsText(file);
      }).then((r) => {
        localStorage.items = r;
        this.items = JSON.parse(r);
      });
    },
    exportLocal: function (){
      const blob = new Blob([localStorage.items], { type: 'application/pdf' });
      const link = document.createElement('a');
      link.href = URL.createObjectURL(blob);
      link.setAttribute('download', moment().format('Y-MM-D')+'-backup.note');
      link.click();
      URL.revokeObjectURL(link.href);
    },
  }
}
</script>

<style scoped>
  div[onedit="no"] {
    background-color: white;

    transition: background-color .5s ease-in-out;
  }

  div[onedit="true"] {
    background-color: rgba(46,46,46,1);
    color: white;
    box-shadow: 0 0 5px #434343;

    transition: background-color .5s ease-in-out;
  }

  .row {
    margin: 0;
  }

  .inputFile {
    width: 0;
    height: 0;
    opacity: 0;
    overflow: hidden;
    position: absolute;
    z-index: -1;
  }
</style>