<template>
  <div class="grid grid-cols-2 gap-x-4">
    <div>
      <h1>Don't</h1>
      <slot name="dont" />
    </div>
    <div>
      <h1>Do</h1>
      <slot name="doit" />
    </div>
  </div>
</template>