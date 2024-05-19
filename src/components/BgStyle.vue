<script setup>
</script>

<template>
    <div class="container">
        <div class="ducktape">
            <img src="../assets/images/cutting area.svg" alt="tesoura seguindo linha pontilhada">
        </div>
    </div>
    <div class="container__avisos">
        <img src="../assets/images/fragile.png" alt="simbolo de conteudo fragil" class="aviso">
        <img src="../assets/images/setas.png" alt="duas setas para cima" class="aviso">
    </div>
</template>

<style scoped>
.container {
    display: flex;
    justify-content: center;
    align-items: center;
    height: calc(80vh - 215px);
    width: 100vw;
}

.ducktape {
    height: 150px;
    width: 100%;
    background-color: #D9D9D9;
    display: flex;
    justify-content: center;
    align-items: center;
}

.ducktape img {
    width: 100%;
    height: auto;
}

.container__avisos {
    position: fixed;
    bottom: 0;
    width: 100vw;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 20px;
}

.container__avisos img.aviso {
    width: 175px;
    height: 175px;
    margin: 0 20px;
}
</style>
