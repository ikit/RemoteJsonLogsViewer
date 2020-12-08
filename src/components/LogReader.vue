<template>
  <v-container style="max-width: 100%; padding: 0; background: #555;">
    <table class="log-container">
        <tr v-for="(l, i1) of log" :key="i1">
            <td v-for="(c, i2) of columns" :key="i2" :class="`${c.style} ${l.sLevel}`">{{ l[c.key] }}</td>
        </tr>
    </table>
  </v-container>
</template>

<script>
import { format } from "date-fns";

export default {
    name: 'LogReader',

    data: () => ({
        log: [],
        columns: [
            { key: "level", style: "level", format: null, },
            { key: "timestamp", style: "timestamp", format: (d) => format(new Date(d), "yyyy.MM.dd HH:mm:ss") },
            { key: "message", style: "message" },
        ],

    }),
    mounted() {
        for (let i = 0; i < 100; i++) {

        this.parseLogs([
            {
                message: "WS Server has started on port 5021",
                level: "info",
                timestamp: "2020-12-03T09:55:10.825Z"
            },
            {
                message: "WS Server has started on port 5021",
                level: "warning",
                timestamp: "2020-12-03T09:55:10.825Z"
            },
            {
                message: "WS Server has started on port 5021",
                level: "error",
                timestamp: "2020-12-03T09:55:10.825Z"
            },
            {
                message: "WS Server has started on port 5021",
                level: "info",
                timestamp: "2020-12-03T09:55:10.825Z"
            },
            {
                message: "WS Server has started on port 5021",
                level: "info",
                timestamp: "2020-12-03T09:55:10.825Z"
            },
        ]);

        }
    },
    methods: {
        parseLogs(log) {
            this.log = this.log.concat(log.map(l => {
                const result = { ...l };
                for (const c of this.columns) {
                    if (c.format) {
                        result[c.key] = c.format(result[c.key]);
                    }
                    result.sLevel = `sLevel-${l.level[0]}`;
                }
                return result;
            }));
        }

    }
}
</script>

<style lang="scss" scoped>

.log-container {
    width: 100%;
    border-spacing: 0;
    max-width: 100%;
    font-family: monospace;
    td { 
        padding-left: 5px;
    }
}

.level {
    width: 75px;
    background: rgba(0,0,0,0.5);
    color: rgba(255,255,255,0.5);
    border-right: 1px solid #555;
}
.timestamp {
    width: 200px;
}


.sLevel-w {
    color: #fb8c00;
}
.sLevel-e {
    color: #ff5252;
}

</style>
