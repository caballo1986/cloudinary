<script lang="ts">
    import "two-up-element";
    import {originalImage} from './store';
</script>

<two-up>
    <img src={$originalImage} alt="Imagen original subiada por el usuario"/>
    <img src="" alt="Imagen sin fondo subida por el usuario"/>
</two-up>

<a download href='' class="block bg-blue-500 hover:bg-blue700 text-xl text-center w-full font-bold text-white rounded-full px4 py-2 mt-10">
    Descargar imagen sin fondo
</a>