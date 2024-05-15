<template>
    <div class="log-list">
        <h3 class="log-title">System Logs</h3>
        <ul class="log-container" ref="logContainer" v-if="logs.length > 0">
            <li v-for="(log, index) in logs" :key="index" class="log-item">
                {{ log.message }}
            </li>
        </ul>
        <p v-else>No logs available.</p>
    </div>
</template>

<script>
import io from 'socket.io-client';

export default {
    name: 'LogList',
    data() {
        return {
            logs: [],
            socket: null,
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
        this.socket.on('logs', (newLog) => {
            console.log('Received new log:', newLog);
            this.logs.push(newLog);
            this.$nextTick(() => {
                this.scrollToBottom();
            });
        });
    },
    methods: {
        scrollToBottom() {
            const logContainer = this.$refs.logContainer;
            if (logContainer) {
                logContainer.scrollTop = logContainer.scrollHeight;
            }
        },
    },
};
</script>

<style scoped>
.log-list {
    background-color: rgb(0, 0, 0);
    padding: 20px;
    border-radius: 10px;
    width: 100%;
    max-height: 500px;
    overflow-y: auto;
}

.log-title {
    color: #45a049;
}

.log-item {
    color: rgb(255, 255, 255);
    margin-bottom: 5px;
}
</style>