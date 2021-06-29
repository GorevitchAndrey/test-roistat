<template>
  <div>
    <button>
      <slot />
    </button>
  </div>
</template>

<script>
export default {
  name: 'ButtonComponent',
}
</script>

<style>
button {
  cursor: pointer;
}
</style>
