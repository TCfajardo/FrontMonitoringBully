<template>
    <div class="menu">
        <div class="algoritmo-nombre">
            <h2>Bully Algorithm Implementation</h2> 
        </div>
        <div class="botones">
            <button @click="crearNodo">Add Node</button>
        </div>
    </div>
</template>

<script>
import axios from 'axios';
import io from 'socket.io-client';

export default {
    name: 'NavbarMenu',
    methods: {
        async crearNodo() {
            try {
                const response = await axios.post('http://localhost:4000/crear-nuevo-nodo');
                console.log(response.data);
            } catch (error) {
                console.error('Error al crear el nuevo nodo:', error.message);
            }
            this.$emit('crear-nodo');
        },
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
        });
    },
};
</script>

<style scoped>
.menu {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 30px;
    background-color: #ffffff;
    border-bottom: 2px solid #e0e0e0;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    width: 90%;
    margin-top: -40px;
}

.algoritmo-nombre {
    flex-grow: 1;
    text-align: center;
    margin-left: 90px;
}

h2 {
    font-size: 1.5em;
    color: #333;
    margin: 0;
}

.botones {
    display: flex;
    gap: 10px;
    margin-right: 40px;
}

button {
    padding: 12px 18px;
    background-color: #000000;
    font-size: 1em;
    color: white;
    border: none;
    border-radius: 5px;
    cursor: pointer;
}

button:hover {
    background-color: #45a049;
}
</style>
