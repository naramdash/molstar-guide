---
# https://vitepress.dev/reference/default-theme-home-page
layout: home

hero:
  name: "Mol* Guide"
  text: "An Opinionated Guide to Mol*"
  image: 
    src: /molstar-logo.png
    alt: molstar
  tagline: For devs who struggled with Mol*, with my experiences
  actions:
    - theme: brand
      text: Markdown Examples
      link: /markdown-examples
    - theme: alt
      text: API Examples
      link: /api-examples

features:
  - title: Feature A
    details: Lorem ipsum dolor sit amet, consectetur adipiscing elit
  - title: Feature B
    details: Lorem ipsum dolor sit amet, consectetur adipiscing elit
  - title: Feature C
    details: Lorem ipsum dolor sit amet, consectetur adipiscing elit
---

<style module>
.imageContainer {
  margin-left: 20px !important;
}
.molstarContainer {
  width: calc(100% - 20px);
  height: 100%;
}
</style>

<script setup>
import { onMounted, useCssModule } from 'vue'

import { PluginContext } from 'molstar/lib/mol-plugin/context'
import { DefaultPluginSpec } from 'molstar/lib/mol-plugin/spec'

const classes = useCssModule()

const plugin = new PluginContext(DefaultPluginSpec())

onMounted(() => {
  const container = document.createElement('div')
  container.classList.add(classes.molstarContainer)

  const canvas = document.createElement('canvas')
  container.appendChild(canvas)

  const imageContainer = document.querySelector('div.image-container')
  imageContainer.classList.add(classes.imageContainer)
  imageContainer.replaceChildren(container)
  
  plugin.init()
    .then(() => plugin.initViewerAsync(canvas, container))
    .then((result) => { console.log('Initialize: ', result) })
    .then(() => 
      plugin.builders.data.download({
        url: '/1tqn.cif',
        label: "1TQN",
        isBinary: false,
      })
    ).then((data) =>
      plugin.builders.structure.parseTrajectory(data, 'mmcif')
    ).then((trajectory) => {
      plugin.builders.structure.hierarchy.applyPreset(
        trajectory,
        "all-models"
      )
    })
})
</script>