<script setup lang="ts">
import { ref, onMounted, onUnmounted, watch } from 'vue'
import * as d3 from 'd3'
import { feature } from 'topojson-client'

const props = defineProps({
  isDark: { type: Boolean, required: true }
})

const containerRef = ref<HTMLDivElement | null>(null)
const tooltipContent = ref({ name: '', description: '', visible: false })
const tooltipPosition = ref({ x: 0, y: 0 })

let svg = null
let projection = null
let path = null
let rotation = [0, -20]
let animationId = 0
let isDragging = false
let lastTime = 0

const visitedCountries = [
  { id: '156', name: 'China', description: 'Guangzhou - Grew up here', color: '#ef4444' },
  { id: '344', name: 'Hong Kong', description: 'Currently based', color: '#f59e0b' },
  {
    id: '840',
    name: 'United States',
    description: 'Massachusetts - University years',
    color: '#3b82f6'
  },
  { id: '392', name: 'Japan', description: 'Visited', color: '#ec4899' }
]

const darkModeColors = {
  land: '#374151',
  visited: {
    '156': '#f87171',
    '344': '#fbbf24',
    '840': '#60a5fa',
    '392': '#f472b6'
  },
  ocean: '#111827',
  graticule: '#374151',
  border: '#4b5563'
}

const lightModeColors = {
  land: '#e5e7eb',
  visited: {
    '156': '#ef4444',
    '344': '#f59e0b',
    '840': '#3b82f6',
    '392': '#ec4899'
  },
  ocean: '#f3f4f6',
  graticule: '#d1d5db',
  border: '#9ca3af'
}

function getColors() {
  return props.isDark ? darkModeColors : lightModeColors
}

async function initGlobe() {
  if (!containerRef.value) return

  const width = containerRef.value.clientWidth
  const height = containerRef.value.clientHeight
  const size = Math.min(width, height)

  d3.select(containerRef.value).select('svg').remove()

  svg = d3
    .select(containerRef.value)
    .append('svg')
    .attr('width', width)
    .attr('height', height)
    .attr('viewBox', `0 0 ${width} ${height}`)
    .style('cursor', 'grab')

  projection = d3
    .geoOrthographic()
    .scale(size / 2.2)
    .translate([width / 2, height / 2])
    .rotate(rotation)
    .clipAngle(90)

  path = d3.geoPath().projection(projection)

  const colors = getColors()

  svg
    .append('circle')
    .attr('class', 'ocean')
    .attr('cx', width / 2)
    .attr('cy', height / 2)
    .attr('r', size / 2.2)
    .attr('fill', colors.ocean)
    .attr('stroke', colors.border)
    .attr('stroke-width', 1)

  svg
    .append('path')
    .attr('class', 'graticule')
    .datum(d3.geoGraticule().step([30, 30]))
    .attr('d', path)
    .attr('fill', 'none')
    .attr('stroke', colors.graticule)
    .attr('stroke-width', 0.5)
    .attr('opacity', 0.3)

  try {
    const world = await d3.json('https://cdn.jsdelivr.net/npm/world-atlas@2/countries-110m.json')

    if (!world) return

    const countries = feature(world, world.objects.countries)

    svg
      .selectAll('.country')
      .data(countries.features)
      .enter()
      .append('path')
      .attr('class', 'country')
      .attr('d', path)
      .attr('fill', (d) => {
        const visited = visitedCountries.find((v) => v.id === String(d.id))
        if (visited) {
          return props.isDark ? darkModeColors.visited[visited.id] : visited.color
        }
        return colors.land
      })
      .attr('stroke', colors.border)
      .attr('stroke-width', 0.5)
      .style('cursor', (d) => {
        return visitedCountries.find((v) => v.id === String(d.id)) ? 'pointer' : 'grab'
      })
      .on('mouseenter', handleMouseEnter)
      .on('mousemove', handleMouseMove)
      .on('mouseleave', handleMouseLeave)

    const drag = d3
      .drag()
      .on('start', () => {
        isDragging = true
        svg.style('cursor', 'grabbing')
      })
      .on('drag', (event) => {
        rotation[0] -= event.dx * 0.5
        rotation[1] += event.dy * 0.5
        rotation[1] = Math.max(-90, Math.min(90, rotation[1]))
        projection.rotate(rotation)
        svg.selectAll('path').attr('d', path)
        svg.selectAll('.graticule').attr('d', path)
      })
      .on('end', () => {
        isDragging = false
        svg.style('cursor', 'grab')
      })

    svg.call(drag)
    animate()
  } catch (error) {
    console.error('Failed to load world data:', error)
  }
}

function animate(time) {
  time = time || 0
  if (!isDragging) {
    const delta = time - lastTime
    rotation[0] += delta * 0.005
    projection.rotate(rotation)
    svg.selectAll('path').attr('d', path)
    svg.selectAll('.graticule').attr('d', path)
  }
  lastTime = time
  animationId = requestAnimationFrame(animate)
}

function handleMouseEnter(event, d) {
  const visited = visitedCountries.find((v) => v.id === String(d.id))
  if (visited) {
    tooltipContent.value = {
      name: visited.name,
      description: visited.description,
      visible: true
    }
  }
}

function handleMouseMove(event) {
  if (tooltipContent.value.visible) {
    tooltipPosition.value = {
      x: event.clientX + 10,
      y: event.clientY + 10
    }
  }
}

function handleMouseLeave() {
  tooltipContent.value.visible = false
}

function updateColors() {
  if (!svg) return

  const colors = getColors()

  svg.select('.ocean').attr('fill', colors.ocean).attr('stroke', colors.border)

  svg.select('.graticule').attr('stroke', colors.graticule)

  svg.selectAll('.country').attr('fill', (d) => {
    const visited = visitedCountries.find((v) => v.id === String(d.id))
    if (visited) {
      return props.isDark ? darkModeColors.visited[visited.id] : visited.color
    }
    return colors.land
  })
}

watch(() => props.isDark, updateColors)

onMounted(() => {
  initGlobe()
  window.addEventListener('resize', initGlobe)
})

onUnmounted(() => {
  cancelAnimationFrame(animationId)
  window.removeEventListener('resize', initGlobe)
})
</script>

<template>
  <div class="relative w-full h-full">
    <div ref="containerRef" class="w-full h-full"></div>
    <div
      v-if="tooltipContent.visible"
      class="fixed z-50 px-3 py-2 text-sm rounded-lg shadow-lg pointer-events-none"
      :class="
        isDark
          ? 'bg-gray-800 text-gray-100 border border-gray-700'
          : 'bg-white text-gray-900 border border-gray-200'
      "
      :style="{ left: `${tooltipPosition.x}px`, top: `${tooltipPosition.y}px` }"
    >
      <p class="font-semibold">{{ tooltipContent.name }}</p>
      <p class="text-gray-500 dark:text-gray-400">{{ tooltipContent.description }}</p>
    </div>
  </div>
</template>
