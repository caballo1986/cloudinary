<script lang="ts">
  import {Cloudinary} from '@cloudinary/url-gen';
  import {backgroundRemoval} from '@cloudinary/url-gen/actions/effect';
  import {ImageStatus} from "../types.d";
  import {imageStatus, modifiedImage, originalImage} from "./store";
  import Dropzone from 'dropzone';
  import 'dropzone/dist/dropzone.css';
  import {onMount} from 'svelte';

  const cloudinary = new Cloudinary({
    cloud:{
        cloudName: 'caballo'
    },
    url: {
        secure: true
    }
  })

  onMount(() => {
    const dropzone = new Dropzone('#dropzone',{
        uploadMultiple: false,
        acceptedFiles: '.jpg,.png,.webp',
        maxFiles: 1
    });
    dropzone.on('sending', (file, xhr, formData) => {
        imageStatus.set(ImageStatus.UPLOADING);
        //aqui podemos añadir la apikey, configuracion
        formData.append('upload_preset', 'default');
        formData.append('timestamp', Date.now()/1000);
        formData.append('api_key', '451584892192498');
    })
    dropzone.on('success', (file, response) => {
        const {public_id: publicId, secure_url : url} = response;
        const imageWithoutBackground = cloudinary
            .image(publicId)
            .effect(backgroundRemoval())

        imageWithoutBackground.toURL();
        console.log("🚀 ~ file: StepUpload.svelte:38 ~ dropzone.on ~ imageWithoutBackground.toUrl();:", imageWithoutBackground.toURL());
        imageStatus.set(ImageStatus.DONE);
        modifiedImage.set(imageWithoutBackground.toURL());
        originalImage.set(url); 
        //crear un fondo transprarente y guardar en el backgroundImage
        console.log("🚀 HA SALIDO BIEN");
        console.log("🚀 ~ file: StepUpload.svelte:18 ~ dropzone.on ~ response:", response)
    })
    dropzone.on('error', (file, response) => {
        console.log("🚀 HA SALIDO MAL"); 
        console.log("🚀 ~ file: StepUpload.svelte:22 ~ dropzone.on ~ response:", response)
    })
  })
</script>
<form id="dropzone" class="shadow-2x1 border-dashed border-2 border-gray-300 rounde-lg aspect-video flex items-center justify-center flex-col" action="https://api.cloudinary.com/v1_1/caballo/image/upload">
    {#if $imageStatus == ImageStatus.READY}
    <button class="font-bold pointer-events-none bg-blue-600 rounded-full text-white text-xl px-6 py-4" >
        Upload files
    </button>
    <strong class="text-lg mt-4 text-gray-800">or Drop a file</strong>
    {:else if $imageStatus == ImageStatus.UPLOADING}
    <strong class="text-lg mt-4 text-gray-800">Uploading files..</strong>
    {/if}

</form>
