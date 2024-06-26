<template>
    <div class="node-table">
        <h3>Distributed System Node List</h3>
        <div class="table-container">
            <div class="node-row node-header">
                <div class="node-cell">ID</div>
                <div class="node-cell">IP</div>
                <div class="node-cell">Status</div>
                <div class="node-cell">Leader</div>
                <div class="node-cell">Action</div>
            </div>

            <div v-for="(node, index) in uniqueNodes" :key="index" class="node-row">
                <div class="node-cell">{{ node.id }}</div>
                <div class="node-cell">{{ node.clientUrl }}</div>
                <div class="node-cell">
                    <div :class="node.isActive ? 'status-indicator-active' : 'status-indicator-inactive'"
                        class="status-indicator" role="status" aria-label="Status indicator"></div>
                </div>
                <div class="node-cell">
                    <img v-if="node.imLeader" src="@/assets/crown_king.jpg" alt="Leader icon" class="leader-icon">
                    <img v-else src="@/assets/peon.jpg" alt="Non-leader icon" class="leader-icon">
                </div>
                <div class="node-cell">
                    <div v-if="node.isActive">
                        <button class="stop-button" @click="detenerNodo(node.id)">Stop</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import io from 'socket.io-client';

export default {
    name: 'NodeTable',
    data() {
        return {
            receivedData: null,
            nodes: []
        };
    },
    mounted() {
        const clientUrl = window.location.href;
        this.socket = io('http://localhost:4000', { query: { clientUrl } });

        this.socket.on('connect', () => {
            console.log('Connected to server ws');
            this.isConnected = true;
        });
        this.socket.on('disconnect', () => {
            console.log('Disconnected from server ws');
            this.isConnected = false;
            this.servers = [];
        });

        // Escuchar el evento enviado desde el servidor
        this.socket.on('node_data_to_vue', (data) => {
            console.log('Datos recibidos en Vue:', data);
            data.forEach(newNode => {
                const existingNodeIndex = this.nodes.findIndex(node => node.clientUrl === newNode.clientUrl);
                if (existingNodeIndex !== -1) {
                    // Actualizar el estado del nodo existente
                    this.nodes[existingNodeIndex] = newNode;
                } else {
                    // Agregar el nuevo nodo a la lista
                    this.nodes.push(newNode);
                }
            });
        });

    },
    computed: {
        // Filtrar nodos únicos por clientUrl
        uniqueNodes() {
            const unique = {};
            return this.nodes.filter(node => {
                if (!unique[node.clientUrl]) {
                    unique[node.clientUrl] = true;
                    return true;
                }
                return false;
            });
        }
    },
    methods: {
        detenerNodo(nodeId) {
            this.socket.emit('stop_node', nodeId);
        },
    },
};
</script>


<style scoped>
.node-table {
    border: 2px solid #dcdcdc;
    padding: 20px;
    border-radius: 10px;
    background-color: #ffffff;
    width: 100%;

    margin: 0;
}

.table-container {
    max-height: 400px;
    overflow-y: auto;
}

.node-row {
    display: flex;
    justify-content: space-between;
    padding: 10px;
    border-bottom: 1px solid #cfc6c6;
}

.node-header {
    font-weight: bold;
    color: #000000;
}

.node-cell {
    flex: 1;
    text-align: center;
}

.status-indicator {
    width: 16px;
    height: 16px;
    border-radius: 50%;
    border: 1px solid white;
    margin: auto;
}

.status-indicator-active {
    background-color: green;
}

.status-indicator-inactive {
    background-color: red;
}

.stop-button {
    background-color: #c3c3c3;
    color: white;
    padding: 8px 12px;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

.stop-button:hover {
    background-color: #c9302c;
}

.leader-icon {
    width: 32px;
    height: 32px;
}

.node-row:hover {
    background-color: #f7f7f7;
}
</style>