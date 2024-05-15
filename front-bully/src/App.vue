<template>
  <div class="center">
    <NavbarMenu /> 
  </div>
  <div class="content">
    <NodeTable />
    <div class="space"></div>
    <LogList  />
  </div>
</template>

<script>
import NavbarMenu from './components/NavbarMenu.vue';
import NodeTable from  './components/NodeTable.vue';
import LogList from  './components/LogList.vue';

export default {
  name: 'App',
  components: {
    NavbarMenu, 
    NodeTable,
    LogList,
  },
  data() {
    return {
      
    };
  },
  methods: {
    detenerNodo(nodeId) {
      console.log(`Detener nodo con ID: ${nodeId}`);
    },
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.center {
  display: flex;
  justify-content: center; 
  align-items: center;
}

.content{
  padding: 3.5%;
  display: flex;
}

.space {
  width: 8%;
}
</style>
