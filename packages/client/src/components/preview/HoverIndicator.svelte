<script>
  import { onMount, onDestroy } from "svelte"
  import IndicatorSet from "./IndicatorSet.svelte"
  import { builderStore, dndIsDragging } from "stores"

  let componentId
  $: zIndex = componentId === $builderStore.selectedComponentId ? 900 : 920

  const onMouseOver = e => {
    const element = e.target.closest(".interactive.component")
    const newId = element?.dataset?.id
    if (newId !== componentId) {
      componentId = newId
    }
  }

  const onMouseLeave = () => {
    componentId = null
  }

  onMount(() => {
    document.addEventListener("mouseover", onMouseOver)
    document.body.addEventListener("mouseleave", onMouseLeave)
  })

  onDestroy(() => {
    document.removeEventListener("mouseover", onMouseOver)
    document.body.removeEventListener("mouseleave", onMouseLeave)
  })
</script>

<IndicatorSet
  componentId={$dndIsDragging ? null : componentId}
  color="var(--spectrum-global-color-static-blue-200)"
  transition
  {zIndex}
/>
