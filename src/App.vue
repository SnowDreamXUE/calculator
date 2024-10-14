<template>
    <div id="app">
        <div class="cat"/>
        <Calculator/>
    </div>
</template>

<script>
import Calculator from './components/calculator'

export default {
    name: 'app',
    components: {
        Calculator
    }
}
</script>

<style>
.cat {
    position: absolute;
    left: 0;
    bottom: 0;
    width: 219px;
    height: 273px;
    background-image: url(https://www.helloimg.com/i/2024/09/30/66fa5068affb5.png);
}
body {
    background-color: #d1e8f1;
}
#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    text-align: center;
    color: #2c3e50;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
}
</style>
