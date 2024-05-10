<template>
  <div class="center">
    <NavbarMenu /> 
  </div>
  <div class="content">
    <NodeTable :nodes="nodos" @detener-nodo="detenerNodo" />
    <div class="space"></div>
    <LogList :logs="exampleLogs" />
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
      nodos: [
        {
          id: 1,
          ip: '192.168.0.1',
          isActive: true,
          isLeader: true,
        },
        {
          id: 2,
          ip: '192.168.0.2',
          isActive: true,
          isLeader: false,
        },
        {
          id: 3,
          ip: '192.168.0.3',
          isActive: false,
          isLeader: false,
        },
        {
          id: 4,
          ip: '192.168.0.2',
          isActive: true,
          isLeader: false,
        },
        {
          id: 5,
          ip: '192.168.0.3',
          isActive: false,
          isLeader: false,
        },{
          id: 4,
          ip: '192.168.0.2',
          isActive: true,
          isLeader: false,
        },
        {
          id: 5,
          ip: '192.168.0.3',
          isActive: false,
          isLeader: false,
        },{
          id: 4,
          ip: '192.168.0.2',
          isActive: true,
          isLeader: false,
        }
      ],
      exampleLogs: [
        "Log 1",
        "Log 2",
        "Log 3",
        "Log 1",
        "Log 2",
        "Log 3",
        "Log 1",
        "Log 2",
        "Log 3",
        "Log 1",
        "Log 2",
        "Log 3",
        "Log 1",
        "Log 2",
        "Log 3",
        "Log 1",
        "Log 2",
        "Log 3",
        "Log 1",
        "Log 2",
        "Log 3",
        "Log 1",
        "Log 2",
        "Log 3",
      ]
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
