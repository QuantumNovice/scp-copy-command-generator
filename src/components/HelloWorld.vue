<template>
  <div class="container" :class="{ dark: isDarkTheme }">
    <h1>SCP Command Generator</h1>
    <div class="theme-toggle">
      <button @click="toggleTheme">
        {{ isDarkTheme ? "Light Theme" : "Dark Theme" }}
      </button>
    </div>
    <div class="input-group">
      <label for="local-dir">Local Directory:</label>
      <input type="text" id="local-dir" v-model="localDir">
    </div>
    <div class="input-group">
      <label for="remote-dir">Remote Directory:</label>
      <input type="text" id="remote-dir" v-model="remoteDir">
    </div>
    <div class="input-group">
      <label for="server-ip">Server IP:</label>
      <input type="text" id="server-ip" v-model="serverIP">
    </div>
    <div class="input-group">
      <label for="remote-username">Remote Username:</label>
      <input type="text" id="remote-username" v-model="remoteUsername">
    </div>
    <!--
    <div class="input-group">
      <label for="remote-password">Remote Password:</label>
      <input type="password" id="remote-password" v-model="remotePassword">
    </div>
    -->
    <div class="output-box">
      <pre>{{ scpCommand }}</pre>
    </div>
    <button @click="generateCommand">Generate SCP Command</button>
    <div v-if="error">{{ error }}</div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      localDir: "",
      remoteDir: "",
      serverIP: "",
      remoteUsername: "",
      remotePassword: "",
      scpCommand: "",
      error: "",
      isDarkTheme: false, // Initial theme state
    };
  },
  methods: {
    generateCommand() {
      const { localDir, remoteDir, serverIP, remoteUsername } = this;
      if (!localDir || !remoteDir || !serverIP || !remoteUsername ) {
        this.error = "Please fill in all fields.";
        return;
      }

      this.error = ""; // Clear previous error message
      this.scpCommand = `scp -r ${remoteUsername}@${serverIP}:${localDir} ${remoteDir}`;
    },
    toggleTheme() {
      this.isDarkTheme = !this.isDarkTheme;
    },
  },
};
</script>

<style scoped>
.container {
  border: 1px solid #ddd;
  border-radius: 5px;
  padding: 10px;
  margin-bottom: 15px;
  background-color: #1d2437; /* Light theme default */
  color: #333; /* Light theme default text color */
}

.container.dark {
  background-color: #1e2045; /* Dark theme background color */
  color: #fff; /* Dark theme text color */
}

.input-group {
  display: flex;
  align-items: center;
  margin-bottom: 5px;
}

.input-group label {
  flex: 1;
}

.output-box {
  background-color: #eee;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-family: monospace;
  white-space: pre-wrap;
  word-break: break-all;
}

.error {
  color: red;
  font-weight: bold;
  margin-top: 10px;
}

.theme-toggle {
  text-align: right;
  margin-bottom: 10px;
}
</style>
