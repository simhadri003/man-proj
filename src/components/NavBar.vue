<template>
    <div class="navbar">
        <div class="navbar-header">Matches</div>
        <div class="navbar-header">Refrees</div>
    </div>
</template>

<script>
export default {
    name: 'NavBar',
}
</script>

<style scoped>
.navbar {
    width: 100%;
    background: purple;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    padding: 20px 40px;
}

.navbar-header {
    color: white;
    padding-right: 40px;
    font-size: 20px;
}
</style>