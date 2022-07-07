<script setup lang='ts'>
import { noise } from '~/logics/noise'
import { isDark } from '~/logics'

const r180 = Math.PI
const r360 = 2 * r180
const width = window.innerWidth
const height = window.innerHeight
const el = $ref<HTMLCanvasElement | null>(null)

const N_PARTICLES = 3000
const N_COLORS = 12
const particles: Particle[][] = []

const palette = !isDark.value
  ? [[27, 27, 27]]
  : [
    [254, 242, 145],
    [253, 198, 103],
    [182, 245, 200],
    [84, 146, 76],
    [221, 124, 81],
    [253, 158, 149],
    [112, 184, 214],
  ]

// return 0 - n
const random = (n: number) => {
  return Math.random() * n
}

let y2 = 0
let _gaussian_previous = false
const randomGaussian = (mean: number, std = 1) => {
  let y1, x1, x2, w
  if (_gaussian_previous) {
    y1 = y2
    _gaussian_previous = false
  }
  else {
    do {
      x1 = random(2) - 1
      x2 = random(2) - 1
      w = x1 * x1 + x2 * x2
    } while (w >= 1)
    w = Math.sqrt(-2 * Math.log(w) / w)
    y1 = x1 * w
    y2 = x2 * w
    _gaussian_previous = true
  }
  const m = mean || 0
  return y1 * std + m
}

const map = (
  n: number,
  start1: number,
  stop1: number,
  start2: number,
  stop2: number,
) => {
  return (n - start1) / (stop1 - start1) * (stop2 - start2) + start2
}

class Particle {
  x: number
  y: number
  altitude: number
  val: number
  angle: number
  constructor(x: number, y: number, phi: number) {
    this.x = x
    this.y = y
    this.altitude = 0
    this.val = 0
    this.angle = phi
  }

  update(index: number) {
    this.x += Math.cos(this.angle)
    this.y += Math.sin(this.angle)

    const nx
      = 1.1
      * map(this.y, 0, height, 4, 0.2)
      * map(this.x, 0, width, -1, 1)
    const ny
      = 3.1
      * map(this.y, 0, height, 4, 0.2)
      * map(this.y, 0, height, -1, 1)

    this.altitude = noise(nx + 400, ny - 200)
    this.val
      = (this.altitude + 0.035 * (index - N_COLORS / 2)) % 1
    this.angle += 3 * map(this.val, 0, 1, -1, 1)
  }

  display(point: Function) {
    if (this.val > 0.485 && this.val < 0.515) {
      point(
        this.x,
        this.y
        + 50
        - this.altitude * 100 * map(this.y, 0, height, 0.2, 4),
      )
    }
  }
}

onMounted(() => {
  const canvas = el!
  const ctx = canvas.getContext('2d')!
  canvas.style.width = `${width}px`
  canvas.style.height = `${height}px`
  canvas.width = width
  canvas.height = height

  for (let i = 0; i < N_COLORS; i++) {
    const ps = []
    for (let j = 0; j < N_PARTICLES; j++) {
      ps.push(new Particle(
        randomGaussian(width / 2, 150),
        randomGaussian(height / 2, 150),
        random(r360),
      ))
    }
    particles.push(ps)
  }

  let iterations = 0
  let lastTime = performance.now()
  const N_ITER = 400
  const interval = 1000 / 40

  const point = (x: number, y: number) => {
    ctx.fillRect(x, y, 1, 1)
  }

  let controls: ReturnType<typeof useRafFn>

  const frame = () => {
    if (performance.now() - lastTime < interval)
      return

    if (iterations >= N_ITER) {
      controls.pause()
      return
    }

    iterations += 1
    lastTime = performance.now()

    particles.forEach((ps, idx) => {
      const [r, g, b] = palette[idx % palette.length]
      ctx.fillStyle = `rgba(${r},${g},${b}, 0.1)`
      ps.forEach((p) => {
        p.update(idx)
        p.display(point)
      })
      ctx.stroke()
    })
  }

  controls = useRafFn(frame, { immediate: true })
})
</script>

<template>
  <div fixed left-0 right-0 bottom-0 pointer-event-none class="-z-1; top-1/3">
    <canvas ref="el" width="400" height="400" />
  </div>
</template>
