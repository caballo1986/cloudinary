<script lang="ts">
    import "two-up-element";
    import {originalImage,modifiedImage} from './store';

    let processingImage = true;
    let tries = 0;
    let intevervalId: any;
    
    $: {
    if (processingImage){
            clearInterval(intevervalId);
            intevervalId = setInterval(() => {
                tries++
                const img = new Image();
                img.src = $modifiedImage;
                img.onload = () => {
                    processingImage = false;
                    clearInterval(intevervalId);
                }
            },500)
        }
    }
</script>

<two-up>
    <img src={$originalImage} alt="Imagen original subiada por el usuario"/>
    {#if processingImage}
        <div class="flex flex-col justify-center items-center">
            <div class="lds-ripple">
                <div></div>
                <div></div>
            </div>
            <p class="text-center mt-4">
                Procesando imagen...
            </p>
        </div>
    {:else}
        <img src={$modifiedImage} alt="Imagen modificada subiada por el usuario"/>
    {/if}
</two-up>

<a download href={$modifiedImage} 
    class="block bg-blue-500 hover:bg-blue700 text-xl text-center w-full font-bold text-white rounded-full px4 py-2 mt-10">
    Descargar imagen sin fondo
</a>