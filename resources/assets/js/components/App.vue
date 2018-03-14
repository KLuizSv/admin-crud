<template>
    <div id="app">
        <table-component
                v-for="crud in cruds"
                v-bind="crud"
                :key="crud.id"
                @update="update"
                @delete="del"
        ></table-component>
        <div>
            <button @click="create()">Add</button>
        </div>
    </div>
</template>

<script>
  function Crud({ id, color, name}) {
    this.id = id;
    this.color = color;
    this.name = name;
  }
  import TableComponent from './TableComponent.vue';
  export default {
    data() {
      return {
        cruds: [],
        working: false
      }
    },
    methods: {
      create() {
        this.mute = true;
        window.axios.get(public_path + 'api/' + controller + '/create').then(({ data }) => {
          this.cruds.push(new Crud(data));
          this.mute = false;
        });
      },
      read() {
        this.mute = true;
        window.axios.get(public_path + 'api/' + controller).then(({ data }) => {
          data.forEach(crud => {
            this.cruds.push(new Crud(crud));
          });
          this.mute = false;
        });
      },
      update(id, color) {
        this.mute = true;
        window.axios.put(public_path + 'api/' + controller + `/${id}`, { color }).then(() => {
          this.cruds.find(crud => crud.id === id).color = color;
          this.mute = false;
        });
      },
      del(id) {
        this.mute = true;
        window.axios.delete(public_path + 'api/' + controller + `/${id}`).then(() => {
          let index = this.cruds.findIndex(crud => crud.id === id);
          this.cruds.splice(index, 1);
          this.mute = false;
        });
      }
    },
    watch: {
      mute(val) {
        document.getElementById('mute').className = val ? "on" : "";
      }
    },
    components: {
      TableComponent
    },
    created() {
      this.read();
    }
  }
</script>
<style>
    #app {
        margin-left: 1em;
    }
    .heading h1 {
        margin-bottom: 0;
    }
</style>