<template>
  <div id="app">
    <h1>Multimon monitor calculator</h1>
    <a href="https://github.com/nilshellerhoff/multimon"><h3>View on Github</h3></a>
    <div class="monitor-container">
      <table>
        <tr>
          <td v-for="monitor in monitors" :key="monitor.id">
            <Monitor
              :width="monitor.width"
              :height="monitor.height"
              :diagonal="monitor.diagonal"
              :scalingFactor=scalingFactor
              @remove="removeMonitor(monitor.id)"
              @move-left="moveMonitorLeft(monitor.id)"
              @move-right="moveMonitorRight(monitor.id)"
            />
          </td>
        </tr>
      </table>
      <div style="text-align: center">
        <button @click="addMonitor" style="display: inline">add monitor</button>
      </div>
    </div>
  </div>
</template>

<script>
import Monitor from "./components/Monitor.vue";

export default {
  name: "App",
  components: {
    Monitor,
  },
  data: () => ({
    monitors: [
      {
        id: 1,
        width: 1920,
        height: 1080,
        diagonal: 24,
      },
    ],
    scalingFactor: 4
  }),
  computed: {
    maxId() {
      return Math.max(...this.monitors.map((monitor) => monitor.id));
    },
  },
  methods: {
    addMonitor() {
      this.monitors.push({
        id: this.maxId + 1,
        width: 1920,
        height: 1080,
        diagonal: 24,
      });
    },
    removeMonitor(id) {
      this.monitors = this.monitors.filter((monitor) => monitor.id != id);
    },

    moveMonitor(id, places) {
      const idx = this.monitors.findIndex(m => m.id == id)
      if (idx + places < 0 || idx + places >= this.monitors.length) return
      const monitor = this.monitors[idx]
      this.monitors = this.monitors.filter(m => m.id != id)
      this.monitors.splice(idx + places, 0, monitor)
    },
        moveMonitorLeft(id) {
      this.moveMonitor(id, -1)
    },
    moveMonitorRight(id) {
      this.moveMonitor(id, 1)
    }
  },
};
</script>

<style>
#app {
  font-family: Arial, sans-serif;
  font-size: 13px;
}
h1, h3 {
  text-align: center;
}
button {
  font-family: Arial, sans-serif;
  font-size: 13px;
}
.monitor-container {
  display: inline-block;
  white-space: nowrap;
  overflow: hidden;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
</style>
