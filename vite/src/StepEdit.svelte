<script lang="ts">
    import "two-up-element";
    import {originalImage,modifiedImage} from './store';
    let processingImage = true;
    let tries = 0;
    let intevervalid: number;
    $: {
    if (processingImage){
            clearInterval(intevervalid);
            intevervalid = setInterval(() => {
                tries++;}
                ,500)
        }
    }
</script>

<two-up>
    <img src={$originalImage} alt="Imagen original subiada por el usuario"/>
    <img 
        on:load={() => (processingImage = false)}
        on:error={() => (processingImage = false)}
        src={`${$modifiedImage}$t=${tries}`}
        alt="Imagen sin fondo subida por el usuario"/>
</two-up>

<a download href={$modifiedImage} class="block bg-blue-500 hover:bg-blue700 text-xl text-center w-full font-bold text-white rounded-full px4 py-2 mt-10">
    Descargar imagen sin fondo
</a>