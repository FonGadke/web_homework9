<template>
    <div>
        <button v-on:click="showFirst = !showFirst">
            Переключить
        </button>
        <transition name="fade">
            <p v-if="showFirst">привет</p>
        </transition>

        <button @click="showSecond = !showSecond">
            Переключить отрисовку
        </button>
        <transition name="slide-fade">
            <p v-if="showSecond">привет</p>
        </transition>

        <div id="what">
            <svg width="200" height="200">
                <polygon :points="points"></polygon>
                <circle cx="100" cy="100" r="90"></circle>
            </svg>
            <label>Sides: {{ sides }}</label>
            <input
                    type="range"
                    min="3"
                    max="500"
                    v-model.number="sides"
            >
            <label>Minimum Radius: {{ minRadius }}%</label>
            <input
                    type="range"
                    min="0"
                    max="90"
                    v-model.number="minRadius"
            >
            <label>Update Interval: {{ updateInterval }} milliseconds</label>
            <input
                    type="range"
                    min="10"
                    max="2000"
                    v-model.number="updateInterval"
            >
        </div>

    </div>
</template>

<script>
    export default {
        data: function () {
            var defaultSides = 10;
            var stats = Array.apply(null, { length: defaultSides })
                .map(function () { return 100 });
            return {
                stats: stats,
                points: generatePoints(stats),
                sides: defaultSides,
                minRadius: 50,
                interval: null,
                updateInterval: 500,
                showFirst: true,
                showSecond: true
            }
        },
        name: "Animation",
        watch: {
            sides: function (newSides, oldSides) {
                var sidesDifference = newSides - oldSides;
                if (sidesDifference > 0) {
                    for (var i = 1; i <= sidesDifference; i++) {
                        this.stats.push(this.newRandomValue())
                    }
                } else {
                    var absoluteSidesDifference = Math.abs(sidesDifference);
                    for (var j = 1; j <= absoluteSidesDifference; j++) {
                        this.stats.shift()
                    }
                }
            },
            stats: function (newStats) {
                TweenLite.to(
                    this.$data,
                    this.updateInterval / 1000,
                    { points: generatePoints(newStats) }
                );
            },
            updateInterval: function () {
                this.resetInterval()
            }
        },
        mounted: function () {
            this.resetInterval()
        },
        methods: {
            randomizeStats: function () {
                var vm = this;
                this.stats = this.stats.map(function () {
                    return vm.newRandomValue()
                })
            },
            newRandomValue: function () {
                return Math.ceil(this.minRadius + Math.random() * (100 - this.minRadius))
            },
            resetInterval: function () {
                var vm = this;
                clearInterval(this.interval);
                this.randomizeStats();
                this.interval = setInterval(function () {
                    vm.randomizeStats()
                }, this.updateInterval)
            }
        }
    }
    function valueToPoint (value, index, total) {
        var x     = 0;
        var y     = -value * 0.9;
        var angle = Math.PI * 2 / total * index;
        var cos   = Math.cos(angle);
        var sin   = Math.sin(angle);
        var tx    = x * cos - y * sin + 100;
        var ty    = x * sin + y * cos + 100;
        return { x: tx, y: ty }
    }

    function generatePoints (stats) {
        var total = stats.length;
        return stats.map(function (stat, index) {
            var point = valueToPoint(stat, index, total);
            return point.x + ',' + point.y
        }).join(' ')
    }
</script>

<style scoped>
    .fade-enter-active, .fade-leave-active {
        transition: opacity 0.5s;
    }
    .fade-enter, .fade-leave-to {
        opacity: 0;
    }

    .slide-fade-enter-active {
        transition: all .3s ease;
    }
    .slide-fade-leave-active {
        transition: all .8s cubic-bezier(1.0, 0.5, 0.8, 1.0);
    }
    .slide-fade-enter, .slide-fade-leave-to {
        transform: translateX(15px);
        opacity: 0;
    }

    button {
        display: block;
    }

    svg { display: block; }
    polygon { fill: #41B883; }
    circle {
        fill: transparent;
        stroke: #35495E;
    }
    input[type="range"] {
        display: block;
        width: 100%;
        margin-bottom: 15px;
    }

</style>