<template>
  <div class="clock" id="clock" :style="{width: width + 'px', height: width + 'px'}">
    <div class="clock-container">
      <span class="time" id="H">{{H}}:</span>
      <span class="time" id="M">{{M}}:</span>
      <span class="time" id="S">{{S}}</span>
    </div>
  </div>
</template>

<script>
  export default {
    props: {
      width: {
        type: Number,
        default: 400
      },
      spacing: {
        type: Number,
        default: 12
      },
      lineWidth: {
        type: Number,
        default: 10
      }
    },
    data () {
      return {
        H: null,
        M: null,
        S: null
      }
    },
    mounted () {
      let c = this.$el.appendChild(document.createElement('canvas'))
      let w = this.width
      let h = this.width

      c.width = w
      c.height = h

      let ctx = c.getContext('2d')
      // degrees to radians conversion function
      let deg2rad = d => (Math.PI / 180) * d

      // constructor for the circles
      function RadialBar (x, y, r, max, value) {
        var self = this
        self.X = x || 0
        self.Y = y || 0
        self.R = r || 0
        self.MAX = max || 1
        self.value = value || 0
        self.target = value || 0

        self.update = function (v) {
          // lerp
          self.target = v > 0 ? v : 0.1
          self.value += (self.target - self.value) * 0.05

          ctx.arc(
            this.X,
            this.Y,
            this.R,
            // move starting point to top
            deg2rad(-90),
            deg2rad(-90) + (deg2rad(360) * (self.value / self.MAX))
          )
        }
      }

      ctx.lineWidth = 3
      ctx.lineCap = 'round'
      const lineWidth = this.lineWidth
      const spacing = this.spacing
      let radius = this.width / 2 - lineWidth
      // contains values for hours, minutes, seconds
      let times
      // create circles
      let circles = {
        H: new RadialBar(w / 2, h / 2, radius, 12, 0),
        M: new RadialBar(w / 2, h / 2, radius - spacing, 60, 0),
        S: new RadialBar(w / 2, h / 2, radius - spacing - spacing, 60, 0)
      }
      const _this = this
      // update time values and text once per second
      function updateTime () {
        var time = new Date()
        var hours = time.getHours()
        if (hours > 12) {
          hours -= 12
        } else if (hours === 0) {
          hours = 12
        }
        times = {
          H: hours,
          M: time.getMinutes(),
          S: time.getSeconds()
        }

        // update text
        Object.keys(times).forEach(k => {
          _this[k] =
          // pad with 0s if needed
          String(times[k]).length > 1 ? times[k] : times[k] = '0' + times[k]
        })

        setTimeout(updateTime, 1000)
      }

      function draw () {
        ctx.clearRect(0, 0, w, h)

        // update circles, set their color, draw
        Object.keys(circles).forEach((k, i) => {
          ctx.beginPath()
          // ctx.setLineDash([.5, 15])
          circles[k].update(times[k])
          ctx.strokeStyle = 'hsl(' + k.charCodeAt(0) * i + ', 50%, 50%)'
          ctx.stroke()
          ctx.lineWidth = lineWidth
        })

        requestAnimationFrame(draw)
      }

      // start
      updateTime()
      draw()
    }
  }
</script>

<style scoped>
  .clock {
    position: relative;
  }
  .clock-container {
    position: absolute;
    left: 0;
    right: 0;
    top: 0;
    bottom: 0;
    margin: auto;
    height: 40px;
    display: flex;
    width: 150px;
    font-size: 2rem;
  }
  .clock-container span {
    width: 50px;
    text-align: center;
  }
</style>
