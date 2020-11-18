<template>
<div class="base-card">
    <slot></slot>
</div>
</template>
<style scoped>
.base-card{
    display: flex;
    flex-direction: column;
    margin: 5px;
    box-shadow: 5px 5px 10px 0 #dddada;
    border-radius: 5px;
}
</style>