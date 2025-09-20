<template>
  <span
    class="nuxt-icon"
    :class="{ 'nuxt-icon--fill': !filled, 'nuxt-icon--stroke': hasStroke && !filled }"
    v-bind="spanAttrs"
    v-html="icon"
  />
</template>

<script setup lang="ts">
import { ref, watchEffect, computed, useAttrs } from 'vue'

defineOptions({
  inheritAttrs: false
})

const props = withDefaults(defineProps<{
  name: string;
  filled?: boolean;
  [key: string]: any;
}>(), { filled: false })

const attrs = useAttrs()

const spanAttrs = computed(() => {
  const spanAttrs: Record<string, any> = {}
  
  Object.keys(attrs).forEach(key => {
    if (!key.startsWith('icon-attr-')) {
      spanAttrs[key] = attrs[key]
    }
  })
  
  return spanAttrs
})

const icon = ref<string | Record<string, any>>('')
let hasStroke = false

/**
 * Extract icon-attr-* props and convert them to normal attributes
 */
const svgAttributes = computed(() => {
  const svgAttrs: Record<string, any> = {}
  
  const allAttrs = { ...props, ...attrs }
  
  Object.keys(allAttrs).forEach(key => {
    if (key.startsWith('icon-attr-')) {
      const attrName = key.replace('icon-attr-', '')
      svgAttrs[attrName] = allAttrs[key]
    }
  })
  
  return svgAttrs
})

/**
 * Function to add attributes to SVG element
 * @param svgString - The SVG string to add attributes to
 * @param attributes - The attributes to add to the SVG element
 */
function addAttributesToSvg(svgString: string, attributes: Record<string, any>): string {
  if (!svgString || Object.keys(attributes).length === 0) {
    return svgString
  }
  
  const attrsString = Object.entries(attributes)
    .map(([key, value]) => `${key}="${String(value)}"`)
    .join(' ')
  
  const svgTagMatch = svgString.match(/<svg([^>]*)>/)
  if (svgTagMatch) {
    const existingAttrs = svgTagMatch[1]
    const newSvgTag = `<svg${existingAttrs} ${attrsString}>`
    return svgString.replace(/<svg[^>]*>/, newSvgTag)
  }
  
  return svgString
}

/**
 * Function to get the icon
 */
async function getIcon () {
  try {
    const iconsImport = (import.meta as any).glob('assets/icons/**/**.svg', {
      eager: false,
      query: '?raw',
      import: 'default'
    })
    const rawIcon = await iconsImport[`/assets/icons/${props.name}.svg`]()
    if (rawIcon.includes('stroke')) { hasStroke = true }
    
    const processedIcon = addAttributesToSvg(rawIcon, svgAttributes.value)
    icon.value = processedIcon
  } catch {
    console.error(
      `[nuxt-icons] Icon '${props.name}' doesn't exist in 'assets/icons'`
    )
  }
}

await getIcon()

watchEffect(getIcon)
</script>

<style>
.nuxt-icon svg {
  width: 1em;
  height: 1em;
  margin-bottom: 0.125em;
  vertical-align: middle;
}
.nuxt-icon.nuxt-icon--fill,
.nuxt-icon.nuxt-icon--fill * {
  fill: currentColor !important;
}

.nuxt-icon.nuxt-icon--stroke,
.nuxt-icon.nuxt-icon--stroke *{
  stroke: currentColor !important;
}
</style>
