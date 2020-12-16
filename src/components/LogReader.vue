<template>
  <v-container style="max-width: 100%; padding: 0; background: #555;">
    <table class="log-container">
        <tr v-for="(l, i1) of log" :key="i1">
            <template v-if="l._rjlrType === 'log'">
                <td class="level" :class="`${l._rjlrLevel}${l._rjlrHighlighted ? ' highlighted': ''}`" @click="expandRow(i1, l)">
                    <v-icon v-if="l._rjlrExpanded" small>far fa-minus-square</v-icon>
                    <v-icon v-else small>far fa-plus-square</v-icon>
                    {{ l.level }}
                </td>
                <td v-for="(c, i2) of columns" :key="i2" :class="`${c.style} ${l._rjlrLevel}${l._rjlrHighlighted ? ' highlighted': ''}`">{{ l[c.key] }}</td>
            </template>
            <template v-else>
                <td class="level-details"></td>
                <td v-bind:colspan="columns.length"><pre>{{ l.data }}</pre></td>
            </template>
        </tr>
    </table>
  </v-container>
</template>

<script>
import { format } from "date-fns";

export default {
    name: 'LogReader',
    props: {
        quickSearch: {
            type: String,
            default: ""
        }
    },
    watch: { 
        quickSearch: function(newVal) {
            this.updateHightlight(newVal);
        }
    },
    data: () => ({
        log: [],
        displayedLogs: [],
        columns: [
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
                timestamp: "2020-12-03T09:55:10.825Z",
                ppid: 1256,
                sapConnectionId: 180,
                tgi: "S0076174",

            },
            {
                message: "WS Server has started on port 5021",
                level: "warning",
                timestamp: "2020-12-03T09:55:10.825Z",
                ppid: 1256,
                tgi: "S0076174",
            },
            {
                message: "WS Server has started on port 5021",
                level: "error",
                timestamp: "2020-12-03T09:55:10.825Z",
                ppid: 1256,
                sapConnectionId: 180,
                tgi: "S0076174",
            },
            {
                message: "WS Server has started on port 5021",
                level: "notice",
                timestamp: "2020-12-03T09:55:10.825Z",
                ppid: 1256,
                tgi: "S0076174",
            },
            {
                message: "WS Server has started on port 5021",
                level: "debug",
                timestamp: "2020-12-03T09:55:10.825Z",
                ppid: 1256,
                sapConnectionId: 180,
                tgi: "S0076174",
            },
        ]);

        }
    },
    methods: {
        parseLogs(log) {
            this.log = this.log.concat(log.map(l => {
                const result = { 
                    _rjlrType: "log",
                    _rjlrExpanded: false ,
                    _rjlrHighlighted: false,
                    ...l};
                for (const c of this.columns) {
                    if (c.format) {
                        result[c.key] = c.format(result[c.key]);
                    }
                    result._rjlrLevel = `sLevel-${l.level[0]}`;
                }
                return result;
            }));
        },

        updateHightlight(search) {
            if (search) {
                for (const l of this.log) {
                    const t = JSON.stringify(l);
                    l._rjlrHighlighted = t.indexOf(search) > -1;
                }
            } else {
                for (const l of this.log) {
                    l._rjlrHighlighted = false;
                }
            }
        },

        expandRow(index, logs) {
            this.log[index]._rjlrExpanded = !this.log[index]._rjlrExpanded;
            if (this.log[index]._rjlrExpanded) {
                const data = JSON.parse(JSON.stringify({ _rjlrType: "details", _rjlrExpanded: false, data: logs }));
                delete data.data._rjlrType;
                delete data.data._rjlrExpanded;
                delete data.data._rjlrHighlighted;
                delete data.data._rjlrLevel;
                this.log.splice(index+1, 0, data);
            } else {
                this.log.splice(index+1, 1);
            }
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

.control {
    width: 20px;
    text-align: center;
    background: rgba(0,0,0,0.5);
}

.level {
    width: 100px;
    background: rgba(0,0,0,0.5);
    color: rgba(255,255,255,0.5);
    border-right: 1px solid #555;

    .control {
        color: rgba(255,255,255,0.5)!important;
    }
}
.level:hover {
    cursor: pointer;
    color: rgba(255,255,255,0.7)!important;
}
.level-details {
    width: 100px;
    background: rgba(0,0,0,0.5);
}
.highlighted {
    background: rgba(255,255,0,0.5)!important;
}

.timestamp {
    width: 200px;
}
.tgi {
    width: 100px;
}


.sLevel-w {
    color: #fb8c00;
}
.sLevel-e {
    color: #ff5252;
}

</style>
