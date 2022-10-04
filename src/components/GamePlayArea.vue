<script lang="ts">
    export default {
        data() {
            return {
                words: "Things to type here"
            }
        }
    }
</script>

<template>
    <div class="gameplaycontainer">
        <p class="gameplay">{{ words }}</p>
    </div>
</template>

<style>

    .gameplaycontainer {
        margin: 0;
        padding: 0;
        height: 60vh;
        width: 60vw;
        margin-top: 15vh;
        margin-left: 15vw;
        background-color: gray;
        border-radius: 40px;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .gameplay {
        padding: 15px;
        color: white; 
    }
</style>