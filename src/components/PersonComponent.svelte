<script lang="ts">
    import { type SvelteWrapperProps } from "@visuallyjs/browser-ui-svelte"

    const {
        data, model, vertex
    }:SvelteWrapperProps = $props()

    function getImage() {
        return `/avatars/${data.img}`
    }
</script>

<img src={getImage()} alt={data.name}/>
<div>
    <strong>{data.name}</strong>
    <span>{data.title}</span>
</div>
